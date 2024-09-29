## Bernoulli Random Variables

A Bernoulli random variable (also called a _boolean_ or _indicator_ random variable) is the simplest kind of parametric random variable. It can take on two values, 1 and 0. 

It takes on a 1 if an experiment with probability $p$ resulted in success and a 0 otherwise. 

Some example uses include a coin flip, a random binary digit, whether a disk drive crashed, and whether someone likes a Netflix movie. 

Here $p$ is the parameter, but different instances of Bernoulli random variables might have different values of $p$.

## Formulation

**Notation**: $X \sim Bern(p)$
**Description**: A boolean variable that is 1 with probability $p$
**Parameters**: $p$, the probability that $X=1$
**Support**: $x$ is either 0 or 1
**PMF**: $P(X=x) = \begin{cases} p &\text{if x = 1} \\ 1-p  &\text{if x = 0} \end{cases}$
**PMF(smooth)**: $P(X=x) = p^x(1-p)^{1-x}$
**Expectation**: $E[X] = p$
**Variance**: $\text{Var}(X) = p(1-p)$

## Proof of Expectation
$$
\begin{align}
    E[X] &= \sum_x x \cdot P(X=x) && \text{Definition of expectation} \\
    &= 1 \cdot p + 0 \cdot (1-p) &&
    X \text{ can take on values 0 and 1} \\
    &= p && \text{Remove the 0 term}
    \end{align}
$$
## Proof of Variance
$$\begin{align}
    E[X^2]
    &= \sum_x x^2 \cdot P(X=x) \\
    &= 0^2 \cdot (1-p) + 1^2 \cdot p\\
    &= p
    \end{align}$$
$$\begin{align}
    Var(X)
    &= E[X^2] - E[X]^2&& \text{Def of variance} \\
    &= p - p^2 && \text{Substitute }E[X^2]=p, E[X] = p \\
    &= p (1-p) && \text{Factor out }p
    \end{align}$$



## References
1. https://chrispiech.github.io/probabilityForComputerScientists/en/part2/bernoulli/
2. 

