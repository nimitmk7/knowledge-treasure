Storage engines, also known as databases, are the implementation of how data is stored and retrieved on machines.

Typically, there are two types of workloads that databases are optimized for, 
1. Transactional processing
2. Analytical processing

## Transactional Databases(OLTP)
Transactional databases are designed to process online transactions and satisfy the low latency, high availability requirements. This type of processing is called OLTP(Online Transaction Processing).

They generally fulfill the [[ACID properties]]. **However, transactional databases don’t necessarily need to be ACID, and some developers find ACID to be too restrictive.**

Because each transaction is often processed as a unit separately from other transactions, transactional databases are often row-major.

That also means transactional databases might not be efficient for analytical questions. 
Example: “Average price for all the rides in September in San Fransisco”

## Analytical Databases(OLAP)
Analytical Databases are efficient with queries that allow you to look at data from different viewpoints. We call this type of processing online analytical processing (OLAP).

## Beyond OLTP and OLAP
Both the terms OLTP and OLAP have become outdated, for three reasons. 
1. The separation of transactional and analytical databases was due to limitations of technology—it was hard to have databases that could handle both transactional and analytical queries efficiently. However, this separation is being closed. Today, we have transactional databases that can handle analytical queries, such as **CockroachDB**. We also have analytical databases that can handle transactional queries, such as **Apache Iceberg** and **DuckDB**.
2. In the traditional OLTP or OLAP paradigms, storage and processing are tightly coupled—how data is stored is also how data is processed. This may result in the same data being stored in multiple databases and using different processing engines to solve different types of queries. An interesting paradigm in the last decade has been to ==decouple storage from processing== (also known as compute), as adopted by many data vendors including Google’s BigQuery, Snowflake, IBM, Teradata. ==The data can be stored in the same place, with a processing layer on top that can be optimized for different types of queries==.
3. “online” has become an overloaded term that can mean many different things. Online used to just mean “connected to the internet.” Then, it grew to also mean “in production”—we say a feature is online after that feature has been deployed in production.

