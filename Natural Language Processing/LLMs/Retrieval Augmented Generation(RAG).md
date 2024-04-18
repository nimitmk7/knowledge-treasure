## Motivation
Large language models (LLMs) have undeniably propelled **NLP task-specific architectures** by excelling at storing and learning factual information from extensive data. However, their performance falters when faced with knowledge-intensive tasks due to inherent limitations.

LLMs struggle to access and manipulate knowledge effectively, as they cannot readily expand or update their memory. Moreover, they may produce erroneous outputs known as "hallucinations" and often fail to provide clear insights into their predictions.

To solve the limitations of LLMs, Retrieval Augmented Generation (RAG) has gained significant attention and is redefining the way we approach text generation tasks.

## What is RAG ?

==Retrieval Augmented Generation (RAG) is an AI framework that helps large language models (LLMs) generate more accurate information by using external contextual information==. 
## How does it work ?

RAG models are made up of three components:

- Retriever: Retrieves relevant information from external knowledge sources
- Ranker: Refines the retrieved information by assessing its relevance and importance
- Knowledge base: References an authoritative knowledge base outside of the model's training data sources before generating a response.

### A Helpful Analogy
Imagine you are trying to solve a complex mystery. The detective's role is to gather clues, evidence, and historical records related to the case. Once the detective has compiled this information, the storyteller designs a compelling narrative that weaves together the facts and presents a coherent story. In the context of AI, RAG operates similarly.

The Retriever Component acts as the **detective**, scouring databases, documents, and knowledge sources for relevant information and evidence. It compiles a comprehensive set of facts and data points.

The Generator Component assumes the role of the **storyteller**. Taking the collected information and transforming it into a coherent and engaging narrative, presenting a clear and detailed account of the mystery, much like a detective novel author.

This analogy illustrates how RAG combines the investigative power of retrieval with the creative skills of text generation to produce informative and engaging content, just as our detective and storyteller work together to unravel and present a compelling mystery.

## The Framework

![[Pasted image 20240418115527.png]]


## References
1. [A Gentle Intro to RAG - Weight & Biases Blog](https://wandb.ai/cosmo3769/RAG/reports/A-Gentle-Introduction-to-Retrieval-Augmented-Generation-RAG---Vmlldzo1MjM4Mjk1)
2. [What is RAG? - AWS Blog](https://aws.amazon.com/what-is/retrieval-augmented-generation/#:~:text=Retrieval%2DAugmented%20Generation%20(RAG),sources%20before%20generating%20a%20response)
3. 

