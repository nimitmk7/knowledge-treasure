Chunkserver receives the writes from the client directly, and not from Master. 

When a chunkserver receives a write(mutation/overwrite), it is never immediately written to the disk, instead it writes it to an in-memory LRU Buffer. 

Why ?

1. Your client is not just writing to one chunkserver.
2. Also, any write to a chunk needs to be applied to all 3 replicas of the chunk.
3. The write can go in any order, but you need to maintain a global order of mutations.

Coz there are other clients also sending writes to chunkservers and those same chunks as well in many cases. 

Example:
2 clients initiated writes for the same chunks parallely, and they would have to write to the same 3 chunk servers. We need to ensure that we see the same global order of mutations. 

Client 1 → Sends mutation to servers in order 1,2,3
Client 2 → Sends mutation to servers in order 2,1,3

Now how this can lead to inconsistent order in different servers. How does chunkserver know whose write to consider first ?

That is where we introduce the concept of “primary” replica. This concept will help us maintain the global order. 

So for each chunk, we have n replicas, but out of those one of them is a primary replica. The order in which that replica sees the writes, is the global order which is enforced. 

When clients write to the primary replica, it assigns a sequence number to the incoming mutations, which is atomically incremented. 

The sequence is conveyed to the chunkservers with secondary replicas. The secondary replicas then apply the mutations from memory to disk in that order.

**But how do we decide, which replica is primary for a chunk ?**

That is where lease management comes into place. 

You assign for a particular chunk, a lease to a particular chunkserver. Whoever has the lease, is the primary replica. 

When the write is initiated to which replica does it go ? 

All of them –> Too slow and prone to failure
Anyone at random → Unpredictable
One of them → Deterministic

Master grants chunk lease to one of the replicas → Primary

All writes for a chunk go to primary and the changes are picked up by other…in order…eventually. 

What about lease expiry ?
A primary can continue to extend the lease, if the primary dies, the lease is attached to some other replica. 

**All the communication is done over heartbeat messages(Core of a distributed system)**

What about two updates are received for the same chunk ?

Flow of a write request
1. Client sends the write through GFS client library
2. GFS client splits the write into chunks(64 MB each)
3. GFS client talks to master to get chunkservers
	1. Primary replica and secondary nodes
	2. Master assigns ID to chunk
4. Client caches the locations for some time 
5. GFS client writes the chunk to the replicas - primary and secondary
6. Each chunkserver holds the mutations(data) in an internal LRU buffer and send ACK that it got the writes.
7. Client waits to recieve ACK from all replicas. But it proceeds after receiving ACK from majority. 
8. Once ACK is received from all, clients sends WRITE request to the primary replica. 
9. Primary replica assigns seq number to this mutation and applies change to its own state. 
10. Primary replica forwards the WRITE request to all secondary replicas. 
11. Secondary then ACK primary suggesting completion of operation.
12. Primary replica now tells the master about the mutation(Mutation, chunk, location, version number)
13. Master node now writes this mutation in operation log, as well as it updates it in-memory metadata and ACK primary replica. 
14. Primary replica replies to the client. 

Now you might think this write operation is tedious and slow, it does happen at a moderate/brisk pace. But as we noted, latency is not that important but bandwidth, the ability to handle this scale of data without errors consistently is. 

Most errors here are retriable, and after some attempts the entire write is retried. 




