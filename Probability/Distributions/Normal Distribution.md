The single most important random variable type is the Normal (aka Gaussian) random variable, parametrized by a mean ($\mu$) and variance ($\sigma^2$), or sometimes equivalently written as mean and variance ($\sigma^2$). If 𝑋 is a normal variable we write $X\sim N(\mu,\sigma^2)$. 

The normal is important for many reasons: it is generated from the summation of independent random variables and as a result it occurs often in nature.

Many things in the world are not distributed normally but data scientists and computer scientists model them as Normal distributions anyways. Why? 

Because it is the most entropic (conservative) modelling decision that we can make for a random variable while still matching a particular expectation (average value) and variance (spread).

## Formulation

**Notation**: $X \sim N(\mu, \sigma^2)$
**Description**: A common, naturally occurring distribution
**Parameters**: $\mu \in \mathbb R$, the mean. $\sigma^2 \in R^+$, the variance.
**Support**: $x \in \mathbb R$
**PDF equation**: $$f(x) = \frac{1}{\sigma \sqrt{2 \pi}} e^{-\frac{1}{2}\Big(\frac{x-\mu}{\sigma}\Big)^2}$$
**CDF Equation**: $$\begin{align}
		F(x) &= \phi(\frac{x-\mu}{\sigma})
		   && \text{Where $\phi$ is the CDF of the standard normal}
	\end{align}$$
**Expectation**: $E[X] = \mu$
**Variance**: $\text{Var}(X) = \sigma^2$

## Linear Transform

If 𝑋 is a Normal such that $X \sim N(\mu, \sigma^2)$ and 𝑌 is a linear transform of 𝑋 such that $Y=aX+b$ then $Y$ is also a Normal where:
$$\begin{align*}
    Y \sim N(a\mu + b, a^2\sigma^2)
\end{align*}$$

**Example:**
![[Pasted image 20240519183549.png]]



