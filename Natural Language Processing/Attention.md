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

