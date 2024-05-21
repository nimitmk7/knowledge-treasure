An exponential distribution measures the **amount** of time until a next event occurs. It assumes that the events occur via a poisson process. 

> [!NOTE] Exponential vs Poisson
> This is different from the Poisson Random Variable which measures number of events in a fixed amount of time.

## Formulation

**Notation**: $X \sim Exp(\lambda)$
**Description**: Time until next events if (a) the events occur with a constant mean rate and (b) they occur independently of time since last event.
**Parameters**: $\lambda \in \{0,1,\dots \}$, the constant average rate
**Support**: $x \in \mathbb R^+$
**PDF equation**: $$f(x) = \lambda e^{-\lambda x}$$
**CDF equation**: $$ 1 - e^{-\lambda x}$$
**Expectation**: $E[X] = \frac{1}{\lambda}$
**Variance**: $\text{Var}(X) = \frac{1}{\lambda^2}$

![[Pasted image 20240519181823.png]]

