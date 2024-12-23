## Premise
For any database, **reliability** is super-important. 

### What happens upon Transaction Commit ?

The changes you make are flushed onto the disk/non-volatile storage. 

>[!faq] Why do we use non-volatile storage ? 
> Because non-volatile storage is safe from: 
> 1. Power loss
> 2. OS failure, and
> 3. Hardware failure. 
>

No matter what happens(except disk goes bust), DB has to ensure committed changes have to reflect on the disk.

![[Pasted image 20241123142241.png]]

But, disk writes are complicated. 

When we perform the disk write, the ==changes are not directly written to the disk sectors==, instead, it goes through a variety of buffers like RAM, OS Cache, Disk Cache, and then finally to the Disk sectors. So, if the changes are in any of these intermediate caches and the process crashes, our changes are lost.

![[Pasted image 20241123142215.png]]

 So, while guaranteeing reliability we have to skip all of these caches and flush our changes as quickly as possible on the disk sectors. 
 
 But if we do that, ==we are impacting the throughput of the system given how expensive disk writes are==.

## Introduction

Write-ahead logging is a standard way to ensure data integrity and reliability. 
### Core idea
Any changes made on the database are first logged in an **append-only file** called Write-ahead Log or Commit Log. Then the actual blocks having the data (row, document) on the disk are updated.

This append-only file is opened in **sync** mode, and the writes bypass the intermediate cache steps.
### Flow
1. Update is triggered on DB.
2. Log the entry in the WAL file
3. Make changes in the table and indices

![[Pasted image 20241123144540.png | 400]]

## Advantages
1. We do not need to flush the data changes on every commit.
	1. Keep the changes in buffer, periodically(in an async manner) flush them to disk.
2. In case of crash, we can recover by replaying the logs.
3. Reduce the number of disk writes.
4. WAL file is sequential and append only, so the cost of logging the changes is significantly lower than the cost of changing data files. 
	1. Log → 1 disk write
	2. Data File → Update table, Update Index, Rebalance tree
5. Point in time snapshot is possible with WAL. 

## Data Integrity in WAL 
When you write a line onto the disk, if the process crashes, you’ll have a incomplete log in the file. We need a way to ensure what you’ve written on the disk is a proper log.

Therefore, every individual record in WAL is CRC-32 protected. 

The flow looks like:
1. Compute CRC for the log.
2. Write the CRC.
3. Write the log.

![[Pasted image 20241123150918.png | 400]]

Thus, while reading the logs during recovery process, we can know if the record on the log file is corrupt or not based on its CRC.

> [!INFO] CRC-32
> A **cyclic redundancy check** (**CRC**) is an error-detecting code commonly used in digital networks and storage devices to detect accidental changes to digital data.

## Log File structure
WAL is not just 1 file, it typically contains multiple set of files, where each file is called a **segment**. 

Each segment is at max 16 MB in size. Within the segment, you have pages. Each page can be at max 8 KB in size.

You add the entries in the pages. As soon as you make an entry in a page, WAL gives it an identifier → LSN(**Log Sequence Number**). 

The sequence number is not integers(like 1,2,3,…..) but rather the byte offset in the log file.

![[Pasted image 20241123152304.png]]

The point(LSN) till which you have applied the changes from the log file to the disk is stored in a separate table. 

## References

<iframe width="560" height="315" src="https://www.youtube.com/embed/wI4hKwl1Cn4?si=VgTXvl4InnWj8kRV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
