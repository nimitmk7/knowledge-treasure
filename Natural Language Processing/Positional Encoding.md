## Premise
Position and order of words are the essential parts of any language. They define the grammar and thus the actual semantics of a sentence. 
\
Recurrent Neural Networks (RNNs) inherently take the order of word into account; They parse a sentence word by word in a sequential manner.

But the Transformer architecture ditched the recurrence mechanism in favor of multi-head self-attention mechanism.

As each word in a sentence simultaneously flows through the Transformer’s encoder/decoder stack, ==The model itself doesn’t have any sense of position/order for each word==. 

Consequently, there’s still the need for a way to incorporate the order of the words into our model.

## Definition
One possible solution to give the model some sense of order is to ==add a piece of information to each word about its position in the sentence==. We call this “piece of information”, the positional encoding.

## Basic Approaches

### Approach 1: (0,1) Time-step
Assign a number to each time-step within the $[0, 1]$ range in which $0$ means the first word and $1$ is the last time-step.
#### Issue
You can’t figure out how many words are present within a specific range. Time-step delta doesn’t have consistent meaning across different sentences(or different sentence lengths I would say).

### Approach 2: Linear Time-step
Assign a number to each time-step linearly. That is, the first word is given “1”, the second word is given “2”, and so on. 
### Issues
1. Not only the values could get quite large, but also our model can face sentences longer than the ones in training.
2. In addition, our model may not see any sample with one specific length which would hurt generalization of our model.

## Ideal criteria for a good encoding
1. Unique encoding for each time-step(word’s position in a sentence)
2. Distance between 2 time-steps should be consistent across sentences with different lengths.
3. It should generalize to longer sentences without any efforts. Its values should be bounded.
4. It must be deterministic.
## Approach 3: Sinusoidal embeddings
1. It isn’t a single number for each position, but rather a $d$-dimensional vector that contains information about a specific position in a sentence. 
2. This encoding is not integrated into the model itself. Instead, this vector is used to equip each word with information about its position in a sentence. 

> **”We enhance the model’s input to inject the order of words”.**

### Formulation
- Let $t$ be the desired position in an input sentence. 
- Let $\vec{p_t} \in \mathbb{R}^d$ be its corresponding encoding, and $d$ be the encoding dimension, and $d \equiv_2 0$
- Then $f : \mathbb{N} \rightarrow \mathbb{R}^d$ will be the function that produces the output vector $\vec{p_t}$ and it is defined as follows:$$\begin{align}\vec{p_t}^{(i)} = f(t)^{(i)}& := \begin{cases}
      \sin({\omega_k} . t),  & \text{if}\  i = 2k \\
      \cos({\omega_k} . t),  & \text{if}\  i = 2k + 1
  \end{cases}
\end{align}$$
- where $$\omega_k = \frac{1}{10000^{2k / d}}$$
- As it can be derived from the function definition, the frequencies are decreasing along the vector dimension. Thus it forms a geometric progression from $2\pi$ to $10000⋅2\pi$ on the wavelengths.

- You can also imagine the positional embedding $\vec{p_t}$ as a vector containing pairs of sines and cosines for each frequency.(Note that $d$ is divisible by 2). $$\vec{p_t} = \begin{bmatrix} \sin({\omega_1}.t)\\ \cos({\omega_1}.t)\\ \\ \sin({\omega_2}.t)\\ \cos({\omega_2}.t)\\ \\ \vdots\\ \\ \sin({\omega_{d/2}}.t)\\ \cos({\omega_{d/2}}.t) \end{bmatrix}_{d \times 1}$$
### The intuition
You may wonder how this combination of sines and cosines could ever represent a position/order? It is actually quite simple, Suppose you want to represent a number in binary format, how will that be?
$$
\begin{align}
  0: \ \ \ \ \color{orange}{\texttt{0}} \ \ \color{green}{\texttt{0}} \ \ \color{blue}{\texttt{0}} \ \ \color{red}{\texttt{0}} & & 
  8: \ \ \ \ \color{orange}{\texttt{1}} \ \ \color{green}{\texttt{0}} \ \ \color{blue}{\texttt{0}} \ \ \color{red}{\texttt{0}} \\
  1: \ \ \ \ \color{orange}{\texttt{0}} \ \ \color{green}{\texttt{0}} \ \ \color{blue}{\texttt{0}} \ \ \color{red}{\texttt{1}} & & 
  9: \ \ \ \ \color{orange}{\texttt{1}} \ \ \color{green}{\texttt{0}} \ \ \color{blue}{\texttt{0}} \ \ \color{red}{\texttt{1}} \\ 
  2: \ \ \ \ \color{orange}{\texttt{0}} \ \ \color{green}{\texttt{0}} \ \ \color{blue}{\texttt{1}} \ \ \color{red}{\texttt{0}} & & 
  10: \ \ \ \ \color{orange}{\texttt{1}} \ \ \color{green}{\texttt{0}} \ \ \color{blue}{\texttt{1}} \ \ \color{red}{\texttt{0}} \\ 
  3: \ \ \ \ \color{orange}{\texttt{0}} \ \ \color{green}{\texttt{0}} \ \ \color{blue}{\texttt{1}} \ \ \color{red}{\texttt{1}} & & 
  11: \ \ \ \ \color{orange}{\texttt{1}} \ \ \color{green}{\texttt{0}} \ \ \color{blue}{\texttt{1}} \ \ \color{red}{\texttt{1}} \\ 
  4: \ \ \ \ \color{orange}{\texttt{0}} \ \ \color{green}{\texttt{1}} \ \ \color{blue}{\texttt{0}} \ \ \color{red}{\texttt{0}} & & 
  12: \ \ \ \ \color{orange}{\texttt{1}} \ \ \color{green}{\texttt{1}} \ \ \color{blue}{\texttt{0}} \ \ \color{red}{\texttt{0}} \\
  5: \ \ \ \ \color{orange}{\texttt{0}} \ \ \color{green}{\texttt{1}} \ \ \color{blue}{\texttt{0}} \ \ \color{red}{\texttt{1}} & & 
  13: \ \ \ \ \color{orange}{\texttt{1}} \ \ \color{green}{\texttt{1}} \ \ \color{blue}{\texttt{0}} \ \ \color{red}{\texttt{1}} \\
  6: \ \ \ \ \color{orange}{\texttt{0}} \ \ \color{green}{\texttt{1}} \ \ \color{blue}{\texttt{1}} \ \ \color{red}{\texttt{0}} & & 
  14: \ \ \ \ \color{orange}{\texttt{1}} \ \ \color{green}{\texttt{1}} \ \ \color{blue}{\texttt{1}} \ \ \color{red}{\texttt{0}} \\
  7: \ \ \ \ \color{orange}{\texttt{0}} \ \ \color{green}{\texttt{1}} \ \ \color{blue}{\texttt{1}} \ \ \color{red}{\texttt{1}} & & 
  15: \ \ \ \ \color{orange}{\texttt{1}} \ \ \color{green}{\texttt{1}} \ \ \color{blue}{\texttt{1}} \ \ \color{red}{\texttt{1}} \\
\end{align}
$$

The LSB bit is alternating on every number, the second-lowest bit is rotating on every two numbers, and so on.

But using binary values would be a waste of space in the world of floats. So instead, we can use their float continuous counterparts - Sinusoidal functions

Indeed, they are the equivalent to alternating bits. Moreover, By decreasing their frequencies, we can go from red bits to orange ones.

![[Pasted image 20241005170320.png]]

## Implementation
How do we use this embedding to equip the input words with their positional information ?

For every word $w_{t}$ in a sentence $[$$w_{1}$, $w_{2}$, $\dots$, $w_{n}$$]$, 
$$\begin{align}
\psi^\prime(w_t) = \psi(w_t) + \vec{p_t}
\end{align}$$
To keep this summation possible, we keep the positional embedding’s dimension equal to the word embeddings’ dimension. i.e.

$$d_{\text{word embedding}} = d_{\text{positional embedding}}$$

















 
 
 
 
 
 
 
 
 
 
 
 
 
 
## References
1. https://kazemnejad.com/blog/transformer_architecture_positional_encoding/
2. 
 
 **Intuition**: To produce a Continuous approximation of binary encoding of positions (integers)