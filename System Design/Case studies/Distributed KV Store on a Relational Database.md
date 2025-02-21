# Requirements 
1. Infinitely Scalable
2. Operations: GET, PUT, DEL
3. Key will have TTL(Time to live)

Example: Apache Ignite

# Storage

## Schema

| Key     | Value                      | expiry    | is_deleted |
| ------- | -------------------------- | --------- | ---------- |
| Varchar | Varchar/Longtext/JSON/BLOB | timestamp | bool       |

### What datatype to choose for value ? 

Varchar:
1. If data is not too long
Longtext:
1. If you need operations on the values
BLOB:
1. Does not support operations on values(like searching or filtering)
2. If you have null character(‘\0’) in your data(e.g. storing a bloom filter), longtext would terminate on seeing the first null character. 
## Expiry
Tradeoff:
1. Store the expiry timestamp directly v/s 
	1. Can use an index, and sort the keys needed to
2. Storing created_at and ttl
	1. Expiration query looks like: `DELETE FROM store where created_at + ttl < CURRENT_TIMESTAMP`.
	2. Need to evaluate all entries of the table, a full table scan as we don’t know what the addition of the table looks like. 

## Deletion
1. If we do hard deletion every time, the index B-tree rebalances every time, which is an expensive operation to do frequently. 
2. Do a soft delete, and then hard delete all the soft deleted rows in batch(using a CRON job).
3. Is a is_deleted column needed ? 
	1. Users have an option of deleting key, so need to record that. 
	2. Adds a new column and corresponding index for the column. 
	3. **Solution**: Make the expiry timestamp of the deleted row as the current timestamp. 
	4. But then how do you differentiate between an expired key and soft deleted key ? 
	5. Instead set the timestamp of the user-deleted key as “-1”.
	6. User delete: `UPDATE store set expiry = -1 where key=k`. 
	7. Further optimization: `UPDATE store set expiry = -1 where key = k and expiry > NOW()`
		1. Thus avoiding Disk I/O by not avoiding a unnecessary write, which was done by deleting an already expired key.
4. Hard delete: `DELETE from store where expiry < NOW()`

## Insert
 `INSERT into store values(k, v, expiry)`

But what if there is a value that already exists for that key. It would fail with an exception.

Should we go for an Upsert ? Or should we do select and check, and then insert/update in a single transaction depending on the result? 

Latter option is suboptimal coz it involves 2 queries instead of 1 query in the former. 

`UPSERT into store values(k, v, expiry)` 

## Fetch
`SELECT * FROM store where key = k and expiry > NOW()`

This way you are filtering expired and deleted keys from reaching to the user. 

## Multiple PUT requests

Use locks to protect data integrity. 
`SELECT * FROM store ... FOR UPDATE NO WAIT;`

`UPDATE store .....`

# High-level architecture

## Initial Design

![[Pasted image 20250219194256.png]]

- Bunch of KV-API servers with a Load balancer
- Batch cleanup jobs continously deletes soft deleted and expired keys from the database.

- We add as many KV-API servers as many are needed to support the load. 
- But we can’t just keep adding them, beyond an extent, now we need to scale the database.

## High number of reads
1. If we have a high read:write request atio(e.g. 99:1 reads-writes) and one DB is not able to handle it, we add read replicas. ![[Pasted image 20250219195240.png]]
2. We can either add a proxy node or the KV-API servers can directly interact with the replicas(API server routing). 
	1. In the latter cases, API servers know the DB topology.
3. Batch Cleanup job only runs on the Master DB.(As replicas are updated periodically to match the master, the changes will reflect there automatically.)
## High number of writes
1. Vertical Scale up the database. 
	1. It means ==increasing the processing power and capacity of a single server by adding more CPU cores, RAM, or storage space==.
2. Beyond that, you need to shard the data, based on a certain strategy(E.g. Range-based). ![[Pasted image 20250219200246.png]]
3. Each master has its own batch cleanup jobs, has its own set of replicas and is exclusive to its range.

