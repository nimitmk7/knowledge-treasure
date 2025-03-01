In the context of Database, Durability ==ensures that once the transactions commit, the changes survive any outages, crashes, and failures==, which means any writes that have gone through as part of the successful transaction should never abruptly vanish.

This is exactly why Durability is one of the essential qualities of any database, as it ensures zero data loss of any transactional data under any circumstance.

A typical example of this is your purchase order placed on Amazon, which should continue to exist and remain unaffected even after their database faced an outage. 

So, to ensure something outlives a crash, it has to be stored in non-volatile storage like a Disk; and this forms the core idea of durability.

# How do databases achieve durability?

The most fundamental way to achieve durability is by ==using a fast transactional log==. The changes to be made on the actual data are first flushed on a separate transactional log, and then the actual update is made.

This flushed transactional log enables us to reprocess and replay the transaction during database reboot and reconstruct the system’s state to the one that it was in right before the failure occurred - typically the last consistent state of the database. 

The write to a transaction log is made fast by keeping the file append-only and thus minimizing the disk seeks.

# Durability in a distributed setting

If the database is distributed, it supports Distributed Transactions, ensuring durability becomes even more important and trickier to handle. In such a setting, the participating database servers coordinate before the commit using a Two-Phase Commit Protocol.

The distributed computation is converged into a step-by-step process where the coordinator communicates the commit to all the participants, waits for all acknowledgments, and then further communicates the commit or rollback. This entire process is split into two phases - Prepare and Commit.

## References
1. https://arpitbhayani.me/blogs/durability

