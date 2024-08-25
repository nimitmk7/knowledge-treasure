In the context of databases, ==Consistency is Correctness==, which means that under no circumstance will the data lose its correctness.

Database systems allow us to define ==rules that the data residing in our database are mandated to adhere to==. Few handy rules could be

- balance of an account should never be negative
- no orphan mapping: there should not be any mapping of a person whose entry from the database is deleted.
- no orphan comment: there should not be any comment in the database that does not belong to an existing blog.

These rules can be defined on a database using Constraints, [Cascades](https://en.wikipedia.org/wiki/Foreign_key#CASCADE), and Triggers.

For example, [Foreign Key constraints](https://en.wikipedia.org/wiki/Foreign_key), [Check constraints](https://en.wikipedia.org/wiki/Check_constraint), On Delete Cascades, On Update Cascades, etc.

![[Pasted image 20240823155706.png]]
## Role of the database engine in ensuring Consistency

An ACID-compliant database engine has to ensure that the data residing in the database continues to adhere to all the configured rules. Thus, even while executing thousands of concurrent transactions, the database always moves from one consistent state to another.
## What happens when the database discovers a violation?

Database engine **rollbacks the changes**, which ensures the database is reverted to a previous consistent state.
## What happens when database does not discover a violation?

Database Engine will continue to apply the changes, and once the transaction is marked successful, this state of the database becomes the newer consistent state.
## How does the database ensure Consistency?

Integrity constraints are checked when the changes are being applied to the data.

==Cascade operations are performed synchronously along with the transaction. this means that the transaction is not complete until the primary set of queries, along with all the eligible cascades, are applied==. 

Most database engines also provide a way to make them asynchronous, allowing us to keep our transactions leaner.

## References
1. https://arpitbhayani.me/blogs/consistency
