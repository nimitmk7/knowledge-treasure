## Premise
A single database transaction often contains multiple statements to be executed on the database.

In Relational Databases, these are usually multiple SQL statements, while in the case of non-Relational Databases, these could be multiple database commands.

## The Concept
Atomicity in ACID mandates that ==each transaction should be treated as a single unit of execution, which means either all the statements/commands of that transaction are executed, or none of them are==.

At the end of the successful transaction or after a failure while applying the transaction, ==the database should never be in a state where only a subset of statements/commands is applied==.

An atomic system thus guarantees atomicity in every situation, including successful completion of transactions or after power failures, errors, and crashes.

![[Pasted image 20240823152453.png]]
### Example
A great example of seeing why it is critical to have atomicity is Money Transfers.

Imagine transferring money from bank account A to B. The transaction involves subtracting balance from A and adding balance to B. If any of these changes are partially applied to the database, it will lead to money either not debited or credited, depending on when it failed.
## How is atomicity implemented ?

### Atomicity in databases
Most databases implement Atomicity using logging; ==the engine logs all the changes and notes when the transaction started and finished==. 

Depending on the final state of the transactions, the changes are either applied or dropped.

Atomicity can also be implemented by keeping a copy of the data before starting the transaction and using it during rollbacks.

## Atomicity in file systems
It is attained by atomically opening and locking the file using system calls: open and flock. We can choose to lock the file in either Shared or Exclusive mode.

## Atomicity at Hardware level
At the hardware level, atomicity is implemented through instructions such as Test-and-set, Fetch-and-add, Compare-and-swap.

## Atomicity in Business Logic


## References
1. https://arpitbhayani.me/blogs/atomicity
2. 

