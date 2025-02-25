# Motivation
- Schema management is painful for certain types of data.

# Facts
- Unlike RDBMS databases, NoSQL databases have no standardization. So you need to know a lot about it before you start using it.
- A single NoSQL database might support multiple NoSQL data models
# Misconceptions
- NoSQL DBs scale well, but it does not imply SQL databases do not scale well.

## Types
1. **Document Model**: Targets use cases where data comes in self-contained documents and relationships between one document and another are rare.
2. **Graph Based**: Targets use cases where relationships between data items are common and important.
3. **Key-Value Store**
4. **Wide-Column**
 
## Document Model
Built around concept of “Document”
1. A document is often a single continuous string, encoded as JSON, XML, or a binary format like BSON (Binary JSON). 
2. All documents in a document database are assumed to be encoded in the same format. 
3. Each document has a unique key that represents that document, which can be used to retrieve it.
4. A collection of documents could be considered analogous to a table in a relational database, and a document analogous to a row

Examples:
![[Pasted image 20240524234747.png | 500]]

5. Although, there is no enforced schema, The application that reads the documents usually ==assumes some kind of structure of the documents==. 
6. Document databases just ==shift the responsibility of assuming structures== from the application that writes the data to the application that reads the data.
7. The document model has ==better locality than the relational model==. In the document model, all information about an object can be stored in a document, making it much easier to retrieve.
8. However, compared to the relational model, it’s ==harder and less efficient to execute joins== across documents compared to across tables.
9. Document databases supports complex queries like aggregations, unique counts. So they are very close to RDBMs in this aspect.
10. Partial updates to the documents is possible. You do not have to read the entire document, make the change and then write the entire value to update just that one attribute in the document.
	1. It minimizes amount of data transferred over the network
	2. It gives better DB performance as well.

Example: MongoDB, ElasticSearch

## Graph Model
Built around the concept of “graph”

A database that uses graph structures to store its data is called a graph database. If in document databases, the content of each document is the priority, then in graph databases, ==the relationships between data items are the priority==.

Because the relationships are modeled explicitly in graph models, it’s faster to retrieve data based on relationships.

- It stores data in nodes and edges.
- Great for modelling Social Behaviours, Recommendations
- Solid use case: Fraud detection
- Extremely painful to manage in production, so only use if you need a sophisticated graph algorithm. 
	- Don’t use Graph DB to model “Does A follow B or not”
	- Use Graph DB to model “LinkedIN distance of connection”
	- 

![[Pasted image 20240525103039.png]]
Example: Neo4j, Amazon Neptune, DGraph, TigerGraph
## Key-value stores
It is a simple type of database where each item contains keys and values. Each key is unique and associated with a single value. Thus it gives you a key-wise access pattern.

They provide high performance in reads and writes because they generally tend to store things in memory.

They mostly don’t support complex queries.

The database is heavily partitioned as it supports 

Common use-cases are caching and session management.

**Examples:** Redis, Amazon DynamoDB, Aerospike

## Wide-column stores
They store data in tables, rows and dynamic columns. The data is stored in tables.

Unlike traditional SQL databases, wide-column stores are flexible, where different rows can have different sets of columns. These databases can ==employ column compression techniques to reduce the storage space and enhance performance==. The wide rows and columns enable efficient retrieval of sparse and wide data.

**Examples:** Apache Cassandra, HBase

## Column Oriented Databases
- SQL databases are row-oriented, so all data of a particular row is stored together. 
	- ![[Pasted image 20250222082606.png]]
	- Say, you want to use 3 columns out of 1000 columns in the query, we still need to retrieve all the columns for the relevant rows, and then discard the remaining 997 columns, and process the 3 relevant ones.
- Instead, store all the data of a column together.
	- ![[Pasted image 20250222083224.png]]
	- So whenever a query is given, you only read the columns mentioned in the query, and skip all others.
- Hence, Column oriented DBs are used in massive analytics and data warehouses. 
- **Examples**: Google Bigquery, Amazon Redshift

# Why do they scale well ?
- There are no relations
- Data can be denormalized
- Data is modelled to be sharded.

# When to use SQL vs NoSQL 

Use SQL when: 
1. ACID properties
2. Relations, Constraints
3. Fixed Schema

Use NoSQL when:
1. No relations
2. Data can be denormalized
3. Data can be sharded




