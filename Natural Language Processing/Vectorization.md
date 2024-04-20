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

## Distributional Semantics
A word’s meaning is given by the words that frequently appear close-by.

> "You shall know a word by the company it keeps" (J. R. Firth 1957: 11)

- When a word $w$ appears in a text, its **context** is the set of **words that appear nearby**(within a fixed-size window).
- We use the **many contexts** of $w$ to build up a representation of $w​$.

![[context_example.png]]

We will build a dense vector for each word, in a manner such that it is *similar* to vectors of words that appear in similar contexts (measuring similarity as the **vector dot (scalar) product)**.

>[!faq] Why dot product? 
>Think, if $X$, $Y$ are two vectors with zero mean, then the covariance $Cov[XY] =E[X \cdot Y]$ is the dot product of $X$ and $Y$ (i.e., shows the relationship)

This is one of the **most important ideas** of modern statistical natural language processing. _Context-aware vectors_ are orders of magnitude more powerful that _one-hot vectors._

> **Note:** **Word vectors** are also called **word embeddings** or **neural word representations**.

We can then take the learned word/phrase vectors and represent them in a 2D decomposition to visualize the distance between words.

![[2D_embedding_example.png]]
## Finding Relationships
There are hundreds of ways to learn the relationship between words, learning these similarities would help us to construct simultaneously **more meaningful vectors** and **more compact vectors** like $\text{profit}=[0.23,0.62...0.44]$ presented above.

There are 2 types of word vector representations:
![[Pasted image 20240418232307.png]]


## Methods

### 1. One-hot Encoding

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

### 2. Word Document Matrix

One obvious method is called **Word-Document Matrix**, and it simply says that **words** that appear in the **same document**, has the **same meaning.**

Loop over billions of documents and for each time word $i$ appears in document $j$,we add one to entry $X_{i j}$. This is obviously a very large matrix $\left(\mathbb{R}^{|V| \times M}\right)$ and it scales with the number of documents $(M)$.

#### Example
`Document 1:` the bank’s **financial** statements are **poor**...

`Document 2:` the weather became **poor** after the temperature dropped...

`Document 3:` the **president** fired his cabinet secretary due to **poor** performance.

`Document 4:` the bank’s **president** had to deal with the **financial** crises.

|            | Doc 1 | Doc 2 | Doc 3 | Doc 4 |
| ---------- | ----- | ----- | ----- | ----- |
| bank’s     | 1     | 0     | 0     | 1     |
| financial  | 1     | 0     | 0     | 1     |
| statements | 1     | 0     | 0     | 0     |
| poor       | 1     | 1     | 1     | 0     |
| president  | 0     | 0     | 1     | 1     |
| cabinet    | 0     | 0     | 1     | 0     |
| crises     | 0     | 0     | 0     | 1     |

**Rows**: Word Vectors, **Columns**: Document Vectors

This method is also known as **bag-of-words** or **count vectorization**, and it has many variants.

#### TF-IDF

We ignored some words in the source sentences, if we didn’t do that, the most important word would in have been the **“the”** shown at the start of each sentence.

![[Pasted image 20240418232759.png]]

The (TF-IDF) **Term Frequency** - **Inverse Document Frequency** can deal with this phenomena directly by capturing both the frequency (TF) and relevance (IDF) for a word in a document.

$$ w_{i, j}=t f_{i, j} \times \log \left(\frac{N}{d f_{i}}\right) $$

$w_{i, j}=$ Weight for word (i) in document (j)
$t f_{i, j}=$ Count of word (i) in document (j) 
$N$= Total number of documents 
$d f_{i}$ = How many documents the word (i) appears in

>[!WARNING] For instance, a collection of documents on the banking industry is likely to have the term “bank” in almost every document. To this end, we introduce a mechanism for attenuating the effect by penalizing `thematic` words that appear in many documents.

We can see that, the first term $t f_{i, j}$ of the TF-IDF model is just the count of the word in the document as before.

The magic happens in the second term $\log \left(\frac{N}{d f_{i}}\right)$ where the model imposes an additional condition for a word to be deemed "important".

- If the word **appears in every document**, it in fact **loses all its weight**, and that would be **“the”** in our example ($\log (1)=0$).
- The value is **highest** when the word **appears many times within a small number of documents** (thus lending high discriminating power to those documents).
- Discriminating power is important because we finally want to use these vectors to predict outcomes.
- There is still the drawback that the word is **captured in a standalone manner** and the context of the word is not captured beyond the frequency.

##### Example
We have an example of idf's (log base 10) in a Reuters collection of 806,791 documents.
$\begin{array}{||l|r|r||} \hline \text { term } & \mathrm{df}_{t} & \mathrm{idf}_{t} \\ \hline \text { car } & 18,165 & 1.65 \\ \text { auto } & 6723 & 2.08 \\ \text { insurance } & 19,241 & 1.62 \\ \text { best } & 25,235 & 1.5 \\ \hline \end{array}$

It shows that the word “**auto**” is more rare than the word “**best**”. 
##### Hands-on
```Python
N = len(docs)

def tf(t, d):
    return d.count(t)

def idf(t):
    df = 0
    for doc in docs:
        df += t in doc
    return log(N/(df + 1))

def tfidf(t, d):
    return tf(t,d)* idf(t)
```

In **scikit-learn**, TF-IDF is implemented as the `TfidfVectorizer` and bag-of-words (BOW) with `CountVectorizer`.

#### TF-IDF vs BOW
- It largely depends on the nature of the documents you are working with and task you are working on.
- If words are _distributed equally_ across the documents, then normalizing by _idf_ will not matter much.
- If rare words do not carry valuable meaning to the classification model, then _idf_ does not have a particular advantage.

#### Issues
1. Ordering and location of the words are not encoded in any fashion.


#### Co-occurence Matrix(Count)

We can do something smarter, called **Window based Co-occurence Matrix** whereby the matrix stores the **co-occurences of each word with another** based on a (1) window or (2) document.

`Document 1:` the bank’s **financial** statements are **poor**...
`Document 2:` the weather turned poor after the temperature dropped...
`Document 3:` the **president** fired his cabinet secretary due to **poor**...
`Document 4:` the bank’s **president** had to deal with the **financial** crises...

##### Example:
For one document:

|            | bank’s | financial | statements | ... | crises |
| ---------- | ------ | --------- | ---------- | --- | ------ |
| bank’s     | 0      | 2         | 1          | ... | 1      |
| financial  | 2      | 0         | 1          | ... | 1      |
| statements | 1      | 1         | 0          | ... | 0      |
| poor       | 1      | 1         | 1          | ... | 0      |
| president  | 1      | 1         | 1          | ... | 1      |
| cabinet    | 0      | 0         | 0          | ... | 1      |
| ...        | ...    | ...       | ...        | ... | ...    |
| crises     | 1      | 1         | 0          | ..  | 0      |

We can now perform another trick, currently we still have a very large $|V| \times|V|$ co-occurrence matrix, $X$.

What if we perform PCA (using SVD) to decrease the matrix size to $V \times k$, with $k$ being the number of outputs features (this method is called LSA).

$|V|\begin{aligned} &|V| \\ &[\hat{X}] \end{aligned}=\left[\begin{array}{ccc} \mid & \mid & \\ u_{1} & u_{2} & \cdots \\ \mid & \mid & \end{array}\right] k\left[\begin{array}{ccc} \sigma_{1} & 0 & \cdots \\ 0 & \sigma_{2} & \cdots \\ \vdots & \vdots & \ddots \end{array}\right] k\left[\begin{array}{ccc}& v_{1} & - \\ & v_{2} & - \\ \vdots & \end{array}\right]$


If $k=3$, then word vector 1 (bank’s), might have values $\text{banks}=[0.34,0.20, -.014]$ for example.

```Python
from sklearn.decomposition import PCA

pca = PCA(n_components=3)
co_matrix_efficient = pca.fit_transform(co_matrix)
```

Word-document co-occurrence matrix will give **general topics** (all sports terms will have similar entries) leading to "**Latent Semantic Analysis**" ("document space").

##### Issues
- The dimensions of the matrix change very often (new words are added very frequently and corpus changes in size).
- The matrix is extremely sparse since most words do not co-occur.
- The matrix can be very high dimensional in general $\left(\approx 10^{6} \times 10^{6}\right)$
- Quadratic cost to train (i.e. to perform PCA)

>[!attention] SVD based methods do not scale well for big matrices and it is hard to incorporate new words or documents. Computational cost for a $m \times n$ matrix is $O\left(m n^{2}\right)​$

#### Final Note
→ Let’s repeat it one more time, _bag-of-words_ matrices are not able to represent semantically similar words.

- For instance, "dogs chew snacks" and "canines eat treats" have zero feature overlap in a bag-of-words, despite similar meanings.
- The use of _co-occurence_ matrices could help but it could lead to computational problems due to the need for a $|V| \times|V|$ matrix.
### 3. [[Word2Vec]]

### 4. Glove
- It uses no neural networks and have shown to perform as well as Word2vec, in fact they produce very similar results.
- The advantage of GloVe is that it does not just rely on **local statistics** (local context information of words like Word2Vec), but incorporates **global statistics** (word co-occurrence) to obtain word vectors like LSA







