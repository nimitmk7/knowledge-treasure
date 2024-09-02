**Core idea:** You acquire the lock before proceeding

Typical Flow: 
1. **Acquire Lock**: Ensure that at any given point in time, only one transaction goes in, while other transactions are kept waiting.
2. **Perform the READ/UPDATE operation**: 
3. **Release Lock**

## Why do we need locks ?
Locks help us protect the sanity of the data. Sanity can be decomposed into 2 terms:
1. **Consistency**
	1. *Example*(Protecting data against concurrent updates): If there is a value of 10 in a database, and 2 transactions arrive with a +1 operation together, if both execute together, they update the value to 11, as they both read 10. If they use locks, first one acquires lock on the row, reads 10, updates to 11, releases the lock. Then second one acquires lock on the row, reads 11, updates to 12, releases the lock. So locks ensured the value is updated correctly to 12.
2. **Integrity**

>[!WARNING] Transactional Deadlock
> ![[Pasted image 20240901094152.png]]
> T1 wants a lock on R1 but R1 is locked by T2. Whereas, T2 wants a lock on R2, but R2 is locked by T1. Thus none of the transactions will be able to proceed. 
> 
> The database runs a deadlock detection algorithm before letting a transaction take a lock(a normal potential cycle check). **The transaction that detects the deadlock will kill itself**.

## Shared/Read Lock
- The locked rows are reserved for **read** by the current transaction
- Other transactions can read the locked rows.
- Other transactions can’t modify the locked rows.
- If the current transaction wants to modify the row, the shared lock will be ==upgraded to an exclusive lock==.
### Implementation
```SQL
SELECT * FROM .... FOR SHARE;
```
The transaction that wrote “FOR SHARE” needs a read lock on the rows specified in the “SELECT” statement.

## Exclusive/Write Lock
- The locked rows are reserved for write by the current transaction.
- Other transactions cannot read the locked rows.
- Other transactions cannot modify the locked rows.

### Implementation
```SQL
SELECT * FROM .... FOR UPDATE;
```
The transaction that wrote “FOR UPDATE” needs a write lock on the rows specified in the “SELECT” statement. 

## SKIP LOCKED
It removes the locked rows from the result set. 
### Implementation
```SQL
SELECT * FROM t WHERE id = 2 FOR UPDATE SKIP LOCKED;
```

## NO WAIT
Locking read does not wait for the lock to be acquired. ==If fails immediately if the row is locked==. 

### Implementation
```SQL
SELECT * FROM t WHERE id = 2 FOR UPDATE NOWAIT;
```
If the row is locked, kill transaction.






