## Premise

### The Bottleneck Problem in Seq2Seq

Encoding of the source sentence(a single vector) needs to capture all the information about the source sentence, as that is the only input to the decoder. 

If some information about the source sentence isn’t in the vector, there is no way the decoder is gonna be able to translate it correctly. 

Therefore, there is a **INFORMATION BOTTLENECK**. We are putting too much pressure on the single vector to be a good representation of the source sentence.

### Solution using Attention
**Core Idea**: On each step of the decoder, use direct connection to
the encoder to focus on a particular part of the source sequence.

## Steps: A Visual overview


![[Pasted image 20240424191449.png]]


![[Pasted image 20240424191509.png]]

![[Pasted image 20240424191619.png]]

![[Pasted image 20240424191726.png]]
![[Pasted image 20240424191834.png]]
![[Pasted image 20240424191909.png]]

## Attention in equations

- We have encoder hidden states $h_1, …, h_{N} \in \mathbb R^{h}$
- On time-step t , we have decoder hidden state $s_t \in \mathbb R^{h}$
- We get the attention scores $e^t$ for this step: $$e^{t} = [s_{t}^{T}h_{1}, \dots, s_{t}^Th_{N}] \in \mathbb R^N$$
- We take softmax to get the attention distribution $\alpha^t$ for this step (this is a probability distribution and sums to 1). $$ \alpha^t = softmax(e^{t}) \in \mathbb R^N $$
- We use $\alpha^t$ to take a weighted sum of the encoder hidden states to get the attention output $a_t$.$$a_{t}= \sum_{i=1}^N \alpha_{i}^Th_{i} \in \mathbb R^h$$
- Finally we concatenate the attention output with the decoder hidden_state and proceed as in the non attention seq2seq model. $$[a_{t};s_{t}] \in \mathbb R^{2h}$$

## Advantages

1. Significantly improves NMT performance
	1. Very useful to allow decoder to focus on certain parts of the source
2. Solves the bottleneck problem
	1. Allows decoder to look directly at source; bypass bottleneck
3. Helps with vanishing gradient problem
	1. Provides shortcut to faraway states
4. Provides some interpretability
	1. By inspecting attention distribution, we can see what the decoder was focusing on.    ![[Pasted image 20240424193509.png]] 
	2. We get soft alignment for free, without training an alignment system. The network just learn it by itself.
