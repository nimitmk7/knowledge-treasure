## Premise
We have to identify a method to represent text (words) to models, and for that we need some notion of **distance/similarity** between words.

## Word Vectorization
The simple conversion from word to vector opens up endless possibilities → **words become features** with which we can perform prediction, sentiment analysis, translation and many other tasks.


There are more than 1 million English words, and they are all **related** in some way or another, e.g., (a) Stock → Share; (b) Investor → Shareholder; (c) Equity → Capital; (d) Revenue → Sales.

The “Shareholder” vector of lengths 3 could perhaps encode information about the function (activity vs <ins>person</ins>), the count (<ins>singular</ins> vs. plural), the industry 
(<ins>finance</ins> vs consumer).
$$\text{Shareholder}=[0, 1, 1]$$

We want to encode word tokens into vectors that represents a point in some sort of "word" space where all words can be found, the example below would have an 8-dimensional space.

### Example
$$
\text { banking }=\left(\begin{array}{r} 0.286 \\ 0.792 \\ -0.177 \\ -0.107 \\ 0.109 \\ -0.542 \\ 0.349 \\ 0.271 \end{array}\right) \quad \text { monetary }=\left(\begin{array}{r} 0.413 \\ 0.582 \\ -0.007 \\ 0.247 \\ 0.216 \\ -0.718 \\ 0.147 \\ 0.051 \end{array}\right)
$$
**banking** = [ 
0.286 `(Concreteness)`: Relatively low concreteness, indicating a more abstract concept 
0.792 `(Intensity)`: High intensity or strength of association
-0.177 `(Sentiment)`: Slightly negative sentiment or emotional valence
-0.107 `(Dynamism)`: Slightly more static or stable than dynamic or changing 0.109 `(Complexity)`: Relatively low complexity or intricacy
-0.542 `(Physicality)`: Moderately associated with non-physical or intangible aspects 
0.349 `(Temporality)`: Moderately associated with temporal or time-related concepts
0.271 `(Specificity)`: Slightly more specific or domain-specific than general 
]

**monetary** = [
0.413 `(Concreteness)`: Moderate concreteness, indicating a somewhat tangible concept
0.582 `(Intensity)`: Moderate intensity or strength of association
-0.007 `(Sentiment)`: Very slightly negative sentiment or emotional valence 0.247 `(Dynamism)`: Slightly more dynamic or changing than static or stable 0.216 `(Complexity)`: Slightly more complex or intricate than simple 
-0.718 `(Physicality)`: Strongly associated with non-physical or intangible aspects 
0.147 `(Temporality)`: Slightly associated with temporal or time-related concepts
0.051 `(Specificity)`: Very slightly more specific or domain-specific than general 
]

Perhaps there actually exists some N-dimensional space for all 1 million words that sufficiently encode all the semantics of our language.

> [!NOTE] 
> These dimensions and their meaning are discovered and learned automatically. We don’t know what the dimensions refer to, and the meaning is spread across all dimensions.

### Use-Case

What can we do with these word embedding vectors?

- **Similarities →** first these embeddings are useful for keyword/search expansion, semantic search, concept categorization, ****information retrieval.
    - Here we want to highlight that the **similarity between words** can now easily be found.
    - E.g., Banking → Monetary are 75% similar.
- **Features →** second, remember that we are still performing a feature generation task by converting textual data to numeric data.
    - So perhaps more importantly, these vectors are used as **high-quality feature** inputs to downstream models.
    - E.g., Banking → [0.28, 0.79, -0.17] and Monetary → [0.41, 0.58, -0.1]


## Methods

### One-hot Encoding

So let's dive into the **most simple** way to turn words into numbers, the **one-hot vector**:
- Every word is an $\mathbb{R}^{|V| \times 1}$ vector filled with $0$s and a $1$ at the _index of that word_ in a sorted list.
- Using all the English language words $V$, we put the value $1$ in its alphabetically sorted position.
- In this notation $|V|$ is the size of our vocabulary. Word vectors in this type of encoding would appear as follow:
$$
w^{\text {aardoark }}=\left[\begin{array}{c} 1 \\ 0 \\ 0 \\ \vdots \\ 0 \end{array}\right], w^{a}=\left[\begin{array}{c} 0 \\ 1 \\ 0 \\ \vdots \\ 0 \end{array}\right], w^{a t}=\left[\begin{array}{c} 0 \\ 0 \\ 1 \\ \vdots \\ 0 \end{array}\right], \cdots w^{\text {zebra }}=\left[\begin{array}{c} 0 \\ 0 \\ 0 \\ \vdots \\ 1 \end{array}\right]
$$
#### Issues
1. **Lack of Semantic Relationships**: Every word is presented as a completely independent entity:$$ \left(w^{\text {hotel }}\right)^{T} w^{\text {motel }}=\left(w^{\text {hotel }}\right)^{T} w^{\text {cat }}=0$$So, with no overlap whatsoever, this representation cannot give us any ***notion of similarity***.
2. **Memory Inefficiency**: 
	- The one-hot encoded vector for each word is a sparse vector with a length equal to the size of the vocabulary.
	- This results in a very large feature space, especially when the vocabulary is large (e.g. 50,000 words), requiring a lot of memory.
	- Most of the vector is taken up by zeros, making the data sparse and less computationally efficient.





