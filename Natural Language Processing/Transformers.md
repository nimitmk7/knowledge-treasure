![[Pasted image 20240420183111.png | 300]]
## Premise
Before 2017, Recurrent Neural Networks (RNNs) and Word2Vec models were used to understand text with deep learning. 
### Issues with Recurrent Models 
#### Linear Interaction Distance
- RNNs are unrolled “left-to-right”, which encodes **linear locality**, so nearby words often affect each other’s decisions.
- **Problem**: RNNs take **O(sequence length)** steps for distant words to interact. 
 ![[Pasted image 20240426100651.png]]
	- Long distance dependencies take a long time to interact.
	- Long distance dependencies are difficult to learn because of RNN gradient problems.
- Linear order of words is “baked in”; we already know linear order isn’t the right way to think about sentences…
 ![[Pasted image 20240426101037.png]]
#### Lack of Parallelizability
- Forward and Backward passes have **O(sequence length)** unparallelizable operations.
	- GPUs can perform a bunch of independent computations at once!
	- But future RNN hidden states can’t be computed in full before past RNN hidden states have been computed
	- Inhibits training on very large datasets!
![[Pasted image 20240426101201.png]]

### Attention: The savior
- Number of unparallelizable operations does not increase with sequence length.
- Maximum interaction distance: O(1), since all words interact at every layer!
- **Core Idea**: Attention allows you to look very far away at once, and doesn’t have dependence on sequence index that keeps us from parallelizing operations.
![[Pasted image 20240426101615.png]]

## [[Attention]]

### Barriers and solutions for self-attention as a building block
![[Pasted image 20240426104640.png]] 

#### 1. Sequence order
Since self-attention doesn’t build in order information, we need to encode the order of the sentence in our keys, queries, and values.

Consider representing each sequence index as a vector $$𝒑_{𝑖} ∈ ℝ^𝑑 , \text{for } 𝑖 ∈ \{1,2, … , 𝑛\} \text{ are position vectors}$$
So we incorporate this info into our self-attention block: we add the $𝒑_{𝑖}$ to our inputs!

Recall that $𝒙_{𝑖}$ is the embedding of the word at index 𝑖. The positioned embedding is: $$\bar x_i = x_i + p_i$$
##### Learned absolute position representations

Let all $𝑝_{i}$ be learnable parameters! Learn a matrix $𝒑 ∈ ℝ^{𝑑×𝑛}$ , and let each $𝒑_{𝑖}$ be a column of that matrix! 
- Pros:
	- Flexibility: each position gets to be learned to fit the data 
- Cons: 
	- Definitely can’t extrapolate to indices outside 1, … , 𝑛. 

- Most systems use this today !

#### 2. Adding non-linearities

There are no element-wise non-linearities in self-attention; stacking more self-attention layers just re-averages **value** vectors. 

Imagine if we were to stack self-attention layers.  Intuitively, there’s one thing that’s missing: the elementwise nonlinearities that we’ve come to expect in standard deep learning architectures. 

In fact, if we stack two self-attention layers, we get something that looks a lot like a single self-attention layer: 

$$
\begin{align} o_{i} & = \sum^n_{j=1} α_{ij} V^{(2)} \left(\sum^n_{k=1} α_{jk} V^{(1)} x_{k}  \right)  \\
& = \sum^n_{k=1}\left(α_{jk} \sum^n_{i=1} α_{ij} \right) V^{(2)} V^{(1)} x_{k}\\
& = \sum_{k=1}^n \alpha_{ij}^* V^* x_{k}\\
\end{align}
$$

So, this is just a linear combination of a linear transformation of the input, much like a single layer of self-attention! Is this good enough?

**Solution**: Add a feed-forward network to post-process each output vector.

In practice, after a layer of self-attention, it’s common to apply feed-forward network independently to each word representation:

$$ h_{FF} = W_{2} \space ReLU(W_{1}h_{self-attention} + b_{1}) + b_{2},$$
where often, $W_{1} ∈ \mathbb R^{5d×d}$ , and $W_{2} ∈ \mathbb R^{5d×d}$ . That is, the **hidden dimension of the feed-forward network is substantially larger than the hidden dimension of the network**, $d$—this is done because this **matrix multiply is an efficiently parallelizable operation**, so it’s an efficient place to put a lot of computation and parameters.

![[Pasted image 20240426122419.png | 500]]

#### 3. Masking the future in self-attention
- To use self-attention in decoders, we need to ensure that we can’t peek at the future. 
- At every timestep, we could change the set of keys and queries to include only past words.(Inefficient !!!)
- **Solution**: To enable parallelization, we mask out attention to the future words by setting attention scores to zero.
$$
e_{ij} =
\begin{cases}
\space q_{i}^{T}k_{j} & j \le i  \\
\space -\infty , & j > i
\end{cases}
$$

![[Pasted image 20240426124527.png | 500]]
### Minimal Self-attention architecture

![[Pasted image 20240426125118.png]]

## The Transformer Model

### The Transformer Decoder
- The Transformer Decoder is a stack of Transformer Decoder **Blocks**.
- Each Block consists of:
	- Self-attention 
	- Add & Norm
	- Feed-Forward
	- Add & Norm

![[Pasted image 20240426135504.png ]]
#### Masked Multi-head attention

##### Hypothetical Example
![[Pasted image 20240426135740.png]]
##### Sequence-Stacked form of attention
- Let’s look at how key-query-value attention is computed, in matrices.
	- Let 𝑋 = $[𝑥_{1}; … ; 𝑥_{𝑛}] ∈ ℝ^{𝑛×𝑑}$ be the concatenation of input vectors.
	- First, note that $𝑋𝐾 ∈ ℝ^{𝑛×𝑑} , 𝑋𝑄 ∈ ℝ^{𝑛×𝑑} , 𝑋𝑉 ∈ ℝ^{𝑛×𝑑}$. 
	- The output is defined as output = softmax $(𝑋𝑄(𝑋𝐾)^⊤)𝑋𝑉 ∈∈ ℝ^{𝑛×𝑑}$ .

1. Take the query-key dot products in one matrix multiplication: $XQ(XK)^{\top}$ 
2. Softmax of the products, and compute the weighted average with another matrix multiplication.



![[Pasted image 20240426141615.png]]
##### Multi-headed attention

- What if we want to look in multiple places in the sentence at once?
	- For word 𝑖, self-attention “looks” where $𝑥_{𝑖}^⊤𝑄^⊤𝐾𝑥_{j}$ is high, but maybe we want to focus on different 𝑗 for different reasons?
- We’ll define multiple attention “heads” through multiple Q,K,V matrices
- Let,$𝑄_ℓ,𝐾_{ℓ},𝑉_{l} ∈ ℝ^{𝑑 × \frac{𝑑}{h}}$, where $h$ is the number of attention heads, and $l$ranges from 1 to $h$.
- Each attention head performs attention independently: 
 $$output_{l} = softmax(𝑋𝑄_{l}𝐾_{l}^⊤𝑋 ^⊤) * 𝑋𝑉_{l} \space \text{where} \space output_{ℓ} ∈ ℝ^{𝑑/ℎ}$$
 - Then the outputs of all the heads are combined!
	 - output = $[output_{1}; … ; output_{h}]$ 𝑌, where 𝑌 ∈ $ℝ^{𝑑×𝑑}$
	
 - Each head gets to “look” at different things, and construct value vectors differently.
- Even though we compute $h$ many attention heads, it’s not really more costly. 
	- We compute $𝑋𝑄 ∈ ℝ^{𝑛×𝑑}$ , and then reshape to $ℝ^{𝑛×ℎ×𝑑/ℎ}$ . (Likewise for 𝑋𝐾, 𝑋𝑉.) 
	- Then we transpose to $ℝ^{h×n×d/h}$ ; now the head axis is like a batch axis.
	- Almost everything else is identical, and the matrices are the same sizes.
![[Pasted image 20240426143131.png | 700]]
######  Scaled Dot Product
- “Scaled Dot Product” attention aids in training. 
- When dimensionality 𝑑 becomes large, dot products between vectors tend to become large.
	- Because of this, inputs to the softmax function can be large, making the gradients small. 
- Instead of the self-attention function we’ve seen: 
$$$$
-  We divide the attention scores by $\sqrt{d/h}$, to stop the scores from becoming large just as a function of 𝑑/ℎ (The dimensionality divided by the number of heads.) $$output_{l} = softmax\left( \frac{𝑋𝑄_{ℓ}𝐾_{ℓ}^⊤𝑋^⊤}{\sqrt{d/h}} \right) ∗ 𝑋𝑉_{l}$$
#### Residual Connections
- They are a trick to help models train better. 
	- Instead of $X^{(i)} = Layer(X^{(i-1)})$(where $i$ represents the layer) ![[Pasted image 20240428112944.png]]
	- We let  $X^{(i)} = X^{(i-1)}+Layer(X^{(i-1)})$ (so we only have to learn **the residual** from previous layer) ![[Pasted image 20240428113117.png]]
- Gradient is great through the residual connection; its 1!
- Bias towards the identity function!
- ![[Pasted image 20240428114559.png | 300]]

#### Layer Normalization
- Trick to help models train faster.
- **Idea**: Cut down on uninformative variation in hidden vector values by normalizing to unit mean and standard deviation **within each layer**.
	- LayerNorm’s success may be due to its normalizing gradients.
- Let $x \in \mathbb R^d$ be an individual(word) vector in the model. 
- Let $\mu = \frac{1}{d} \sum^d_{j=1} x_{j};$ this is the mean; $\mu\in\mathbb R$.
- Let $\sigma = \sqrt{\frac{1}{d} \sum^d_{j=1} (x_{j} - \mu)^2}$; this is the standard deviation; $\sigma\in\mathbb R$.
- Let $\gamma \in \mathbb R^d$ and $\beta \in \mathbb R^d$ be learned “gain” and “bias” parameters.(Can omit!)
- Then layer normalization computes: $$\text{output} = \frac{x-\mu}{\sqrt{\sigma} + \epsilon} * \gamma \space + \beta  $$
>[!TIP] 
> Here, we’ve broadcasted the $\mu$ and $\sigma$ across the d dimensions of $x$, and thats how we perform the vector operation.

### Transformer Encoder
- The Transformer Decoder constrains to unidirectional context, as for language models.
- But we want bidirectional context, like in a bidirectional RNN, in the encoder. So we remove the masking in the self-attention.

![[Pasted image 20240428120813.png]]

### Putting them both together
![[Pasted image 20240428120840.png]]
#### Cross-attention
-  Self-attention is when keys, queries, and values come from the same source. 
- Let $h_{1} , … , h_{𝑛}$ be output vectors from the Transformer encoder; $x_{i}\in \mathbb R^d$ 
- Let $z_{1} , … , z_{n}$ be input vectors from the Transformer decoder, $z_{i} \in \mathbb R^{d}$ 
- Then keys and values are drawn from the encoder (like a memory): 
	- $𝑘_{𝑖}$ = $𝐾ℎ_{𝑖}$ ,
	- $𝑣_{𝑖}$ = $𝑉ℎ_{𝑖}$ 
- And the queries are drawn from the decoder,
	- $𝑞_{𝑖} = 𝑄z_𝑖$ .
![[Pasted image 20240428121955.png | 500]]
 
## Videos

<iframe width="560" height="315" src="https://www.youtube.com/embed/wjZofJX0v4M?si=f4TgLPeUarjTLUu9" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/LWMzyfvuehA?si=9T_f3vQGYEodPurC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe> 

## Problems
1. **Quadratic compute in self-attention**
	1. Computing all pairs of interactions means our computation grows quadratically with sequence length - $O(n^2)$
	2. For recurrent models, it grew linearly - $O(n)$
2. **Position representations**
	1. Simple absolute indices are not the best way to represent position.

## References
1. https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1234/slides/cs224n-2023-lecture08-transformers.pdf
2. https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1234/readings/cs224n-self-attention-transformers-2023_draft.pdf