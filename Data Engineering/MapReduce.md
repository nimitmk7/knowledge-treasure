It is a programming model and associated implementation for processing and generating large datasets. Users specify 2 functions:

1. **Map**: Processes a key/value pair to generate a set of intermediate key/value pairs
2. **Reduce**: Merges all intermediate values associated with the same intermediate key

Many real world tasks are expressible in this model.

The run-time system takes care of the details of 
1. **Partitioning the input data,** 
2. **Scheduling the program’s execution across a set of machines**, 
3. **Handling machine failures**
4. **Managing the required inter-machine communication**. 

This allows programmers without any experience with parallel and distributed systems to easily utilize the resources of a large distributed system.
## Programming Model
Input K-V pairs → **Map** → Intermediate K-V pairs → **Reduce** → Output K-V pairs

### Example
**Problem**: Counting number of occurrences of each word in a large collection of documents

```
map(String key, String value): 
// key: document name
// value: document contents for each word w in value:
EmitIntermediate(w, "1");
```
The map function emits each word plus an associated count of occurrences (just ‘1’ in this simple example).

```

reduce(String key, Iterator values): 
// key: a word
// values: a list of counts
int result = 0;
    for each v in values:
      result += ParseInt(v);
    Emit(AsString(result));
```
The reduce function sums together all counts emitted for a particular word. 

In addition, the user writes code to fill in a map-reduce specification object with the names of the input and output files, and optional tuning parameters. The user then invokes the MapReduce function, passing it the specification object. The user’s code is linked together with the MapReduce library.

## Execution

![Execution Overview](mprd_exc_overview.png)

The **Map** invocations are distributed across multiple machines by automatically partitioning the input data into a set of $M$ splits. The input splits can be processed in parallel by different machines. 

The **Reduce** invocations are distributed by partitioning the intermediate key space into $R$ pieces using a partitioning function. (e.g. $hash(key)\space\mathbf{mod}\space R$). The number of partitions (R) and the partitioning function are specified by the user.

### Sequence

1. The MapReduce library in the user program first **splits the input files** into $M$ pieces of typically 16 megabytes to 64 megabytes (MB) per piece (controllable by the user via an optional parameter). It then starts up many copies of the program on a cluster of machines.

2. One of the copies of the program is special – the **master**. The rest are workers that are assigned work by the master. There are $M$ map tasks and $R$ reduce tasks to assign. The master **picks idle workers** and **assigns each one** a map task or a reduce task.

3. A worker who is assigned a map task **reads the contents of the corresponding input split**. It **parses key/value pairs out of the input data** and **passes each pair** to the user-defined Map function. The intermediate key/value pairs produced by the Map function are **buffered in memory**.

4. Periodically, the **buffered pairs are written to local disk**, partitioned into R regions by the partitioning function. The locations of these buffered pairs on the local disk are passed back to the master, who is responsible for **forwarding these locations to the reduce workers.**

5.  When a reduce worker is notified by the master about these locations, it uses **remote procedure calls**(rPC) to read the buffered data from the local disks of the map workers. 
	1. When a reduce worker has read all the intermediate data, it sorts it by the intermediate keys so that all the occurrences of the same key are grouped together. 
	2. The sorting is needed because typically many different keys map to the same reduce task. If the amount of intermediate data is too large to fit in memory, an external sort is used. 

6. The reduce worker iterates over the sorted intermediate data and for **each unique intermediate key encountered**, it passes the key and the corresponding set of intermediate values to the user’s Reduce function. The output of the Reduce function is appended to a final output file for this reduce partition.

7. When all map tasks and reduce tasks have been completed, the master wakes up the user program. At this point, the MapReduce call in the user program returns back to the user code.

After successful completion, the output of the map-reduce execution is available in the $R$ output files (one per reduce task, with file names as specified by the user). 

Typically, users do not need to combine these $R$ output files into one file – they often pass these files as input to another MapReduce call, or use them from another distributed application that is able to deal with input that is partitioned into multiple files.

### Master Data Structures

The master keeps several data structures. For each map task and reduce task, it stores the state (idle, in-progress, or completed), and the identity of the worker machine (for non-idle tasks).

The master is the channel through which the location of intermediate file regions is propagated from map tasks to reduce tasks. Therefore, for each completed map task, the master stores the locations and sizes of the R inter- mediate file regions produced by the map task.

Updates to this location and size information are received as map tasks are completed. The information is pushed incrementally to workers that have in-progress reduce tasks.

### Fault Tolerance

#### Worker Failure

The master pings every worker periodically. If no response is received from a worker in a certain amount of time, the master marks the worker as failed.
([[Heartbeats]] mechanism)

Any map tasks completed by the worker are reset back to their initial _idle_ state, and therefore become eligible for scheduling on other workers. Similarly, any map task or reduce task in progress on a failed worker is also reset to _idle_ and becomes eligible for rescheduling.

Completed map tasks are re-executed on a failure because their output is stored on the local disk(s) of the failed machine and is therefore inaccessible. ==Completed reduce tasks do not need to be re-executed since their output is stored in a global file system==.

When a map task is executed first by worker A and then later executed by worker B (because A failed), ==all workers executing reduce tasks are notified of the execution==. Any reduce task that has not already read the data from worker $A$ will read the data from worker $B$.

MapReduce is resilient to large-scale worker failures. For example, during one MapReduce operation, network maintenance on a running cluster was causing groups of 80 machines at a time to become unreachable for sev- eral minutes. The MapReduce master simply re-executed the work done by the unreachable worker machines, and continued to make forward progress, eventually completing the MapReduce operation.

#### Master Failure

It is easy to make the master **write periodic checkpoints of the master data structures** described above. If the master task dies, ==a new copy can be started from the last checkpointed state==. 

However, given that there is only a single master, its failure is unlikely; therefore our current implementation aborts the MapReduce computation if the master fails. Clients can check for this condition and retry the MapReduce operation if they desire.

### Locality

Network bandwidth is a scarce resource in our computing environment. We conserve network bandwidth by taking advantage of the fact that the input data is stored on the local disks of the machines that make up our cluster. 

The file system typically stores multiple(typically 3 copies) of each block on different machines. The MapReduce master takes the location information of the input files into account and attempts to schedule a map task on a machine that contains a replica of the corresponding input data.

Failing that, it attempts to schedule a map task near a replica of that task’s input data (e.g., on a worker machine that is on the same network switch as the machine containing the data). When running large MapReduce operations on a significant fraction of the workers in a cluster, most input data is read locally and consumes no network bandwidth.



## Reference

1. [MapReduce: Simplified Data Processing on Large Clusters](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)
2. 


