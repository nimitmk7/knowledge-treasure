## Definition
Vector embeddings are numerical representations of data, such as images, text, videos, and audio. These vectors are high-dimensional dense vectors, with each dimension containing information about the original content.

Data ---> Vector

## Motivation

If we represent objects like images, videos, documents, or audio as vector embeddings, we can **quantify their semantic similarity by their proximity in a vector space**. 

## Example
The embedding vector of an image of a cat will be more similar to the embedding vector of another cat or the word "meow" than that of a picture of a dog or the word "woof".

![[Pasted image 20240309213357.png]]


## Use cases
1. Clustering
2. Recommendation
3. Image search

## Vector Similarity Search

 Vector similarity search works by searching for images based on the content of the image itself, instead of relying only on manually assigned keywords, tags, or other metadata, as keyword-based search systems do. 

### Procedure
In a vector search system, the **vector embedding of a user's query is compared to a set of pre-stored vector embeddings to find a list of vectors that are the most similar** to the query vector. 

Since embeddings that are numerically close are also semantically similar, we can measure the semantic similarity by using a distance metric, such as cosine or Euclidean distance.

### Pros
This approach is usually more efficient and accurate than traditional search techniques, which depend heavily on the user's capability to find the best search terms.



