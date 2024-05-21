The most basic of all the continuous random variables is the uniform random variable, which is equally likely to take on any value in its range $(\alpha,\beta)$. 𝑋 is a _uniform random variable_ $X \sim Uni(\alpha, \beta)$ if it has PDF:
$$
\begin{align*}
    f(x) = \begin{cases} \frac{1}{\beta - \alpha} &\text{when } \alpha \leq x \leq \beta \\ 
0 & \text{otherwise} \end{cases} 
\end{align*}
$$

## Formulation

**Notation**: $X \sim Uni(\alpha, \beta)$
**Description**: A continuous random variable that takes on values, with equal likelihood between $\alpha$ and $\beta$
**Parameters**: $\alpha \in \mathbb R$, the minimum value of the variable, and $\beta \in \mathbb R$, $\beta > \alpha$, the maximum value of the variable.
**Support**: $x \in [\alpha, \beta]$
**PDF equation**: $$f(x) = \begin{cases}
\frac{1}{\beta - \alpha} && \text{for }x \in [\alpha, \beta]\\
0 && \text{else}
\end{cases}$$
**CDF equation**: $$ F(x) = \begin{cases}
\frac{x - \alpha}{\beta - \alpha} && \text{for }x \in [\alpha, \beta]\\
0 && \text{for } x < \alpha \\
1 && \text{for } x > \beta
\end{cases}$$
**Expectation**: $E[X] = \frac{1}{2} (\alpha + \beta)$
**Variance**: $\text{Var}(X) = \frac{1}{12} (\beta - \alpha)^2$
**PDF Example:**
![[Pasted image 20240519181247.png]]



