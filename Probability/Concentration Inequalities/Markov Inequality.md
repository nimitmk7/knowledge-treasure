# Theorem
For any random variable $X$, which only takes non-negative values, and any positive $t$, $$Pr[X\ge t] \le  \frac{\mathbb E[X]}{t}$$
Equivalently, $$Pr[X \ge \alpha \cdot\mathbb E[X] ] \le \frac{1}{\alpha}$$
## Proof

$$\begin{align}\mathbb{E}[X] &= \sum_{x} x \Pr(X=x) \\
&= \sum_{\substack{x \\ x \geq t}} x \Pr(X=x) +
\sum_{\substack{x \\ x < t}} x \Pr(X=x)  \\ & \\
 &\geq \sum_{\substack{x \\ x \geq t}} t \Pr(X=x) + 0 
= t \Pr(X \geq t).\\
\end{align}$$


