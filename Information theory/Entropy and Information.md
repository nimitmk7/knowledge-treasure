If we cannot predict the outcome of an event, we are uncertain, and it is from our perspective, random. However, we may be able to acquire information about the event that reduces its apparent randomness, making us more certain about it.

What will the weather be like in 3 hours? It’s uncertain. But from now until then, we can continuously acquire information by observing the current weather, and 3 hours from now our uncertainty will have collapsed to zero. It no longer appears random to us.

## Formulation(Discrete, IID Case)

We have:
1. Finite, discrete set of independent and identically distributed(**IID**) events 
2. A random variable that assigns a probability to each element of the set.

### Information
The amount of information gained from an outcome $x$ can be calculated as:
$$
Info(x) = \log_2 \frac{1}{p(x)}
$$
The conventional choice of a base-2 logarithm means that the units will be **bits**. 
### Entropy
Since we can calculate the information provided by each outcome, and we know each outcome’s probability, we can compute the probability-weighted average amount of information provided by a random event, otherwise known as the **entropy**, defined by $H(X)$. 
$$
H(X)= \sum_{x\in \mathcal X} p(x) \log_2 \frac{1}{p(x)}
$$
#### Interpretation
- Entropy can also be interpreted as how surprising the random event is on average. 
- Events that are more random will tend to be more surprising on average, and observing their outcome will yield more information, and reduce our uncertainty more. 
- On the flip side, higher entropy also means that there is more uncertainty in the random event to begin with, **so we must acquire more information to be certain about its outcome** compared to an event with lower entropy.
- It might seem that, since lower probability outcomes contain more information, that random events containing many extremely rare outcomes will have the highest entropy. In fact, the opposite is true: random events with probability spread as evenly as possible over their possible outcomes will have the highest entropy. 
- Mathematically, this happens because the gains in information that come from an event becoming increasingly rare are outweighed by the decrease in the probability that the highly informative outcome will occur, due to the logarithm in the formula for information.

## Intuition
More common outcomes provide us with less information, and rarer outcomes provide us with more info. **How?**

If a certain outcome or fact is uncommon(**has less probability**), it automatically eliminates more common outcomes(**having higher probabilities**), making the pool of possibilities smaller. 

## Example
![[Pasted image 20240312112953.png]]
## References
1. [A visual introduction to information theory](https://arxiv.org/abs/2206.07867)
2. 