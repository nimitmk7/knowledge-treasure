## Premise
How much information can be shared among random events ?

How much uncertainty will be reduced about a random event by knowing the outcome of a different random event ?

This answer to these questions is given by mutual information. 

## Intuition
It gives us a sense of how closely related 2 variables are, like $R^2$. 

It quantifies the "amount of information" obtained about one random variable by observing the other random variable.

It tells us the average surprise or change we see in one variable is related to the surprise in the another.

Watch:

: <iframe width="560" height="315" src="https://www.youtube.com/embed/eJIp_mgVLwE?si=i-VxcOOPxfoPTjL4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


## Formulation

X, Y are 2 random variables. 

### Point-wise mutual information
For a particular event, where X = $x$ and Y = $y$, it is defined as:

$$
\log\left(\frac{p(x,y)}{p(x)p(y)}\right)
$$

which is equal to
$$\log\left(\frac{1}{p(x)}\right) - \log\left(\frac{1}{p(x|y)}\right)$$

### Mutual Information

$$
I(X;Y) =\sum_{x\in\mathcal X}\sum_{y\in\mathcal Y} p(x,y)\log\left(\frac{p(x,y)}{p(x)p(y)}\right)
$$
## Properties
1. It is an invertible function. $I(X,Y) = I(Y,X)$
## Conditional Entropy

$$
H(X\space|\space y) = \sum_{x\in\mathcal X}p(x|y)\space \log\frac{1}{p(x|y)}  
$$

$$
H(X\space|\space Y) = \sum_{x\in\mathcal Y}\sum_{x\in\mathcal X}p(x,y)\space \log\frac{1}{p(x\space|\space y)}  
$$

## Joint Entropy
$$
H(X\space,\space Y) = \sum_{x\in\mathcal Y}\sum_{x\in\mathcal X}p(x,y)\space \log\frac{1}{p(x,y)}  
$$


## Relationship between entropy and mutual information

$$
I(X;Y) = H(Y) - H(Y|X) = H(X) - H(X|Y) = H(X) + H(Y) - H(X,Y)
$$

