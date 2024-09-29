Link: https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf

## Introduction

### What is Bigtable ?
It is a **distributed storage system** for managing **structured data** that is designed to scale at very large size: ==petabytes of data across thousands of commodity servers.==

### Goals
1. Wide applicability
2. Scalability
3. Availability
4. High performance

### Other requirements
Handle a variety of demanding workloads, ranging from ==throughput-oriented batch-processing== to ==latency-sensitive serving of data to end users==.

Allow clients to have control over 
1. Data Layout and format
2. Locality properties of the data represented in underlying storage.
3. Whether to serve data out of memory or disk

## Data Model

BigTable is a **sparse**, **distributed**, **persistent**, **multi-dimensional sorted map**.
 
 (row: string, column: string, time: int64) → string

### Rows
- Every read or write of data under a single row key is **atomic**. 
	- More predictable behavior in presence of concurrent updates to the same row.

- Maintains data in lexicographic order by row key.
- Row range for a table is dynamically partitioned into ranges called **tablets**.
	- Tablets:
	1. distribute the data
	2. help in load balancing.
- Therefore, reads of short row ranges are efficient(require communication with smaller number of machines)
- Clients should select their row keys to get good locality for their data access pattern.
	- **Example**: Webpages in same domain grouped into contiguous rows.
### Column Families

Column keys are grouped into sets called **column families**. Columns under a family are logically related and stored together.

A column family ==must be created before data can be stored under any column key in that family==; after a family has been created, any column key within the family can be used.

**Intent**: Small number of rarely changing column families, but unbounded number of columns.

Column key is named using syntax “family:qualifier”\

Access control and both disk and memory accounting are performed at the column-family level. So, ==All data in a column family is stored together on disk, which can optimize read performance when accessing related data==.

### Timestamps
Each cell in a BigTable can contain multiple versions of the same data; these versions are indexed by timestamp.

BigTable timestamps are 64-bit integers. They can be assigned in 2 ways: 
1. Assigned by Bigtable
	1. “Real time” in microseconds
2. Explicity assigned by client applications.

Different versions of a cell are ==stored in decreasing timestamp order, so that the most recent versions can be read first==.

**How to reduce burden of managing versioned data?**

Support for two per-column-family settings that tell BigTable to ==garbage-collect cell versions automatically==. 

1. Only the last $n$ versions of a cell be kept
2. Only new enough versions be kept(e.g. values written in past 7 days)

## API
The Bigtable API provides functions for 
1. Creating tables
2. Deleting tables
3. Creating column families
4. Deleting column families
5. Changing cluster metadata
6. Changing table metadata
7. Changing column family metadata

Client applications can: 
1. Write/Delete values
2. Look up values from individual rows
3. Iterate over a subset of the data in a table.

### Other features

1. Single-row transactions
2. Use cells as integer counters
3. Execute client-supplied scripts in the address spaces of the servers

## Building blocks

### Cluster Infra
Bigtable is built on several other pieces of Google infrastructure. 
1. Bigtable uses the distributed **Google File System** (GFS) to store log and data files.
2. A Bigtable cluster typically operates in a shared pool of machines that run a wide variety of other distributed applications, and ==Bigtable processes often share the same machines with processes from other applications.==
3. Bigtable depends on a cluster management system for 
	1. scheduling jobs, 
	2. managing resources on shared machines, 
	3. dealing with machine failures
	4. monitoring machine status.

### Disk Management

The Google *SSTable*(**Sorted String Table**) file format is used internally to store BigTable data. 

SSTable → **Persistent**, **ordered** **immutable map** from keys to values where both keys and values are arbitrary byte strings.

SSTable operations:
1. Look up value for a Key
2. Iterate over all K-V pairs in a specified key range

- Each SSTable contains a sequence of blocks.
- A block index is used to locate the blocks
- A lookup maybe performed with a single disk seek:
	- Find appropriate block by doing binary search in the in-memory index.
	- Read the appropriate block from disk.
- Optionally an SSTable can be completely mapped into memory.

### Distributed Locking

#### Chubby
A highly available and persistent distributed lock service.

It consists of 5 active replicas, one of which is elected master and actively serves requests. 

The service is live when a majority of the replicas are running and can communicate with each other.

It provides a namespace that consists of directories and small files. ==Each directory or file can be used as a lock, and reads and writes to a file are atomic==.

The Chubby client library provides consistent caching of Chubby files. Each Chubby client maintains a **session** with a Chubby service. 

A client’s session expires if it is unable to renew its session lease within the lease expiration time. ==When a client’s session expires, it loses any locks and open handles.==

Chubby clients can also register callbacks on Chubby files and directories for notification of changes or session expiration. 

##### For what does Bigtable use Chubby ?
1. Ensure that there is at most one active master at any time;
2. Store the bootstrap location of Bigtable data.
3. **Discover tablet servers**
4. Finalize tablet server deaths
5. Store Bigtable schema information
6. Store access control lists

If Chubby becomes unavailable for an extended period of time, Bigtable becomes unavailable.

## Implementation
 3 major components:
 1. A library linked into every client
 2. A master server
 3. Many tablet servers(can be dynamically added/removed)

### Master
The master is a responsible for:
	1. Assigning tablets to tablet servers
	2. Detect the addition and expiration of tablet servers
	3. Balancing tablet-server load
	4. Garbage collection of files in GFS
	5. Schema changes
		1. Table creation
		2. Column Family creation

> [!INFO] Because Bigtable clients do not rely on the master for tablet location information, most clients never communicate with the master. As a result, the master is lightly loaded in practice.

### Tablet server
It manages a set of tablets for which:
1. It handles read and write requests
2. Splits tablets that have grown too large

### Tablet location

![[Pasted image 20240923192929.png]]
1. A file stored in Chubby contains the location of the **root tablet.**
2. The **root tablet** contains location of all the tablets within the special METADATA table.
3. Each METADATA tablet contains the location of a set of user tablets.

The root tablet is just the first tablet in the METADATA table, but is treated specially—it is never split—to ensure that the tablet location hierarchy has no more than three levels.

So, to find location of a tablet: 
Root table lookup → METADATA table lookup → Tablet lookup

> Q.  How does METADATA table store tablet location ?
   A.   Each location is stored under a row key: 
   (Encoded table identifier, End Row)

The client library caches tablet locations. 

If the client
1. Does not have the location of a tablet cached or 
2. Discovers that cached location information is incorrect, 

then it recursively moves up the tablet location hierarchy(Root → Metadata → Tablet)

#### Client cache empty: 3 network round-trips
1. Read from the ROOT Tablet
2. Read from the METADATA table
3. Access the data

#### Client cache stale entry: 6 network round-trips
1. Failed attempt to use the cached location
2. Read from root tablet to refresh cache and locate METADATA table
3. Read from METADATA table and find new tablet location.
4. Retrieve the tablet location
5. Access the node with the actual data
6. Additional round-trip for secondary miss

We also store secondary information in the METADATA table, including a log of all events pertaining to each tablet.(Used for debugging and performance analysis)

### Tablet assignment
Each tablet is assigned to one tablet server at a time. 

The master keeps track of:
1. The set of live tablet servers
2. The current assignment of tablets to tablet servers(including unassigned tablets)

When a tablet is unassigned, and a tablet server with sufficient room for the tablet is available, the master assigns the tablet by sending a **tablet load request** to the tablet server.

Bigtable uses Chubby to keep track of tablet servers.

When a tablet server starts, it creates, and acquires an exclusive lock on, a uniquely-named file in a specific Chubby directory. 

The master monitors this directory (the *servers* directory) to discover tablet servers.(Refer to point 3 of [[#For what does Bigtable use Chubby ?]]).

1. A tablet server stops serving its tablets if it loses its exclusive lock.
2. A tablet server will attempt to reacquire an exclusive lock on its file as long as the file still exists. If the file no longer exists, then the tablet server will never be able to serve again, so it kills itself.
3. Whenever a tablet server terminates, it attempts to release its lock so that the master will reassign its tablets more quickly.

**How can master know if tablet server is no longer active ?**
It periodically asks each tablet server for the status of its lock.

If a tablet server reports that it has ==lost its lock==, or master was ==unable to reach== a server during its last several attempts, ==the master attempts to acquire an exclusive lock on the server’s file==. 

If the master is able to acquire the lock, 
1. Chubby is live, 
2. The tablet server is either:
	1. Dead, or 
	2. Having trouble reaching Chubby,

So the master ensures that the tablet server can never serve again by deleting its server file.(Remember, tablet server kills itself if it loses the lock which is ensured via the server file)

Once a server’s file has been deleted, the master can move all the tablets that were previously assigned to that server into the set of unassigned tablets. 

**How to ensure cluster is not vulnerable to networking issues ?**
To ensure that a Bigtable cluster is not vulnerable to networking issues between the master and Chubby, the master kills itself if its Chubby session expires. 

However, as described above, master failures do not change the assignment of tablets to tablet servers. 

#### Master startup
When a master is started by the cluster management system, it needs to discover the current tablet assignments before it can change them. 

The master executes the following steps at startup. 
1. Grabs a unique “master” lock in Chubby to prevent concurrent master instantiations.
2. To scan the servers directory in Chubby to find the live tablet servers.
3. The master communicates with every live tablet server to discover what tablets are already assigned to each server.
4. The master scans the METADATA table to learn the set of tablets. 

==Tablets in METADATA table - Tablets list from tablet servers = Unassigned tablets==

**What if METADATA tablets have not been assigned yet ?**
So, METADATA table can’t be scanned by server till METADATA tablets are assigned. 

To mitigate this, master adds the root tablet to the list of unassigned tablets, if it was found unassigned during step 3. This will ensure that root tablet will be assigned. 

Because root tablet contains. names of all METADATA tablets, master knows about all of them after scanning the root tablet. (Remember, root tablet is present in a file separately stored in Chubby)

**When does the set of existing tablets change?**
1. A tablet is created or deleted
2. Two existing tables are merged to form one larger tablet
3. Existing tablet is split into two smaller tablets

The master can keep track of the first two scenarios, because it initiates them, but can’t keep track of splits.

**Why ?**
Tablet splits are treated specially since they are initialized by a tablet server. 

**How do we solve this?**
1. The tablet server commits the split by ==recording information for the new tablet in the METADATA table==.
2. When the split has committed, the master is notified. 

**What if the split notification is lost?**
Master detects the new tablet when it asks a tablet server to load the tablet that has now split. 

The tablet server will notify the master of the split, because the tablet entry it finds in the METADATA table will specify only a portion of the tablet that the master asked it to load.

### Tablet Serving
The persistent state of a tablet is stored in GFS([[Google File System]]). 

#### Updates
Updates are committed to a commit log that stores redo records. 

> [!INFO] What is a redo record ?
> A redo record, also called a redo entry, **holds a group of change vectors, each of which describes or represents a change made to a single block in the database**.

![[Pasted image 20240926194404.png]]
Recently committed ones → Sorted Buffer(**Memtable**) in RAM
Older Updates → Sequence of SSTables

#### Recovery
1. Tablet server reads metadata from METADATA table.
	1. metadata = List of tablet SSTables + Redo points(pointers into commit logs that may contain data for the tablet)
2. Server reads the indices of the SSTables into memory
3. Server reconstructs the mem-table by applying all of the updates that have committed since the redo points. 

#### Write Operation
1. Check ==whether write operation is well-formed==.
2. Check ==whether sender is authorized== to perform mutation.
	1. Read the list of permitted writers from aChubby file
3. If mutation is valid, ==write to commit log==.
	1. Use Group commit to improve throughput of small mutations.
4. After write has been committed, ==insert contents into the memtable==.

>[!faq] When does the write reach the disk ?
>  The MemTable is sorted and acts as a write buffer that holds recent writes. Data remains in the MemTable until it reaches a certain size. When the MemTable reaches its size limit, it is flushed to GFS(to be stored persistently on disk) as an SSTable (Sorted String Table).

#### Read Operation
1. Check for ==well-formedness== and ==proper authorization==.
2. If valid, execute on a merged view of sequence of SSTables and memtable.

> Incoming Read and Write Operations can continue while tablets are split and merged.
### Compactions

### Minor Compactions
(MemTable size) > Threshold → Freeze contents → Convert to SSTable → Write to GFS → Create new MemTable
#### Goals
1. Reduce memory usage of the tablet server
2. Reduce amount of data that has to be read from the commit log during recovery if server potentially dies. 
### Merging Compaction
As every minor compaction creates a new SSTable, read operations might need to merge updates from an unspecified number of SSTables. 

So, we bound the number of SSTable files by executing a **merging** compaction. 

A merging compaction 
1. Reads the contents of a few SSTables and the memtable.
2. Write out a new SSTable from the merged contents. 
3. Discard the Input SSTables and memtable. 
### Major Compaction
A merging compaction that ==rewrites all SSTables into exactly one SSTable==. 

It produces an SSTable that ==contains no deletion information or deleted data==. On the other hand, SSTables produced by non-major compactions can contain special deletion entries that suppress deleted data in older SSTables that are still live. 

Bigtable cycles through all of its tablets and regularly applies major compactions to them. 

#### Purposes
1. Reclaim resources used by deleted data.
2. Ensure that deleted data disappears from the system in a timely fashion. 

## Refinements

### Locality groups
Clients can group multiple column families together into a locality group. A separate SSTable is generated for each locality group in each tablet.

Segregating column families that are not typically accessed together into sep- arate locality groups ==enables more efficient reads==.

In addition, some useful tuning parameters can be specified on a per-locality group basis. 

For example, A locality group can be declared to be in-memory, and their SSTables will be ==loaded lazily into the memory== of the tablet server.

**Where will it be useful ?**
It is useful for small pieces of data that are accessed frequently. 

> [!faq] What does it mean to load lazily into RAM ? 
>  The data although declared in-memory, will be only be brought/loaded into memory when needed. 

### Compression
Clients can control whether or not the SSTables for a locality group are compressed, and if so, which compression format is used. 

The user-specified compression format is applied to each SSTable block(whose size is controllable via a locality group specific tuning parameter), instead of at the file level. 
#### Tradeoff: Block Level vs File level. 
1. Block-level compression allows for ==fine-grained control over the compression behavior== for different types of data.
2. Compressing each block separately results in slightly l==ess efficient overall compression==(in space and time) compared to compressing the entire file.
3. The block-level compression allows for reading small portions of an SSTable without needing to decompress the entire file.This is crucial for performance when only a subset of the data is needed.

> [!faq] Why is compressing each block separately less space efficient ?
>  
>  Generally speaking, compression tends to be more efficient when applied to larger amounts of data. 
>  1. Most compression algorithms work by identifying and eliminating redundancies in data. Larger files typically contain more redundancies, giving compression algorithms more opportunities to optimize.
>  2. With more context to work with, algorithms can build better dictionaries or frequency tables, leading to higher compression ratios. 
>  3. Compression algorithms often have some overhead (metadata, dictionaries, etc.). If you have larger files to compress, overall overhead space will be lesser.
>  4. When compressing at the block level, each block is treated independently. This means that ==patterns or redundancies that span across blocks can't be leveraged for compression==. It also results in more instances of compression overhead, one for each block.

### Caching for read performance

To improve read performance, tablet servers use two levels of caching. 

1. **The Scan Cache** : A higher-level cache that caches the key-value pairs returned by the SSTable interface to the tablet server code. 
	1. The Scan Cache is most useful for appli- cations that tend to read the same data repeatedly
2. **The Block Cache**: A lower-level cache that caches SSTables blocks that were read from GFS. 
	1. The Block Cache is useful for applications that tend to read data that is close to the data they recently read (e.g., se- quential reads, or random reads of different columns in the same locality group within a hot row).

### Bloom filters

#### Premise
A read operation has to read from all SSTables that make up the state of a tablet. If these SSTables are not in memory, ==we may end up doing many disk accesses==. 

#### Solution
We reduce the number of accesses by allowing clients to specify that Bloom filters should be created for SSTables in a particular locality group. 

A Bloom filter allows us to ask whether an SSTable might contain any data for a specified row/column pair. So using bloom filters, most lookups for non-existent rows or columns do not need to touch disk. 

### Commit-log implementation

#### Premise
If we keep the commit log for each tablet in a separate log file, a very large number of files would be written concurrently to GFS, which in turn ==could cause a large number of disk seeks== to write to the different physical log files. 

In addition, having separate log files per tablet also reduces the effectiveness of the group commit optimization, since groups would tend to be smaller.

> [!faq] What is a group commit ?
>  Group commit is a performance optimization technique used in database systems. ==Instead of writing each transaction to disk immediately, multiple transactions are grouped together and written in a single I/O operation==. 
>  
>  This reduces the overall number of I/O operations, which can significantly improve performance, especially on systems where I/O is a bottleneck.

#### Solution
Append mutations to a single commit log per tablet server, thus ==combining mutations for different tablets in the same physical log file==. 

##### Complications in Recovery
Using one log provides significant performance benefits during normal operation, but it complicates recovery. 

When a tablet server dies, the tablets that it served will be moved to a large number of other tablet servers: each server typically loads a small number of the original server’s tablets.

![[Pasted image 20240928114344.png]]

To recover the state for a tablet, the new tablet server needs to reapply the mutations for that tablet from the commit log written by the original tablet server.

**Basic Approach**: Read the full commit log file and apply just the entries needed for the tablets it needs to recover. 

However under this scheme, if 100 machines were each assigned a single tablet from a failed tablet server, then the log file would be read 100 times (once by each server).

**How to avoid duplicate reads of commit log file ?**
Sort the commit log entries in order of the keys(<Table, row name, log sequence number>). 

> [!faq] Why this key ?
>  Table and row name helps identify a tablet. Logs need to be read and applied in the order in which they were written, so we need log sequence number sorting, and log sequence number ensures a unique ID for the mutation.
>  

In the sorted output, all mutations for a particular tablet are contiguous and can therefore be ==read efficiently with one disk seek followed by a sequential read==. 
 
To parallelize the sorting, we partition the log file into 64 MB segments, and ==sort each segment in parallel on different tablet servers==. 
 
This sorting process is ==coordinated by the master== and is initiated when a tablet server indicates that it needs to recover mutations from some commit log file.

**Can there be issues in writing to commit log file in GFS ?**

Writing commit logs to GFS sometimes causes performance hiccups for a variety of reasons, for example:
1. A GFS server machine involved in the write crashes, 
2. The network paths traversed to reach the particular set of three GFS servers is suffering network congestion, or is heavily loaded. 

**How do we protect our mutations from these issues ?**

To protect mutations from GFS latency spikes, each tablet server actually has ==two log writing threads, each writing to its own log file; only one of these two threads is actively in use at a time==. 

If writes to the active log file are performing poorly, ==the log file writing is switched to the other thread==, and mutations that are in the commit log queue are written by the newly active log writing thread.

**How do you manage duplicate entries ?**
Log entries ==contain sequence numbers to allow the recovery process to elide duplicated entries== resulting from this log switching process.

### Speeding up tablet recovery

If the master moves a tablet from one tablet server to another, ==the source tablet server first does a minor compaction on that table==t. 

This compaction **reduces recovery time by reducing the amount of uncompacted state** in the tablet server’s commit log. 

After finishing this compaction, the tablet server stops serving the tablet. 

**What about the new entries that come during the time when the first minor compaction was happening, because serving stopped after the compaction was complete?** 

Before it actually unloads the tablet, the tablet server does another (usually very fast) minor compaction to eliminate any remaining uncompacted state in the tablet server’s log that arrived while the first minor compaction was being performed. 

After this second minor compaction is complete, the tablet can be loaded on another tablet server without requiring any recovery of log entries.


### Exploiting Immutability
Besides the SSTable caches, various other parts of the Bigtable system have been simplified by the fact that ==all of the SSTables that we generate are immutable==. 

Because SSTables are immutable, so they are set in stone(no updates can be done), we do not need any synchronization of accesses to the file system when reading from SSTables. So concurrency control over rows can be implemented very efficiently. 

But, memtable is mutable(the only mutable data structure) and can accessed by both reads and writes.  

**How to solve contention for reads of memtable?**
Each memtable row is copy-on-write and thus allows reads and writes to happen in parallel. 

> [!faq] What is Copy-on-Write (CoW) mechanism ?
>   It is a resource management and optimization technique used in computing to efficiently handle situations where multiple entities (such as processes, threads, or operations) need to access the same data.   
>   
>   Suppose you have a piece of data (like a block of memory, an object, or a row in a memtable) that is being used by multiple entities. Initially, all these entities point to the same instance of this data. Since the data is identical for all entities, they share the same memory space.  
>   
>   When one of the entities needs to modify the data, this is where CoW comes into play. Instead of modifying the shared data directly, the system creates a copy of the data for the entity that intends to write.
>   
>   The modification is then performed on the copy, leaving the original shared data unchanged for other entities that still need it. 
>   
>   After the copy is made and modified, the writing entity's reference to the data is updated to point to the new copy. Meanwhile, other entities continue to reference the original, unmodified data.
>   

Since SSTables are immutable, the ==problem of permanently removing deleted data is transformed to garbage collecting obsolete SSTables==.  Each tablet’s SSTables are registered in the METADATA table. The master removes obsolete SSTables as a **mark-and-sweep garbage collection** over the set of SSTables, where the METADATA table contains the set of roots.

Finally, the immutability of SSTables ==enables us to split tablets quickly==. Instead of generating a new set of SSTables for each child tablet, we let the child tablets share the SSTables of the parent tablet.









. 












































