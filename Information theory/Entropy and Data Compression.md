For understanding entropy as a concept, refer to: [[Entropy and Information]].

A random variable's entropy quantifies **the limit of how much lossless compression of data can be achieved**, which is called the source coding problem.

## Goal
**Lossless Compression** : Choose an encoding scheme that will(on average) yield the shortest possible binary description of our sequence without introducing any ambiguity as to what the outcomes of the draws were.

## Intuition
- Sequences that require longer encodings contain more information, as the entropy is higher.
- We can achieve a shorter average binary string length by **encoding more probable outcomes with shorter strings and less probable outcomes with longer strings**. (Using **prefix code**, refer to Huffman Coding Scheme).
- Therefore, the more evenly distributed the outcomes are, more is the number of outcomes with longer strings.
- If we use the same no of digits for each outcome, it would introduce redundancy and the average encoding length would be longer than necessary.

## Example

![[Pasted image 20240312114636.png]]

## Redundancy

The redundancy of a random event depends on
1. how uncertain its outcome is, **compared to** 
2. how uncertain a variable on the same probability space could be. 

The latter is defined by the maximum entropy distribution $Hmax(\chi)$ on the set of possible outcomes $\chi$. _The maximum entropy distribution_ is always the one in which the probability of all events is equal. 

This makes sense because the outcome is the most uncertain, or equivalently the optimal binary recording of the events has the longest possible length.

By setting the probability of all events equal in a particular probability space, we find that maximum entropy is equal to the log number of possible states:

$$
H_{max}(\mathcal X) = log_2|\chi|
$$

Where $|\chi|$ is the number of elements in the set $\chi$. The larger the number of elements in $\chi$, the more uncertain we can potentially be about what value it will take.

The redundancy W is the difference between this maximum entropy and the actual entropy of the random variable:

$$
W(X) = H_{max}(\mathcal X) − H(X)
$$




