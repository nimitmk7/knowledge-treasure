Consider $n$ independent trials of an experiment where each trial is a "success" with probability $p$. Let $X$ be the number of successes in $n$ trials. 

This situation is truly common in the natural world, and as such, there has been a lot of research into such phenomena. Random variables like $X$ are called binomial random variables. If you can identify that a process fits this description, you can inherit many already proved properties such as the PMF formula, expectation, and variance!
![[Pasted image 20240519165552.png]]
Here are a few examples of binomial random variables:

-  # of heads in 𝑛 coin flips
-  # of 1’s in randomly generated length 𝑛 bit string
-  # of disk drives crashed in 1000 computer cluster, assuming disks crash independently.

## Formulation

**Notation**: $X \sim Bin(n,p)$
**Description**: Number of "successes" in $n$ identical, independent experiments each with probability of success $p$.
**Parameters**:  $n \in \{0,1,…\}$, the number of experiments, $p\in[0,1],$ the probability that a single experiment gives a “success”
**Support**: $x \in \{ 0, 1, …, n \}$
**PMF Equation**: $P(X=x) = \binom{n}{x}p^x(1-p)^{n-x}$
**Expectation**: $E[X] = n\cdot p$
**Variance**: $\text{Var}(X) = n \cdot p \cdot(1-p)$

## Intuition
One way to think of the binomial is as the sum of $n$ [[Bernoulli Distribution]] variables. 

## PMF
![[Pasted image 20240519170536.png]]

## References
1. https://chrispiech.github.io/probabilityForComputerScientists/en/part2/binomial/