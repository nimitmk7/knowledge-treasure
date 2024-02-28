A regression with a (0,1) target variable. We need to squash the output of the linear regression into an interval between 0 and 1. This helps us interpret the result as probability. 

The logistic function will help us do that. 

## Logistic function
It is a common S-shaped curve(sigmoid) with equation:

$$
f(x) = \frac {L}{1+ e^{-k(x-x_0)}}
$$

where 
$x_0$ = the $x$ value of the sigmoid's midpoint
L = the curve's maximum value
$k$ = the logistic growth rate or steepness of the curve

The simplest version most commonly used is $\frac{1}{1+e^{-x}}$. 

## Formal Equation
$$
h(x) = g(W^Tx) = \frac{1}{1+e^{-W^Tx}}
$$
### Importance of $x$
- If you express a probability as a function of x using the logistic function $p = \frac{1}{1+e^{-x}}$
- Then, when you solve for x, you get $x = ln(\frac{p}{1-p})$
- ![[Pasted image 20240203190535.png]]
- 
- Turns out that $\frac{p}{1-p}$ is the odds ratio, the ratio of chance of a win divided by a chance of loss, so x is the natural log of the odds ratio a.k.a **Logit**

## Logistic Regression Fitting(using MLE)

$$
P(y|x;w)= \Pi_{i=1}^{n} P(y^{(i)}|x^{(i)};w) = \Pi_{i=1}^{n} \phi(z^{(i)})^{y^{(i)}}(1-\phi(z^{(i)})^{1-y^{(i)}}
$$
### Explanation
$$
\begin{align}
P(y=1|x;w) = \phi(z) \\
P(y=0|x;w)= 1 - \phi(z)
\end{align}
$$
For examples having target as 1, we want to maximize the probability $\phi(z)$, and for the examples having target as 0, we want to maximize the complement of it, $1-\phi(z)$


### Log likelihood
In practice it is easier, to maximize the log of this equation, which is called **log likelihood**. 
$$
ln\;P(y|x;w)=  \Sigma_{i=1}^{n}[ y^{(i)}ln(\phi(z^{(i)}))+{(1-y^{(i)})ln((1-\phi(z^{(i)}))}]
$$
## Cost Function: Negated Log Likelihood
If we add a negative sign to log likelihood, the objective function is now to minimize the negated log likelihood, which can be used as a cost function, also known as **Cross Entropy Loss Function**.
$$
J(w) =  \Sigma_{i=1}^{n}[- y^{(i)}ln(\phi(z^{(i)}))-{(1-y^{(i)})ln((1-\phi(z^{(i)}))}]
$$

