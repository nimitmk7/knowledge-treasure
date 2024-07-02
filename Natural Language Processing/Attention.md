We can think of attention as performing fuzzy lookup in a key-value store. In a lookup table, we have a table of keys that map to values. The query matches one of the keys, returning its value. 

In attention, the query matches all keys softly, to a weight between 0 and 1. The keys’ values are multiplied by the weights and summed.
![[Pasted image 20240426102109.png]]

### Hypothetical Example
![[Pasted image 20240426102157.png]]

### Self-attention
In self-attention, Keys, Queries and Values are from the same sequence.

### Formulation and Steps
Let $𝒘_{1:𝑛}$ be a sequence of words in vocabulary 𝑉, like “Zuko made his uncle tea”. 

For each $𝒘_{𝑖}$ , let $𝒙_𝑖 = 𝐸𝒘_𝒊$ , where $𝐸 ∈ ℝ^{𝑑×|𝑉|}$ is an embedding matrix.

>[!INFO] $w_i$ is a one-hot vector with size as the vocabulary size, as evident by the size of $E$ matrix.

1. Transform each word embedding with weight matrices Q, K, V, each in $ℝ^{𝑑×𝑑}$.
	1. $q_i = Qx_i$ (queries)
	2. $k_i = Kx_i$ (keys)
	3. $v_i = Vx_i$ (values)
2. Compute pairwise similarities between keys and queries; normalize with softmax.
	1. $e_{ij} = q_i^T k_j$
	2. $\alpha_{ij} = \frac {exp(e_ij)}{\Sigma _j’ exp(e_ij’)}$
3. Compute output for each word as weighted sum of values.
	$o_{i} = \sum_{j} \alpha_{ij} v_{j}$

## Intuition

When we write, the choice that we make for the next word in the sentence is influenced by other words that we have already written. For example, suppose you start a sentence as follows:

“The pink elephant tried to get into the car but it was too __ ”

Clearly, the next word should be something synonymous with big. How do we know this?

Certain other words in the sentence are important for helping us to make our decision. 
1. It is an elephant and not a sloth, so we prefer “big” rather than “slow”.
2. If it were a swimming pool, rather than a car, we might choose “scared” as possible alternative to “big”.
3. The action of getting into the car implies that size is the problem, if the elephant was trying to squash the car instead, we might choose fast as the final word, with **it** now referring to the car.

Some other words are completely irrelevant. 
1. The fact that the elephant is pink has no influence on our choice of final word. 
2. The minor words in the sentence(the, but , it, etc.) give the grammatical form, but here aren’t important to determine the required objective.

In other words, we are paying **attention** to certain words in the sentence and largely ignoring others. Wouldn’t it be great if our model could do the same thing?

An attention mechanism (also know as an attention head) in a Transformer is designed to do exactly this. It is able to decide where in the input it wants to pull information from, in order to efficiently extract useful information without being clouded by irrelevant details. This makes it highly adaptable to a range of circumstances, as it can decide where it wants to look for information at inference time.

In contrast, a recurrent layer tries to build up a generic hidden state that captures an overall representation of the input at each time step. A weakness of this approach is that many of the words that have already been incorporated into the hidden vector will not be directly relevant to the immediate task at hand (e.g., predicting the next word), as we have just seen. Attention heads do not suffer from this problem, because they can pick and choose how to combine information from nearby words, depending on the context.

### Queries, Keys and Values
To predict the next word, other preceding words chime in with their opinions, but their contributions are ==weighted by how confident they are in their own expertise in predicting words that follow== .

In our example, 
1. the word “elephant” might confidently contribute that it is more likely to be a word related to size or loudness.
2. the word “was” doesn’t have much to offer to narrow down the possibilities.

We can think of an attention head as a kind of information retrieval system, where a **query**(What follows the word “too”) is made into a key/value store(other words in the sentence) and the resulting output is a sum of values, weighted by the resonance between query and each key.

#### Query
The **query**(Q) can be thought of as a representation of the current task at hand(e.g. “What word follows too?”). 

It is derived from the embedding of the word “too”, by passing it through a weights matrix $W_Q$ to change the dimensionality of the vector from $d_e$ to $d_k$.

#### Key
The **key** vectors(K) are representations of each word in the sentence–you can think of these as descriptions of the kinds of prediction task that each word can help with. 

They are derived in a similar fashion to the query, by passing each embedding through a weights matrix WK to change the dimensionality of each vector from $d_e$ to $d_k$. Notice that the keys and the query are the same length ($d_k$).

#### Query-Key product
Inside the attention head, each key is compared to the query using a dot product between each pair of vectors($QK^{T}$). This is why keys and query have to be the same length.

The higher this number is for a particular key/query pair, the more the key resonates with the query, so it is allowed to make more of a contribution to the output of the attention head. 

The resulting vector is 
1. Scaled by $d_k$ to keep the variance of the vector sum stable (approximately equal to 1)
2. A softmax is applied to ensure the contributions sum to 1. This is a vector of attention weights.

#### Value
The value vectors (V) are also representations of the words in the sentence—you can think of these as the unweighted contributions of each word. 

They are derived by passing each embedding through a weights matrix WV to change the dimensionality of each vector from $d_e$ to $d_v$. Notice that the value vectors do not necessarily have to have the same length as the keys and query (but often do, for simplicity).

The value vectors are multiplied by the attention weights to give the attention for a given Q, K, and V. 

$$Attention(Q,K,V) =softmax\left(\frac{QK^{T}}{\sqrt{d_{k}}}\right)V$$

![[Pasted image 20240626150814.png]]





