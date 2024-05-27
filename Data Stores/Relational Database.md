A **relational database** is defined as _a database structured to recognize relations among stored items of information_.

The way a relational database works is by storing information in tables, where each table has its own rows and columns. 

A row represents a _record_ (or _tuple_), while a column represents a _field_ (or _attribute_). 

In this example of a grocery list, a row represents a grocery item and how many of each item, while a column represents an attribute of a grocery item in the list. This table is shown by selecting all elements in the table

![[Database_example.webp]]

After seeing this example, you might think a database looks like a spreadsheet. However, what makes a database _relational_ is the fact that there are **relationships between the tables**. This enables a relational database to efficiently store large amounts of data, while retrieving selected data.

## Type of Relationships

### One-to-One
In a one-to-one relationship, only one row of a table is linked to at most one row on the other table. 
![[Pasted image 20240503122330.png]]
### One-to-Many
In a one-to-many relationship, one row of one table can link to many rows in a table. 

![[Pasted image 20240503122444.png]]

### Many-to-Many
In a many-to-many relationship, one or more rows of one table can link to 0, 1 or many rows in the other table. To implement this relationship, we must use a **mapping** or **intermediary** or **junction** table.

![[Pasted image 20240503122542.png]]
## Normalization rules
Database normalization is **the process of structuring a relational database in accordance with a series of so-called normal forms in order to reduce data redundancy and improve data integrity**.

### [[Normal Forms]]

### Downsides
1. Your data is now ==spread across multiple relations==. You can join the data from different relations back together, but ==joining can be expensive for large tables==.
2. 
## [[Relational Database Schema Design]]




## References
1. https://medium.com/@kimtnguyen/relational-database-schema-design-overview-70e447ff66f9
2. 