## 1NF
A table is in 1NF if
- There are only single valued attributes
- Each column has a unique name
- The values in each column are of same type.
- The order in which data is stored does not matter.

![[Pasted image 20240503134006.png]]
**Intuition**: The first stage of database normalization, or 1NF, makes sure that there are **no recurring groups inside rows** and that **all of a table’s columns contain atomic values, or indivisible values**.

## 2NF

1NF + Non-key column dependent on primary key. 

Put simply, a relation (or table) is in 2NF if:

1. It is in 1NF and has a single attribute unique identifier (UID)(in which case every non key attribute is dependent on the entire UID), or
2. It is in 1NF and has a multi-attribute unique identifier, and every regular attribute (not part of the UID) is dependent on _all attributes_ in the multi-attribute UID, not just one attribute (or part) of the UID.

### Example
Let's assume, a school can store the data of teachers and the subjects they teach. In a school, a teacher can teach more than one subject. 
![[Pasted image 20240503135605.png]]

In the given table, non-prime attribute TEACHER_AGE is dependent on TEACHER_ID which is a proper subset of a candidate key. That's why it violates the rule for 2NF.

To convert to 2NF, we decompose it into two tables:
![[Pasted image 20240503135639.png]]

## 3NF
A table is 3NF if it is 2NF and the non-key columns are independent of each other.




