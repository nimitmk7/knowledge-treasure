Isolation is the ==ability of the database to concurrently process multiple transactions in a way that changes made in one does not affect the other==. 

A simple analogy is how we have to make our data structures and variables thread-safe in a multi-threaded (concurrent) environment.

And similar to how we use Mutex and Semaphores to protect variables, the database uses **locks** (shared and exclusive) to protect transactions from one another.

![[Pasted image 20240823160215.png]]

## Why is isolation important?
Isolation is one of the most important properties of any database engine, the absence of which directly impacts the integrity of the data.

### Example 1: Cowin Portal

When 500 slots open for a hospital, the system has to ensure that a max of 500 people can book their slots.

### Example 2: Flash Sale

When Xiaomi conducts a flash sale with 100k units, the system has to ensure that orders of a max of 100k units are placed.

### Example 3: Flight Booking

If a flight has a seating capacity of 130, the airlines cannot have a system that allows ticket booking of more than that.

### Example 4: Money transfers

When two or more transfers happen on the same account simultaneously, the system has to ensure that the end state is consistent with no mismatch of the amount. Sum of total money across all the parties to remain constant.

The isolation property of a database engine allows the system to put these checks on the database, which ensures that the data never goes into an inconsistent state even when hundreds of transactions are executing concurrently.

## How is isolation implemented ?
==A transaction before altering any row takes a lock (shared or exclusive) on that row, disallowing any other transaction to act on it==. The other transactions might have to wait until the first one either commits or rollbacks.

The granularity and the scope of locking depend on the isolation level configured. Every database engine supports multiple Isolation levels, which determines how stringent the locking is. The 4 isolation levels are

- Serializable
- Repeatable reads
- Read committed
- Read uncommitted
