
## Motivation
- Schema management is painful for certain types of data.

## Types
1. **Document Model**: Targets use cases where data comes in self-contained documents and relationships between one document and another are rare.
2. **Graph Based**: Targets use cases where relationships between data items are common and important.

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

## Graph Model
Built around the concept of “graph”

A database that uses graph structures to store its data is called a graph database. If in document databases, the content of each document is the priority, then in graph databases, ==the relationships between data items are the priority==.

Because the relationships are modeled explicitly in graph models, it’s faster to retrieve data based on relationships.

![[Pasted image 20240525103039.png]]

