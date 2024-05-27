The process of converting a data structure or object state into a format that can be stored or transmitted and reconstructed later is data serialization. 

How to choose a serialization format to work with ?
1. Human readability
2. Access patterns
3. Text-based or binary-based 

![[Pasted image 20240524213218.png]]
## JSON
JSON stands for JavaScript Object Notation. 

### Example
![[Pasted image 20240524213534.png]]

### Pros
1. Its key-value pair paradigm is simple but powerful, ==capable of handling data of different levels of structuredness==.
2. It is human-readable
### Cons
1. Once you’ve committed the data in your JSON files to a schema, it’s pretty painful to retrospectively go back to change the schema.
2. JSON files are text files, so they take up lot of space


## Row-Major vs Column-Major Format

**Row-major**: Consecutive elements in row are stored next to each other. E.g CSV
**Column-major**: Consecutive elements in colunmn are stored next to each other. E.g. Parquet

Because ==modern computers process sequential data more efficiently than non-sequential data==, if a table is row-major, accessing its rows will be faster than accessing its columns in expectation. Same is the case for column-major tables, where accessing its columns will be faster than accessing its rows in expectation.

![[Pasted image 20240524233616.png]]

Also, Row major formats allow faster data writes. 

> Overall, row-major formats are better when you have to do a lot of writes, whereas column-major ones are better when you have to do a lot of column-based reads.


> [!INFO] NumPy vs Pandas
> One subtle point that a lot of people don’t pay attention to, which leads to misuses of Pandas, is that this library is built around the columnar format, whereas NumPy is built around the row-major format.

## Text vs Binary Format
CSV, JSON → Text Files
Parque → Binary Files

Binary files are the catchall that refers to all nontext files. As the name suggests, binary files are typically files that contain only 0s and 1s, and are meant to be read or used by programs that know how to interpret the raw bytes. A program ==has to know exactly how the data inside the binary file is laid out== to make use of the file.

1. Binary files are more compact.
	1. Consider that you want to store the number 1000000. If you store it in a text file, it’ll require 7 characters, and if each character is 1 byte, it’ll require 7 bytes. If you store it in a binary file as int32, it’ll take only 32 bits or 4 bytes.
2. AWS recommends using the Parquet format because “the Parquet format is up to 2x faster to unload and consumes up to 6x less storage in Amazon S3, compared to text formats.”8









