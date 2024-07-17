![[Pasted image 20240705105457.png | 300]]

A distributed fault-tolerant data warehouse system that enables analytics at a massive scale and facilitates reading, writing, and managing petabytes of data residing in distributed storage using SQL.

It is built on top of Apache Hadoop and supports storage on Amazon S3, Azure Data Lake Storage, Google Cloud Storage.

In summary, 
> “Hive is a data warehousing infrastructure based on [Apache Hadoop](http://hadoop.apache.org/). Hadoop provides massive scale out and fault tolerance capabilities for data storage and processing on commodity hardware.”

> [!faq] Why use Hive and not directly GCS or S3 ?
> GCS/S3 is just a cloud storage service for storing objects. Hive acts as an abstraction layer that provides features like SQL querying, data warehousing capabilities, schema management, metadata management, batch processing, support for complex data types, etc. over GCS/S3.
> 
> This makes it easier for analysts and programmers to work with data.

## Data Units
Databases → Tables → Partitions → Buckets

## Types

### Primitive Types
1. Integers
	1. TINYINT(1 byte)
	2. SMALLINT(2 byte)
	3. INT(4 byte)
	4. BIGINT(8 byte)
2. Boolean type
    1. BOOLEAN—TRUE/FALSE
3. Floating point numbers
    1. FLOAT—single precision
    2. DOUBLE—Double precision
4. Fixed point numbers
	1. DECIMAL—a fixed point value of user defined scale and precision
5. String types
	1. STRING—sequence of characters in a specified character set
	2. VARCHAR—sequence of characters in a specified character set with a maximum length
	3. CHAR—sequence of characters in a specified character set with a defined length
6. Date and time types
	1. TIMESTAMP — A date and time without a timezone ("LocalDateTime" semantics)
	2. TIMESTAMP WITH LOCAL TIME ZONE — A point in time measured down to nanoseconds ("Instant" semantics)
	3. DATE—a date
7.  Binary types
    1. BINARY—a sequence of bytes

### Complex Types

Complex Types can be built up from primitive types and other composite types using:

1. **Structs**: the elements within the type can be accessed using the DOT (.) notation. For example, for a column c of type STRUCT {a INT; b INT}, the a field is accessed by the expression “c.a”.
2. **Maps** (key-value tuples): The elements are accessed using ['element name'] notation. For example in a map M comprising of a mapping from 'group' -> gid the gid value can be accessed using M['group'].
3. **Arrays** (indexable lists): The elements in the array have to be in the same type. Elements can be accessed using the [n] notation where n is an index (zero-based) into the array. For example, for an array A having the elements ['a', 'b', 'c'], A[1] returns 'b'.

## Hive Partitioning

