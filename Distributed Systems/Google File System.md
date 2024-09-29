Source: https://research.google/pubs/the-google-file-system/ 

## Introduction
This system is designed and implemented to meet the rapidly growing demands of Google’s data processing needs. 

Goals:
1. Performance
2. Scalability
3. Reliability
4. Availability

### Premise
1. Component Failures are the norm rather than the exception.
	1. The file system consists of ==hundreds or even thousands of storage machines built from inexpensive commodity parts and is accessed by a comparable number of client machines==. The quantity and quality of the components virtually guarantee that some are not functional at any given time and some will not recover from their current failures.
	2. Problems are caused by **application bugs**, **operating system bugs**, **human errors** and **failures of disks, memory, connectors, networking and power supplies**.
	3. So **constant monitoring**, **error detection**, **fault tolerance** and **automatic recovery** must be integral to the new system.
2. Files are huge by traditional standards. 
	1. Design assumptions and parameters such as I/O operation and block sizes have to be revisited. 
3. Most files are mutated by appending new data rather than overwriting existing data. 
	1. Given this access pattern on huge files, appending becomes the focus of performance optimization and atomicity guarantees, while caching data blocks in the client loses its appeal.
4. Co-designing the applications and the file system API benefits the overall system by increasing our flexibility.
	1. For example, we have relaxed GFS’s consistency model to vastly simplify the file system without imposing an onerous burden on the applications. We have also introduced an atomic append operation so that multiple clients can append concurrently to a file without extra synchronization between them. 
## Design Overview

### Assumptions

1. The system is built from many inexpensive commodity components that often fail. 
2. The system stores a modest number of large files. We expect a few million files, each typically 100 MB or larger in size.
	1. Small files must be supported, but we need not optimize for them.
3. The workloads consist of two kinds of reads: **Large streaming reads** and **Small random reads**. 
4. The workloads also have many large, sequential writes that append data to files.
5. System must efficiently implement well-defined semantics for multiple client that concurrently append data to the same file.
6. High sustained bandwidth is more important than low latency. 
	1. Most of our target applications place a premium on processing data in bulk at a high rate, while few have stringent response time requirements for an individual read or write.
### Interface
Files are organized hierarchically in directories and identified by path-names. We support the usual operations to create, delete, open, close, read, and write files.\

Moreover, GFS has snapshot and record append operations. Snapshot creates a copy of a file or a directory tree at low cost. Record append allows multiple clients to ap- pend data to the same file concurrently while guaranteeing the atomicity of each individual client’s append.

### Architecture
A GFS cluster consists of a single *master* and multiple *chunkservers* and is accessed by multiple *clients*.

![[Pasted image 20240911225431.png | Figure 1]]

Each of these is typically a commodity Linux machine running a user-level server process. It is easy to run both a chunkserver and a client on the same machine, as long as machine resources permit and the lower reliability caused by running possibly flaky application code is acceptable.

Files are divided into fixed-size *chunks*.  ==Each chunk is identified by an immutable and globally unique 64 bit chunk handle assigned by the master at the time of chunk creation==. 

Chunkservers store chunks on local disks as Linux files and read or write chunk data specified by a chunk handle and byte range. 

==For reliability, each chunk is replicated on multiple chunkservers==. By default, we store three replicas, though users can designate different replication levels for different regions of the file namespace.

#### Master
The master maintains all file system metadata. This includes **namespace**, **access control information**, **mapping from files to chunks**, and **the current locations of chunks**. 

It also controls system-wide activities such as **chunk lease management**, **garbage collection of orphaned chunks**, and **chunk migration between chunkservers.**

The master periodically communicates with each chunkserver in **HeartBeat messages** to ==give it instructions and collect its state==.

#### Clients
GFS client code linked into each application implements the file system API and communicates with the master and chunkservers to read or write data on behalf of the application. 

Clients interact with the master for metadata operations, but ==all data-bearing communication(reads, writes) goes directly to the chunkservers==.

Neither the client nor the chunkserver caches file data. Client caches offer little benefit because most applications stream through huge files or have working sets too large to be cached. Not having them simplifies the client and the overall system by eliminating cache coherence issues.

### Single Master
Having a single master vastly simplifies our design and enables the master to make sophisticated chunk placement and replication decisions using global knowledge.

However, we must minimize its involvement in reads and writes so that it does not become a bottleneck. So, ==clients never read or write file data through the master, instead it asks the master which chunkservers it should contact==. 

1. Using fixed chunk size, client translates the file name and byte offset specified by the application into a chunk index within the file.
2. Master replies with the corresponding chunk handle and locations of the replicas. 
3. Client sends a request to one of the replicas, most likely the closest one. The request specifies the chunk handle and a byte range within that chunk. 
4. Further reads of the same chunk require no more client-master interaction until the cached information expires or the file is reopened. In fact, the client typically asks for multiple chunks in the same request and the master can also include the information for chunks immediately following those requested.

### Chunk Size
It is a key design parameter. GFS has chosen 64 MB as the chunk size. 

Each chunk replica is stored as a plain Linux file on a chunkserver and is extended only as needed. Also, Lazy space allocation avoids wasting space due to internal fragmentation.

> [!faq] What is Lazy space allocation ?
> 
>  With lazy space allocation, the physical allocation of space is delayed as long as possible, until data at the size of the chunk size (in this case, 64 MB) is accumulated. In other words, the decision process that precedes the allocation of a new chunk on disk, is heavily influenced by the size of the data that is to be written. This preference of waiting instead of allocating more chunks based on some other characteristic, minimizes the chance of internal fragmentation. 
>  
>  This does not mean the chunks will always be filled up. A chunk which contains the file region for the end of a file will typically only be partially filled up. Because, most chunks are full because most files contain many chunks, only the last of which may be partially filled.

### Advantages of large chunk size
1. Reduces client’s need to interact with server ==because reads and writes on the same chunk require only one initial request to the master for chunk location information==.
	1. A significant reduction for heavy workloads where applications read and write large files sequentially.
2. Since on a large chunk, a client is more likely to perform many operations on a given chunk, it ==can reduce network overhead by keeping a persistent TCP connection to the chunkserver over an extended period of time==.
3. It reduces the size of the metadata stored on the master, which allows us to keep the metadata in memory.

### Disadvantages of large chunk size
1. A small file consists of a small number of chunks, perhaps just one. The chunkserver storing those chunks may become hotspots if many clients access the same file. 
	1. Can be fixed by storing such hotspot files with a higher replication factor.
	2. Or allow clients to read data from other clients in special cases.

### Metadata
Master stores 3 major types of metadata:
1. File and chunk namespaces
2. Mapping from files to chunks
3. Locations of each chunk’s replicas

Everything is stored in the main memory(RAM) of the master.

The first two types (namespaces and file-to-chunk mapping) are also kept persistent by logging mutations to an operation log stored on the master’s local disk and replicated on remote machines. ==Using a log allows us to update the master state simply, reliably, and without risking inconsistencies in the event of a master crash.== 

The master does not store chunk location information persistently. Instead, ==it asks each chunkserver about its chunks at master startup and whenever a chunkserver joins the cluster==.


#### In-memory Data Structures
The master periodically scans its entire state in the background. The periodic scanning is used to implement:
1. Chunk Garbage collection
2. Re-replication in presence of chunkserver failures
3. Chunk migration to balance load and disk space usage across chunkservers.

> [!INFO] Potential Concern for RAM only approach 
> One potential concern for this memory-only approach is that the number of chunks and ==hence the capacity of the whole system is limited by how much memory the master has==. This is not a serious limitation in practice. The mas- ter maintains less than 64 bytes of metadata for each 64 MB chunk. Most chunks are full because most files contain many chunks, only the last of which may be partially filled. Similarly, the file namespace data typically requires less then 64 bytes per file because it stores file names compactly using prefix compression. 
> 
> If necessary to support even larger file systems, the cost of adding extra memory to the master is a small price to pay for the simplicity, reliability, performance, and flexibility we gain by storing the metadata in memory.

#### Chunk Locations
The authors initially attempted to keep chunk location information persistently at the master, but they we decided that it was much simpler to request the data from chunkservers at startup, and periodically thereafter. 

This ==eliminated the problem of keeping the master and chunkservers in sync== as chunkservers **join and leave the cluster**, **change names**, **fail**, **restart**, and so on. In a cluster with hundreds of servers, these events happen all too often.

#### Operation Log
The operation log contains a historical record of critical metadata changes. It is central to GFS. 
\
Not only is it the only persistent record of metadata, but it also serves as a logical time line that defines the order of concurrent operations. 

Files and chunks, as well as their versions, are all uniquely and eternally identified by the logical times at which they were created.

##### Storing operation log
Since operation log is critical, we must store it reliably and ==not make changes visible to clients until metadata changes are made persistent==. Otherwise, we effectively lose the whole file system or recent client operations even if the chunks themselves survive. 

Therefore, we replicate the log on multiple remote machines and ==respond to a client operation only after flushing the corresponding log record to disk both locally and remotely.== 

The master ==batches several log records together before flushing== thereby reducing the impact of flushing and replication on overall system throughput.

##### State Recovery
The master recovers its file system state by replaying the operation log. To minimize startup time, we must keep the log small. 

The master checkpoints its state whenever the log grows beyond a certain size so that it can recover by loading the latest checkpoint from local disk and replaying only the limited number of log records after that. 

###### Checkpoint
The checkpoint is in a ==compact B-tree like form that can be directly mapped into memory and used for namespace lookup without extra parsing==. This further speeds up recovery and improves availability.
 
Because ==building a checkpoint can take a while, the master’s internal state is structured in such a way that a new checkpoint can be created without delaying incoming mutations==. The master ==switches to a new log file and creates the new checkpoint in a separate thread==. 

The new checkpoint includes all mutations before the switch. It can be created in a minute or so for a cluster with a few million files. When completed, it is written to disk both locally and remotely.

Recovery needs only the latest complete checkpoint and subsequent log files. Older checkpoints and log files can be freely deleted, though we keep a few around to guard against catastrophes. 

A failure during checkpointing does not affect correctness because the recovery code detects and skips incomplete checkpoints.

### Consistency Model

#### Guarantees by GFS
1. File namespace mutations (e.g., file creation) are atomic. 
	1. They are handled exclusively by the master: namespace locking guarantees atomicity and correctness 
2. The state of a file region after a data mutation depends on the type of mutation, whether it succeeds or fails, and whether there are concurrent mutations. ![[Pasted image 20240912101944.png]]
	1. A file region is **consistent** if all clients will always see the same data, regardless of which replicas they read from.
	2. A region is **defined** after a file mutation, if it is consistent and clients will see what the mutation writes in its entirety.
		1. When a mutation succeeds without interference from concurrent writers, the affected region is defined (and by implication consistent): ==all clients will always see what the mutation has written==. Concurrent successful mutations leave the region undefined but consistent: all clients see the same data, but it may not reflect what any one mutation has written. Typically it consists of mingled fragments from multiple mutations.
	3. A failed mutation makes the region inconsistent(hence also undefined). Different clients may see different data at different times.
3. Data mutations maybe **writes** or **record appends.** 
4. After a sequence of successful mutations, the mutated file region is guaranteed to be defined and contain the data written by the last mutation.
	1. GFS achieves this by
		1. applying mutations to a chunk in the same order on all its replicas, and
		2. using chunk version numbers to detect any replica that has become stale because it has missed mutations while its chunkserver was down.
	2. Stale replicas will never be involved in a mutation or given to clients asking the master for chunk locations. They are garbage collected at the earliest opportunity.
5. Since clients cache chunk locations, they may read from a stale replica before that information is refreshed. This window is limited by the cache entry’s timeout and the next open of the file, which purges from the cache all chunk information for that file. 
6. ==As most of our files are append-only, a stale replica usually returns a premature end of chunk rather than outdated data==. When a reader retries and contacts the master, it will immediately get current chunk locations.
7. Long after a successful mutation, component failures can of course still corrupt or destroy data. GFS ==identifies failed chunkservers by regular handshakes between master and all chunkservers== and ==detects data corruption by checksumming==.
8. Once a problem surfaces, the ==data is restored from valid replicas as soon as possible==. ==A chunk is lost irreversibly only if all its replicas are lost before GFS can react, typically within minutes==. Even in this case, it becomes unavailable, not corrupted: applications receive clear errors rather than corrupt data.

#### Implications for Applications
1. All our applications mutate files by appending rather than overwriting.
2. Readers verify and process only the file region up to the last checkpoint, which is known to be in the defined state.
3. Readers deal with the occasional padding and duplicates as follows. Each record prepared by the writer contains extra information like check- sums so that its validity can be verified. A reader can identify and discard extra padding and record fragments using the checksums.

## System Interactions

### Leases and mutation order

![[Pasted image 20240912112814.png]]
1. A client ==asks the master which chunkserver holds the current lease for the chunk and the locations of the other replicas==. If no one has a lease, the master grants one to a replica it chooses.
2. The master ==replies with the identity of the primary and the locations of the other(secondary) replicas==. Client caches this data for future mutations, and only contacts the master again only when the primary becomes unreachable or replies that it no longer holds a lease.
3. The client ==pushes data to all the replicas==. A client can do so in any order. Each chunkserver will store the data in an internal LRU buffer cache until the data is used or aged out. 
4. Once all the replicas have acknowledged receiving the data, ==the client sends a write request to the primary==. The request identifies the data pushed earlier to all of the replicas. The ==primary assigns consecutive serial numbers to all the mutations it receives, possibly from multiple clients, which provides the necessary serialization==. It applies the mutation to its own local state in serial number order.
5. The primary ==forwards the write request to all secondary replicas==. Each secondary replica applies mutations in the same serial number assigned by the primary.
6. The secondaries all ==reply to the primary indicating that they have completed the operation==.
7. The primary replies to the client. Any errors encountered at any of the replicas are reported to the client. In case of errors, the write may have succeeded at the primary and an arbitrary subset of the secondary replicas. (If it had failed at the primary, it would not have been assigned a serial number and forwarded.) The client request is considered to have failed, and the modified region is left in an inconsistent state. ==Our client code handles such errors by retrying the failed mutation==. It will make a few attempts at steps (3) through (7) before falling back to a retry from the beginning of the write.

If a write by the application is large or straddles a chunk boundary, GFS client code breaks it down into multiple write operations.

### Data Flow

We decouple the flow of data from the flow of control to use the network efficiently. 

To fully utilize each machine’s network bandwidth, the data is pushed linearly along a chain of chunkservers rather than distributed in some other topology (e.g., tree). Thus, each machine’s full outbound bandwidth is used to transfer the data as fast as possible rather than divided among multiple recipients.

To avoid network bottlenecks and high-latency links (e.g., inter-switch links are often both) as much as possible, ==each machine forwards the data to the “closest” machine in the network topology that has not received it.==

Finally, we minimize latency by pipelining the data transfer over TCP connections. Once a chunkserver receives some data, it starts forwarding immediately. Pipelining is especially helpful to us because we use a switched network with full-duplex links. 

Sending the data immediately does not reduce the receive rate. Without network congestion, the ideal elapsed time for transferring B bytes to R replicas is B/T + RL where T is the network throughput and L is latency to transfer bytes between two machines.

### Atomic Record Appends
GFS provides an atomic append operation called record append. In a traditional write, the client specifies the offset at which data is to be written. Concurrent writes to the same region are not serializable: the region may end up containing data fragments from multiple clients. 

In a record append, however, the client specifies only the data. GFS appends it to the file at least once atomically (i.e., as one continuous sequence of bytes) at an offset of GFS’s choosing and returns that offset to the client.

Record append is heavily used by our distributed applications in which many clients on different machines append to the same file concurrently. Clients would need additional complicated and expensive synchronization, for example through a distributed lock manager, if they do so with traditional writes.

Record append is a kind of mutation and follows the control flow with only a little extra logic at the primary. 

 The primary checks to see ==if appending the record to the current chunk would cause the chunk to exceed the maximum size== (64 MB). 
 - If so, it pads the chunk to the maximum size, tells secondaries to do the same, and replies to the client indicating that the operation should be retried on the next chunk. 
 - If the record fits within the maximum size, which is the common case, the primary appends the data to its replica, tells the secon- daries to write the data at the exact offset where it has, and finally replies success to the client.

 A record append fails at any replica, the client retries the operation. As a result, replicas of the same chunk may contain different data possibly including duplicates of the same record in whole or in part. ==GFS does not guarantee that all replicas are bytewise identical.It only guarantees that the data is written at least once as an atomic unit.== 

### Snapshot
The snapshot operation ==makes a copy of a file or a directory tree (the “source”) almost instantaneously==, while minimizing any interruptions of ongoing mutations. 

Our users use it to quickly create branch copies of huge data sets (and often copies of those copies, recursively), or to checkpoint the current state before experimenting with changes that can later be committed or rolled back easily.

## Master Operation

### Namespace management and Locking
Many master operations can take a long time, and we do not want to delay other master operations while they are running. Therefore, we allow multiple operations to be active and ==use locks over regions of the namespace to ensure proper serialization==.

GFS does not have a per-directory data structure that lists all the files in that directory. Nor does it support aliases for the same file or directory. GFS logically ==represents its namespace as a lookup table mapping full pathnames to metadata==. With **prefix compression**, this table can be efficiently represented in memory. Each node in the namespace tree (either an absolute file name or an absolute directory name) has an associated read-write lock.

Each master operation acquires a set of locks before it runs. Typically, if it involves “/d1/d2/.../dn/leaf”, it will acquire read-locks on the directory names “/d1”, “/d1/d2”, ..., “/d1/d2/.../dn”, and either a read lock or a write lock on the full pathname “/d1/d2/.../dn/leaf”. Note that leaf may be a file or directory depending on the operation.

> “The nice property of this locking scheme is that it ==allows concurrent mutations in the same directory==.”

### Replica Placement
A GFS cluster is highly distributed at many levels.

It typically has hundreds of chunkservers spread across many machine racks. These chunkservers in turn may be accessed from hundreds of clients from the same or different racks. ==Communication between two machines on different racks may cross one or more network switches==. 

Additionally, bandwidth into or out of a rack may be less than the aggregate bandwidth of all the machines within the rack. 

Multi-level distribution presents a unique challenge to dis- tribute data for scalability, reliability, and availability.

The chunk replica placement policy serves two purposes: 
1. Maximize data reliability and availability
2. maximize network bandwidth utilization. 

For both, it is not enough to spread replicas across machines, which only guards against disk or machine failures and fully utilizes each machine’s net- work bandwidth. We must also spread chunk replicas across racks. This ensures that some replicas of a chunk will survive and remain available even if an entire rack is damaged or offline 

It also means that traffic, especially reads, for a chunk can exploit the aggre- gate bandwidth of multiple racks. On the other hand, write traffic has to flow through multiple racks, a tradeoff we make willingly.
### Creation, Re-replication, Rebalancing
Chunk replicas are created for 3 reasons:
1. Chunk Creation
2. Chunk re-replication
3. Rebalancing

When the master *creates* a chunk, it chooses where to place the initially empty replicas. It considers several factors: 

1. We want to place new replicas on chunkservers with below-average disk space utilization. Over time, this will equalize disk utilization across chunkservers.
2. We want to limit the number of recent “creations” on each chunkserver. As they will then attract a lot of heavy traffic together on the chunkserver.
3. We want to spread replicas of a chunk across racks.  

The master re-replicates a chunk as soon as the number of available replicas falls below a user-specified goal. 

This could happen for various reasons: 
1. A chunkserver becomes unavailable 
2. A chunkserver reports that its replica may be corrupted. 
3. one of the chunkserver’s disks is disabled because of errors, or the replication goal is increased. 

Each chunk that needs to be re-replicated is prioritized based on several factors. 
1. How far is the chunk from its replication goal(More far, higher priority)
2. Re-replicate chunks for live files over the chunks belong to recently deleted files. 
3. Boost the priority of any chunk, that is blocking client progress, to minimize the impact of failures on running applications. 

The master picks the highest priority chunk and “clones” it by instructing some chunkserver to copy the chunk data directly from an existing valid replica. 

The master rebalances replicas periodically: 
1. It examines the current replica distribution and moves replicas for better disk space and load balancing.
	1. Also through this process, the master gradually fills up a new chunkserver rather than instantly swamps it with new chunks and the heavy write traffic that comes with them.
2. The master must also choose which existing replica to remove. In general, it prefers to ==remove the replicas on chunkservers with below-average free space== so as to equalize disk space usage.

### Garbage Collection
After a file is deleted, GFS does not immediately reclaim the available physical storage. It does so only lazily during regular garbage collection at both the file and chunk levels.

#### Mechanism
When a file is deleted by the application, the master logs the deletion immediately just like other changes. 

However instead of reclaiming resources immediately, the file is just renamed to a hidden name that includes the deletion timestamp. During the master’s regular scan of the file system namespace, it removes any such hidden files if they have existed for more than the configured period(default 3 days).

Until then, the file can still be read under the new, special name and can be undeleted by renaming it back to normal. When the hidden file is removed from the namespace, its in-memory metadata is erased. This effectively severs its links to all its chunks.

In a similar regular scan of the chunk namespace, the master identifies orphaned chunks (i.e., those not reachable from any file) and erases the metadata for those chunks.

In a HeartBeat message regularly exchanged with the master, each chunkserver reports a subset of the chunks it has, and the master replies with the identity of all chunks that are no longer present in the master’s metadata. The chunkserver is free to delete its replicas of such chunks.

**Summary**: 
1. Log the delete operation.
2. Rename the file to a hidden name with timestamp included.
3. After the configured period, remove the file from the namespace, thus removing its in-memory metadata as well, and cutting all links to its chunks.
4. In a regular heartbeat message exchange, such orphaned files are identified and are instructed to delete from chunkservers.

### Discussion
The garbage collection approach to storage reclamation offers several advantages over eager deletion. 

1. It is simple and reliable, in a large scale distributed system where component failures are common.
	1. Garbage collection provides a uniform and dependable way to clean up any replicas not known to be useful. 
2. It merges storage reclamation into the regular background activities of the master, such as the regular scans of namespaces and handshakes with chunkservers. 
	1. Thus, it is done in batches and the cost is amortized. 
	2. Moreover, it is done only when the master is relatively free. The master can respond more promptly to client requests that demand timely attention.
3. Delay in reclaiming storage provides a safety net against accidental irreversible deletion.

Disadvantages:
1. The delays sometimes hinders user effort to fine-tune usage when storage is tight. 
2. Applications that repeatedly create and delete temporary files may not be able to reuse the storage right away. 

We address these issues by expediting storage reclamation if a deleted file is explicitly deleted again. We also allow users to apply different replication and reclamation policies to different parts of the namespace.
### Stale Replica Detection
Chunk replicas may become stale if chunkserver fails and misses mutations to the chunk while it is down. 

For each chunk, the master maintains a *chunk version number* to distinguish between up-to-date and stale replicas. 

Whenever the master grants a new lease on the chunk, it increases the chunk version number and informs the up-to-date replicas. 


## Fault tolerance and diagnosis
Component failures can result in an unavailable system, or worse corrupted data. 

### High Availability
We keep the overall system highly available with 2 simple yet effective strategies: 
1. Fast Recovery
2. Replication

#### Fast Recovery
Both the master and the chunkserver are designed to restore their state and start in seconds no matter how they terminated. 

In fact, we do not distinguish between normal and abnormal termination; servers are routinely shut down just by killing the process. 

Clients and other servers experience a minor hiccup as they time out on their outstanding requests, reconnect to the restarted server, and retry.

#### Chunk Replication
Each chunk is replicated on multiple chunkservers on different racks. Users can specify different replication levels for different parts of the file namespace.

The master clones existing replicas as needed to keep each chunk fully replicated as chunkservers go offline or detect corrupted replicas through checksum verification.

#### Master Replication
The master state is replicated for reliability. 

1. Its operation log and checkpoints are replicated on multiple machines. 
2. A mutation to the state is considered committed only after its log record has been flushed to disk locally and on all master replicas. 
3. For simplicity, one master process remains in charge of all mutations as well as background activities such as garbage collection that change the system internally. When it fails, it can restart almost instantly.
4. If its machine or disk fails, monitoring infrastructure outside GFS starts a new master process elsewhere with the replicated operation log.
5. Clients use only the canonical name of the master (e.g. gfs-test), which is a DNS alias that can be changed if the master is relocated to another machine.

##### Shadow masters
Shadow masters provide read-only access to the file system even when the primary master is down. They are shadows, not mirrors, in that they may lag the primary slightly, typically fractions of a second. 

They enhance read availability for files that are not being actively mutated or applications that do not mind getting slightly stale results.

In fact, since file content is read from chunkservers, applications do not observe stale file content.

To keep itself informed, a shadow master reads a replica of the growing operation log and applies the same sequence of changes to its data structures exactly as the primary does. 

Like the primary, it polls chunkservers at startup (and infrequently thereafter) to locate chunk replicas and exchanges frequent handshake messages with them to monitor their status.

It ==depends on the primary master only for replica location updates resulting from the primary’s decisions to create and delete replicas==.

### Data Integrity
Each chunkserver uses checksumming to detect corruption of stored data. 

We can recover from corruption using other chunk replicas, but ==it would be impractical to detect corruption by comparing replicas across chunkservers. 

Moreover, divergent replicas may be legal: the semantics of GFS mutations, in particular atomic record append as discussed earlier, does not guarantee identical replicas. Therefore, each chunkserver must independently verify the integrity of its own copy by maintaining checksums.

A chunk is broken up into 64 KB blocks. Each has a corresponding 32 bit checksum. Like other metadata, checksums are kept in memory and stored persistently with logging, separate from user data.

For reads, the chunkserver verifies the checksum of data blocks that overlap the read range before returning any data to the requester, whether a client or another chunkserver. 

Therefore chunkservers will not propagate corruptions to other machines.

If a block does not match the recorded checksum, 
1. The chunkserver returns an error to the requestor and reports the mismatch to the master. 
2. In response, the requestor will read from other replicas, while the master will clone the chunk from another replica. 
3. After a valid new replica is in place, the master instructs the chunkserver that reported the mismatch to delete its replica.

Checksumming has little effect on read performance for several reasons. 
1. Since most of our reads span at least a few blocks, we need to read and checksum only a relatively small amount of extra data for verification. 
2. GFS client code further reduces this overhead by trying to align reads at checksum block boundaries. 

Moreover, checksum lookups and comparison on the chunkserver are done without any I/O, and checksum calculation can often be overlapped with I/Os.

==Checksum computation is heavily optimized for writes that append to the end of a chunk== (as opposed to writes that overwrite existing data) because they are dominant in our workloads. 

We just ==incrementally update the check-sum for the last partial checksum block==, and ==compute new checksums for any brand new checksum blocks filled by the append==. Even if the last partial checksum block is already corrupted and we fail to detect it now, the new checksum value will not match the stored data, and the corruption will be detected as usual when the block is next read.

In contrast, if a write overwrites an existing range of the chunk,  we must read and verify the first and last blocks of the range being overwritten, then perform the write, and finally compute and record the new checksums.

If we do not verify the first and last blocks before overwriting them partially, the new checksums may hide corruption that exists in the regions not being overwritten.

During idle periods, chunkservers can scan and verify the contents of inactive chunks. This allows us to detect corruption in chunks that are rarely read, and master can deal with replacing the corrupted replica. 

> This prevents an inactive but corrupted chunk replica from fooling the master into thinking that it has enough valid replicas of a chunk.

### Diagnostic Tools

Extensive and detailed diagnostic logging has helped immeasurably in problem isolation, debugging, and performance analysis, while incurring only a minimal cost. 

Without logs, it is hard to understand transient, non-repeatable interactions between machines. 

GFS servers generate diagnostic logs that record many significant events (such as chunkservers going up and down) and all RPC requests and replies. 

These diagnostic logs can be freely deleted without affecting the correctness of the system. However, we try to keep these logs around as far as space permits.

By matching requests with replies and collating RPC records on different machines, we can reconstruct the en- tire interaction history to diagnose a problem. The logs also serve as traces for load testing and performance analysis.

#### Performance Impact
Performance impact is minimal because these logs are written sequentially and synchronously. The most recent events are also kept in memory and available for continuous online monitoring.
## References
1.  https://stackoverflow.com/questions/18109582/what-is-lazy-space-allocation-in-google-file-system
2. 




