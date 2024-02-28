> "Tidy datasets are all alike, but every messy dataset is messy in its own way."
> **Hadley Wickham**


## Tidy Data

1. Each **variable** must have its own **column**
2. Each **observation** must have its own **row**.
3. Each **value** must have its own **cell**.
4. Each **type** of observational unit form a **table**.

 ![[Pasted image 20240201160910.png]]

## Data Wrangling

**It is the process of converting raw data into a usable form**. 
7 most common operations are: 

### Pivot and Melt

**Pivot** is used to transform long-form data to wide-form, and **Melt** is used to transform wide-form data to long-form.

Wide form: More columns, lesser rows
Long form: Less columns, more rows
![[Pasted image 20240201180007.png]]
### Separate and Unite

In data cleaning, **Separate** splits a single column into multiple columns, while **Unite** combines multiple columns into a single column.
![[Pasted image 20240201180825.png]]
These operations are mostly used for string columns, and often involve using a separator character like "/" is used in the column.

### Select and Filter

**Select**: Taking a subset of columns and creating a new table/dataset out of that.


![[Pasted image 20240201181358.png]]
**Filter** : Picking the subset of rows we are gonna keep and creating a new table/dataset out of that.
![[Pasted image 20240201181633.png]]

### Mutate:

Add new column or modify existing columns.

![[Pasted image 20240201181758.png]]
![[Pasted image 20240201181837.png]]

### Bind or Concatenate

A way to combine 2 different dataframes into one.

![[Pasted image 20240201182059.png]]

![[Pasted image 20240201182118.png]]

### Join

Joining 2 datasets by using the index columns as the lookup index. 

![[Pasted image 20240201182444.png]]

Different types of join: **Outer**, **Inner**, **Left**, **Right**

### Missing Data

There are 2 types of missing data: 

1. **Explicitly** missing data: Often marked with a special value like N/A
2. **Implicitly** missing data: Empty field

Few ways to handle the missing data are:
1. Drop the columns with missing data
2. Drop the rows with missing data
3. Assign a default/standard value







