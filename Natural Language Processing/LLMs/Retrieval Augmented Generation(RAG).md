## Motivation
Large language models (LLMs) have undeniably propelled **NLP task-specific architectures** by excelling at storing and learning factual information from extensive data. However, their performance falters when faced with knowledge-intensive tasks due to inherent limitations.

### Limitations of LLMs
LLMs struggle to access and manipulate knowledge effectively, as they cannot readily expand or update their memory. Moreover, they may produce erroneous outputs known as "hallucinations" and often fail to provide clear insights into their predictions.

1. LLM operates with outdated information, as it lacks access to the most recent and reliable facts beyond its knowledge cutoff date.
2. Responses provided by the LLM do not reference its sources, which means that its claims can’t be independently verified for accuracy or relied upon with complete trust.

Thus, to solve the limitations of LLMs, Retrieval Augmented Generation (RAG) has gained significant attention and is redefining the way we approach text generation tasks.

## What is RAG ?

Retrieval Augmented Generation (RAG) is an AI framework that helps large language models (LLMs) ==generate more accurate information by using external contextual information==. 
## How does it work ?
### A Helpful Analogy
Imagine you are trying to solve a complex mystery. The **detective**'s role is to gather clues, evidence, and historical records related to the case. Once the detective has compiled this information, the **storyteller** designs a compelling narrative that weaves together the facts and presents a coherent story. In the context of AI, RAG operates similarly.

- The Retriever Component acts as the **detective**, scouring databases, documents, and knowledge sources for relevant information and evidence. It compiles a comprehensive set of facts and data points.

- The Generator Component assumes the role of the **storyteller**. Taking the collected information and transforming it into a coherent and engaging narrative, presenting a clear and detailed account of the mystery, much like a detective novel author.

This analogy illustrates how RAG combines the investigative power of retrieval with the creative skills of text generation to produce informative and engaging content, just as our detective and storyteller work together to unravel and present a compelling mystery.

### The Framework

![[Pasted image 20240418115527.png]]
 1. **Prompt** - At the onset, the user provides a prompt outlining their expectations for the response.

2. **Contextual Search** - This pivotal step involves augmenting the original prompt with external contextual information. An external program responsible for searching and retrieving data from various sources comes into play. This process may encompass querying a relational database, conducting keyword-based searches within indexed documents, or even invoking APIs to fetch data from remote or external sources.

3. **Prompt augmentation** - Following the contextual search, the additional information retrieved is seamlessly integrated into the original user prompt. This augmentation enriches the user's query with factual data, enhancing its depth and relevance.

4. **Inference** - With this augmented and context-enriched prompt in hand, the Language Model (LLM) comes into play. The LLM, now armed with both the original user query and the supplementary context, significantly enhances its accuracy. It can tap into factual data sources to provide more precise and contextually relevant responses.

5. **Response** - The LLM formulates the response, incorporating factually correct information. This response is then relayed back, ensuring that the user receives accurate and reliable answers to their queries.
### Components

#### 1. The RAG retriever

**Function**: Compile a set of contextually relevant information that can be used to enrich the user’s query, by retrieving it from external knowledge sources

**Techniques used**: Keyword-based search, Document retrieval, Structured database queries

**Potential Sources**: Pre-built indexes, search algorithms, APIs to access knowledge sources, databases, documents, websites

#### 2. The RAG ranker

**Function**: Ensure that the most relevant/important information is presented to the generator, by refining the retrieved information.

**Algorithms**: Text similarity metrics, context-aware ranking models, machine learning techniques

#### 3. The RAG Generator

**Function**: Ensure that the response aligns with the user’s query and incorporates the factual knowledge retrieved from external sources by generating a response from the prompt & retrieved+ranked information.

**Models**: Transformer based models(e.g. GPT, BERT) to craft human-like text that is contextually relevant, coherent and informative.

## RAG Techniques and Models

### RAG-Sequence
The same retrieved document is used to predict each token in the target sequence. It ==maintains consistency by relying on a single document throughout the generation process==.

### RAG-Token
Different tokens in the target sequence can be predicted based on **different documents**. This ==allows for more flexibility as each token can benefit from the most relevant context==.

## Benefits of RAG

1. Improved Accuracy
2. Contextual Relevance
3. **Enhanced Coherence**: Logical flow and consistency in generated answers
4. **Versatility**: Can adapt to a wide range of tasks
5. **Efficiency**: 
6. Customization
7. Multilingual Capabilities

## Challenges and Future Directions
1. Handling diverse query types
2. Balancing retrieval and generation
3. Scaling to large datasets
4. Evaluation Metrics
5. Ethical Considerations
6. Limited Real-time information
7. Cost and Resource intensiveness

## Further
1. https://python.langchain.com/docs/get_started/introduction.html
2. https://arxiv.org/abs/2005.11401


## References
1. [A Gentle Intro to RAG - Weight & Biases Blog](https://wandb.ai/cosmo3769/RAG/reports/A-Gentle-Introduction-to-Retrieval-Augmented-Generation-RAG---Vmlldzo1MjM4Mjk1)
2. [What is RAG? - AWS Blog](https://aws.amazon.com/what-is/retrieval-augmented-generation/#:~:text=Retrieval%2DAugmented%20Generation%20(RAG),sources%20before%20generating%20a%20response)
3. 

