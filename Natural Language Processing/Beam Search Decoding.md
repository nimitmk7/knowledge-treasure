## Definition

**Core idea**: On each step of the decoder, keep track of the $k$ most probable partial translations(which we call **hypotheses**)
- $k$ is the beam size(in practice around 5 to 10)

A hypothesis $y_1, …, y_t$ has a score which is its log probability:

$$ score(y_1, …, y_t) = \log P_{LM}(y_1, …, y_t|x) = \sum_{i=1}^t \log P_{LM}(y_{i}|y_1, …, y_{i-1},x)$$

- Scores are all negative, and higher score is better.
- We search for high-scoring hypotheses, tracking top $k$ on each step.

Beam search is not guaranteed to find optimal solution. But it is more efficient than exhaustive search!

### Example
k = 2

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
	- We reach time-step T (where T is some pre-defined cutoff), or
	- We have at least n completed hypotheses (where n is pre defined cutoff)

### Issues

1. Longer hypotheses have lower scores, so you’re gonna end up choosing a shorter hypotheses.
	**Fix**: Normalize scores by length.


## Effect of changing beam size k
- Small $k$ has similar problems to greedy decoding($k=1$)
	- Ungrammatical, unnatural, nonsensical, incorrect outputs
- Larger $k$ means you consider more hypotheses
	- Increasing $k$ reduces problems of greedy decoding.
	- Larger $k$ is more computationally expensive. 
	- But increasing $k$ can introduce other problems
		- For [[Neural Machine Translation(NMT)]], ==increasing $k$ too much decreases BLEU score==. This is primarily because large-$k$ beam search produces too-short translations(even with score normalization.)
		- In open ended tasks like chit-chat dialogue, large $k$ can make output **more generic.**
### Effect of beam size in chitchat dialogue

![[Pasted image 20241010201814.png]]

