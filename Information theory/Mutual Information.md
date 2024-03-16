## Premise
How much information can be shared among random events ?

How much uncertainty will be reduced about a random event by knowing the outcome of a different random event ?

This answer to these questions is given by mutual information.

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
I(X;Y) = \sum_{x\in\mathcal X}\sum_{y\in\mathcal Y}\log\left(\frac{p(x,y)}{p(x)p(y)}\right)
$$

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

