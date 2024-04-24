 It involves doing Machine Translation with a single neural network. The neural network architecture is called sequence-to-sequence (aka **seq2seq**) and it involves two **RNNs**(LSTMs).

> [!INFO] More on Seq2Seq
> 
> **Seq2Seq** is useful for more than just Machine Translation. Many NLP tasks can be phrased as sequence-to-sequence. Like Summarization, Dialogue, Parsing, Code generation etc.

Watch: 
<iframe width="560" height="315" src="https://www.youtube.com/embed/XXtpJxZBa2c?si=n9UW8x7cOgvwwzN5" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Architecture

![[Pasted image 20240421193257.png | 300]]

Encoder RNN produces an encoding of the source sentence, which is the initial hidden state for Decoder RNN.

Decoder RNN is a Language Model that generates target sentence, conditioned on encoding. We start off by feeding the **start** token into the decoder, and then we get the first output, and we keep feeding it into the decoder to get successive outputs, and it ends when decoder produces the **end** token.

### Encoder
It takes the a source sequence as input and encodes it into a **fixed-size** "context vector" $\phi$.

We use an RNN to do this mathematically, which evolves its hidden state as follows:
$$h_{t} = f(x_{t}, h_{t-1})$$
The context vector is generated from the sequence of hidden states,
$$\mathbf \phi = q(\mathbf h_1, ..., \mathbf h_{Tx})$$
If we use a bidirectional RNN, the architecture would look like this:
![[Pasted image 20240421193648.png]]
> [!tip] 
>  Seq2Seq encoders will often do something that initially look strange: they will process the input sequence in reverse. 
>  
>  This is actually done on purpose with the idea being that the last thing that the encoder “sees” will roughly corresponds to the first thing that the model outputs; this makes it easier for the decoder to “get started” on the output.

### Decoder
It uses the context vector from encoder output as a “seed” from which to generate an output sequence. We’d like to use it as a language model that’s "aware" of the **words that it’s generated so far** and of the **input**.

The decoder directly calculates,
$$\mathbf{\hat y}   = \arg \max_y p(\mathbf y | \mathbf \phi)$$
We can write this as:
$$\arg \max_y p(\mathbf y| \mathbf \phi) = \arg \max_y p(y_1, y_2, ..., y_{Ty} | \phi)$$
and then use the product rule of probability to decompose this to:
$$\arg \max_y p(y_{Ty} | y_1, ..., y_{Ty-1}, \mathbf \phi)  \dots p(y_2|y_1, \mathbf \phi ) p(y_1| \mathbf \phi) $$
We can now write,
$$\mathbf{\hat y}   = \arg \max_y p(\mathbf y | \mathbf \phi) = \arg \max \prod_{t=1}^{T_y} p(y_t | y_1, ..., y_{t-1}, \mathbf \phi) $$
In this equation $p(y_t | y_1, ..., y_{t-1}, \mathbf \phi)$ is a probability distribution represented by a softmax across all all the words of the vocabulary. 

We can use an RNN to model the conditional probabilities 
$$LSTM = g(s_t,  y_{t-1}, \phi ) = p(y_t | y_1, ..., y_{t-1}, \mathbf \phi) $$

Once the decoder is set up with its context, we’ll pass in a special token to signify the start of output generation; in literature, this is usually an token appended to the end of the input (there’s also one at the end of the output). 

Then, we’ll run all stacked layers of LSTM, one after the other, following at the end with a softmax on the final layer’s output that can tell us the most probable output word.

## Training

![[Pasted image 20240423104547.png]]
1. Get a big parallel corpus.
2. _Both_ the encoder and decoder are trained at the same time, so that they both learn the same context vector representation as shown next.
3. For every step of the decoder, you produce a probability distribution of the word that comes next → $\hat y_i$ , and compute the loss for each one.
4. Compute the average loss. 
5. Perform backpropagation to update the parameters of the full system.

>[!faq] FAQs
>
>**If the decoder RNN outputs the end token too early, then how do you measure the loss function on the words that came after that ?** 
> 
> This is the difference between training and inference in Seq2Seq. During training, you feed the target output word into the RNN, and the predicted output word is not fed back into the network, and only used for computing the loss. 
> 
> **Is the length of the source sentence and length of the target sentence fixed ? How do you deal with different lengths in practical scenarios ?** 
> 
> In practice, as it is easier to assume that your training batch has all even-sized tensors, where everything is the same length, you **pad** all short sentences up to some predefined maximum length, or maybe length of the maximum length example in your batch. Also, make sure to not use any hidden states that come from the padding. 
> 
> **Does the word embedding come from the same training corpus as translation?**
> You can use pre-trained word vector systems like Word2Vec or GLoVE and use them directly, or you can fine-tune them for your corpus. Or you can initialize your embeddings as zero, and then learn them from scratch from the corpus. Depends on choice of the implementation.


## Greedy Decoding
Generating the target sentence by taking $argmax$ on each step of the decoder. This is **greedy decoding** as it takes the most probable word on each step and feeds it to the next step.

**Problem**: Greedy decoding has no way to undo decisions.

**How to fix this?** 

1. **Exhaustive Search Decoding**
	1. Try computing all possible sequences, which means on every step $t$ of the decoder, we’re tracking $V^t$ possible partial translations, where $V$ is vocab size. 
	2. $O(V^t)$ is far too expensive.
2. Beam Search Decoding

## Beam Search Decoding

**Core idea**: On each step of the decoder, keep track of the $k$ most probable partial translations(which we call **hypotheses**)
- $k$ is the beam size(in practice around 5 to 10)

A hypothesis $y_1, …, y_t$ has a score which is its log probability:

$$ score(y_1, …, y_t) = \log P_{LM}(y_1, …, y_t|x) = \sum_{i=1}^t \log P_{LM}(y_{i}|y_1, …, y_{i-1},x)$$

- Scores are all negative, and higher score is better.
- We search for high-scoring hypotheses, tracking top $k$ on each step.

Beam search is not guaranteed to find optimal solution. But it is more efficient than exhaustive search!

### Example
k = 2. 

![[Pasted image 20240424131205.png]]


- For each of the $k$ hypotheses, find top $k$ words and calculate scores.
- Once you stop, backtrack to get the full sentence. 

### Stopping criterion

- In greedy decoding , usually we decode until the model produces a $\text{<END>}$token. 
	- For example: $\text{<START>}$ he hit me with a pie $\text{<END>}$.
- In beam search decoding , different hypotheses may produce $\text{<END>}$ tokens on different timesteps. 
	- When a hypothesis produces $\text{<END>}$, that hypothesis is complete .
	- Place it aside and continue exploring other hypotheses via beam search.
- Usually we continue beam search until:
	- We reach timestep T (where T is some pre-defined cutoff), or
	- We have at least n completed hypotheses (where n is pre defined cutoff)

### Issues

1. Longer hypotheses have lower scores, so you’re gonna end up choosing a shorter hypotheses.
	**Fix**: Normalize scores by length.
	

## Advantages of NMT

1. Better performance
	1. More fluent
	2. Better use of context
	3. Better use of phrase similarities
2. A single neural network to be optimized end-to-end
	1. No subcomponents to be individually optimized
3. Requires much less human engineering effort
	1. No feature engineering
	2. Same method for all language pairs

## Disadvantages of NMT

1. NMT is less interpretable
	1. Hard to debug
2. NMT is difficult to control
	1. Can’t easily specify rules or guidelines for translation
	2. Safety concerns

## Evaluation

### BLEU: Bilingual Evaluation Understudy

- It compares the **machine-written translation** to one or several **human-written translation(s)**, and computes a similarity score based on:
	- n-gram precision(usually for 1,2,3 and 4-grams)
	- Plus a penalty for too-short system translations
- Useful but imperfect
	- There are Many valid ways to translate a sentence. 
	- So a good translation can get a poor BLEU score because it has low n-gram overlap with the human translation.

> [!faq] FAQ
> 
> **In calculating BLEU score, does the permutation of n-grams matter in the 2 sentences ?**
>  
>  Yes. The comparsion is one-to-one(exact sequences). By definition, n-grams are sequences where the order matters. 















