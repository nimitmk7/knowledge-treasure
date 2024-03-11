Vector databases are a specialized type of database that are designed for handling data as high-dimensional vectors, or [[Vector Embeddings]]. 

## How are they different ?
Vector databases are distinct from traditional databases in that they are optimized to store and query vectors with a large number of dimensions, which can range from tens to thousands, depending on the complexity of the data and the transformation function applied.

## Workflow

![[Pasted image 20240309213854.png]]
1. Use an embedding model to generate vector embeddings for our raw data(text/images/audio/video).
2. Store them in the vector database along with a reference to the original data and metadata.
3. When a query is issued by an application, we use the same embedding model to generate embeddings for the query. This query can be of the same data type as our dataset (e.g., an image to search for similar images) or of a different data type (e.g., text to search for similar images)
4. Use the query vector to search for similar vector embeddings in the database.
5. The similarity search will output a list of vectors that are most similar to the query vector. The raw data associated with each vector can then be accessed.



## References
1. https://dev.to/sfoteini/image-vector-similarity-search-with-azure-computer-vision-and-postgresql-12f7