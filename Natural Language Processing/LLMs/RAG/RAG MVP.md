<iframe width="560" height="315" src="https://www.youtube.com/embed/0nA5QG3087g?si=I_HpuPS6nINzSSkr" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## What RAG is not ?
- A new paradigm
- A framework
- An end-to-end system

## What RAG is ?
- Act of stitching together Retrieval and Generation to ground the latter. 
- The Retrieval comes from the field of **Information Retrieval**.
- The Generation part is handled by LLMs.
- “Good RAG” is made up of good components:
	- Good Retrieval pipeline
	- Good Generative model
	- Good way of linking them up

![[Pasted image 20240625144747.png]]

## The Compact MVP
- The most compact (& most common) deep retrieval pipeline boils down to a very simple process which is a bi-encoder approach:
![[Pasted image 20240625143808.png]]
### Hands-on
![[Pasted image 20240625144932.png]]
### Where’s the vector DB ?
- The vector db in this example is ` np.array `! 
- A key point of using a vector DB (or an index) is to allow Approximate search, so you don’t have to compute too many cosine similarities. 
- You don’t actually need one to search through vectors at small scales: any modern CPU can search through 100s of vectors in milliseconds.

![[Pasted image 20240625145126.png]]

### Why are you calling embeddings bi-encoders ?
- Bi-encoders are used to create **single-vector representations**. They **pre-compute** document representations.
- Document and query representations are computed **entirely separately**, they aren’t aware of each other.
- Thus, all you need to do at inference is to **encode your query** and search for similar document vectors.
- This is **very computationally efficient**, but comes with retrieval performance tradeoffs.
	- If queries look very different from your training data, or queries is looking for some very specific information in a particular document, the separate training approach is not very helpful. 
	- So, documents and query being unaware of each other is **bad**. How to fix ?
### Reranking: The power of cross-encoders
- Most common approach is to use Cross-Encoders.
![[Pasted image 20240625145739.png]]
- Its **not computationally realistic** to compute query-aware document representations for every single query-document pair, every time a new query comes up. 
### Other Reranking Approaches
- **Core idea**: Leverage a powerful but computationally expensive model to score only a subset your documents, previously retrieved by a more efficient model. 
### Compact Pipeline + Reranking
- With the addition of a re-ranking step, this is what your Retrieval pipeline now looks like:
![[Pasted image 20240625150256.png]]

## Keyword Search: The Old Legend lives on
- Semantic search via embeddings is powerful, but compressing information from hundreds of tokens to a single vector is bound to lose information.
- Embeddings learn to represent information **that is useful to their training queries**. 
- This training data will **never be fully representative**, especially when you use the model on your own data, on which it hasn’t been trained.

- Additionally, **humans love to use keywords**. We have very strong tendencies to notice and use certain acronyms, domain-specific words, etc…
- To capture all this signal, **you should ensure your pipeline uses Keyword search**.

- Keyword search, also called “**full-text search**”, is built on old technology: BM25, powered by TF-IDF (a way of representing text and weighing down words that are common) 
- An ongoing joke is that **information retrieval has progressed slowly because BM25 is too strong a baseline**.
- BM25 is especially powerful on longer documents and documents **containing a lot of domain-specific jargon**.
- Its inference-time compute overhead is **virtually unnoticeable**, and it’s therefore a near free-lunch addition to any pipeline.

## TF-IDF MVP++
With text search and reranking, this is what your pipeline now looks like:

![[Pasted image 20240625152434.png]]

- Combine the scores of cosine similarity search and BM25 in a weighted fashion based on the specific context(e.g. 0.7 for MB25 and 0.3 for Cosine similarity).

### Metadata Filtering
- An extremely important component of production Retrieval is metadata filtering. 
- Outside of academic benchmarks, documents do not exist in a vacuum. There’s a lot of metadata around them, some of which can be very informative. 
- Take this query: 
	“Can you get me the cruise division financial report for Q4 2022?”
- There is a lot of ways semantic search can fail here, the two main ones being: 
	- The model must accurately represent all of “financial report”, but also “cruise division”, “Q4” and “2022”, into a single vector, otherwise it will fetch documents that look relevant but aren’t meeting one or more of those criteria. 
	- If the number of documents you search for (“k”) is set too high, you will be passing irrelevant financial reports to your LLM, hoping that it manages to figure out which set of numbers is correct.
- It’s perfectly possible that vector search would succeed for this query, but it’s a lot more likely that it will fail in at least one way.
- However, this is very easy to mitigate: there are entity detection models, such as GliNER, who can very easily extract zero-shot entity types from text:
![[Pasted image 20240625154735.png]]

- All you need to do is ensure that **business/query-relevant information is stored alongside their associated documents**.
- You can then use the extracted entities to **pre-filter your documents**, ensuring you only **perform your search on documents whose attributes are related to the query**.

## Final Compact MVP++
With this final additional component, this is what your MVP Retrieval pipeline should now look like:

![[Pasted image 20240625154901.png]]

This does look scarier (especially if you have to fit into a slide), but it’s very simple to implement.

![[Pasted image 20240625155036.png]]

I use 