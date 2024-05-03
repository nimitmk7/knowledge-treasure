A **key–value database**, or **key–value store**, is a data storage paradigm designed for storing, retrieving, and managing associative arrays, and a data structure more commonly known today as a dictionary or **hash table**. 

Dictionaries contain a collection of objects, or records, which in turn have many different fields within them, each containing data. These records are stored and retrieved using **a key that uniquely identifies the record**, and is used to find the data within the database.

## KV DB vs RDB
Key–value databases work in a very different fashion from the better known relational databases (RDB). 

RDBs predefine the data structure in the database as a series of tables containing fields with well defined data types. Exposing the data types to the database program allows it to apply a number of optimizations. 

In contrast, key–value systems treat the data as a single opaque collection, which may have different fields for every record. 

1. This offers **considerable flexibility** and more closely follows modern concepts like object-oriented programming.
2. Because optional values are not represented by placeholders or input parameters, as in most RDBs, key–value databases often **use far less memory to store the same data**, which can lead to large performance gains in certain workloads.

## Examples

1. Redis
2. DynamoDB
3. RocksDB
4. ScyllaDB




## References
1. https://en.wikipedia.org/wiki/Key%E2%80%93value_database
2. 