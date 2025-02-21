## Premise
Design a multi-user blogging platform, like Medium. 

## Requirements
1. A user can publish multiple blogs
2. The platform supports multiple users.

## Basic Database Schema

### Users table
1. id
2. name
3. bio
### bio
We store it in a **varchar/short text** field. It stores the text inside the row itself, and it is faster to skip through as well as faster to read.

It works for this use-case as bio is a shorter field, unlike body of the blog.

![[Pasted image 20240716120001.png | 300]]
### Blogs table
1. id
2. author_id
3. title
4. is_deleted (**Why?**)
5. published_at
6. body

#### Importance of is_deleted ?
- This field takes care of ==soft deletion== procedure.
	- It helps in archival, **recovery** and audit. 
		- Recoverability is a crucial feature, especially in accidental deletions.
		- Archived data is used by company for internal use-cases, like training ML algorithms, or historical analysis.
		- Deleted content/data might be required for legal or auditory purposes.
	- Soft deletion is easy on the database engine, as it avoids frequent B-tree rebalancing.
#### body
If you store the actual body as a row, ==the row will be huge== as the body is a huge field and can be of size like 1-3 KBs.

So while going to one row from another, you need to read and skip so much data before hitting the next row.

So we store data for fields with large amount of data like **body** as a reference, and then store the reference in the row, and we use a data type like “LONGTEXT”.

![[Pasted image 20240716115151.png | 300]]


The tradeoff is that the reads are a little slower, so when we are reading a row, then we need to go to the reference location to read the **body**. So we are doing 2 disk reads. 

1. For **row**
2. For **body** field content
#### published at

It is a datetime column. Almost all databases support a native datetime object, it helps convert the value to the native datetime object type of a language like Java or Python.

But a database has to write this value in disk as bytes. Different databases choose their own serialization format for that purpose.

