## Premise
It is Multi-class [[Logistic Regression]]. SoftMax regression allows us to handle $y(i)∈\{1,…,K\}$ where $K$ is the number of classes.

>[!INFO] Note that our convention will be to index the classes starting from 1, rather than from 0.
## Formulation
We had a training set of $\{ (x^{(1)}, y^{(1)}), \ldots, (x^{(m)}, y^{(m)}) \}$  labeled examples, where the input features are $x^{(i)} \in \Re^{n}$.

Given a test input $x$, we want our hypothesis to estimate the probability that $P(y=k|x)$ for each value of $k=1,…,K$. i.e., we want to estimate the probability of the class label taking on each of the $K$ different possible values.

Thus, our hypothesis will output a $K$-dimensional vector (whose elements sum to 1) giving us our $K$ estimated probabilities.
$$
\begin{align}
h_\theta(x) =
\begin{bmatrix}
P(y = 1 | x; \theta) \\
P(y = 2 | x; \theta) \\
\vdots \\
P(y = K | x; \theta)
\end{bmatrix}
=
\frac{1}{ \sum_{j=1}^{K}{\exp(\theta^{(j)\top} x) }}
\begin{bmatrix}
\exp(\theta^{(1)\top} x ) \\
\exp(\theta^{(2)\top} x ) \\
\vdots \\
\exp(\theta^{(K)\top} x ) \\
\end{bmatrix}
\end{align}
$$

- Here $\theta^{(1)}, \theta^{(2)}, \ldots, \theta^{(K)} \in \Re^{n}$ are the parameters of our model. Each class has its own set of parameters.
- Notice that the term $\frac{1}{ \sum_{j=1}^{K}{\exp(\theta^{(j)\top} x) } }$ normalizes the distribution, so that it sums to one.

- For convenience, we will also write $θ$ to denote all the parameters of our model. When you implement softmax regression, it is usually convenient to represent $θ$ as a $n-by-K$ matrix obtained by concatenating $θ^{(1)},θ^{(2)},…,θ^{(K)}$ into columns, so that 

$$
\theta = \left[ \begin{array}{cccc}| & | & | \\
\theta^{(1)} & \theta^{(2)} & \cdots & \theta^{(K)} \\
| & | & | & |
\end{array}\right]
$$

### Cost Function
- We now describe the cost function that we’ll use for softmax regression. 

- In the equation below, $1\{\cdot\}$ is the ”indicator function” so that $1\{\hbox{a true statement}\}=1$ , and $1\{\hbox{a false statement}\}=0$. 

- For example,$1\{2+2=4\}$ evaluates to 1; whereas $1\{1+1=5\}$ evaluates to 0. 

Our cost function will be:
$$
\begin{align}
J(\theta) = - \left[ \sum_{i=1}^{m} \sum_{k=1}^{K}  1\left\{y^{(i)} = k\right\} \log \frac{\exp(\theta^{(k)\top} x^{(i)})}{\sum_{j=1}^K \exp(\theta^{(j)\top} x^{(i)})}\right]
\end{align}
$$Notice that this generalizes the logistic regression cost function, which could also have been written:

$$
\begin{align}
J(\theta) &= - \left[ \sum_{i=1}^m   (1-y^{(i)}) \log (1-h_\theta(x^{(i)})) + y^{(i)} \log h_\theta(x^{(i)}) \right] \\
&= - \left[ \sum_{i=1}^{m} \sum_{k=0}^{1} 1\left\{y^{(i)} = k\right\} \log P(y^{(i)} = k | x^{(i)} ; \theta) \right]
\end{align}
$$

The softmax cost function is similar, except that we now sum over the $K$ different possible values of the class label. Note also that in softmax regression, we have that
$$
P(y^{(i)} = k | x^{(i)} ; \theta) = \frac{\exp(\theta^{(k)\top} x^{(i)})}{\sum_{j=1}^K \exp(\theta^{(j)\top} x^{(i)}) }

$$
### Solving the minimum
We cannot solve for the minimum of $J(θ)$ analytically, and thus as usual we’ll resort to an iterative optimization algorithm. Taking derivatives, one can show that the gradient is:
$$
\begin{align}
\nabla_{\theta^{(k)}} J(\theta) = - \sum_{i=1}^{m}{ \left[ x^{(i)} \left( 1\{ y^{(i)} = k\}  - P(y^{(i)} = k | x^{(i)}; \theta) \right) \right]  }
\end{align}
$$
Recall the meaning of the $\nabla_{\theta^{(k)}}$ notation. In particular, $\nabla_{\theta^{(k)}} J(\theta)$
 is itself a vector, so that its $j$-th element is $\frac{\partial J(\theta)}{\partial \theta_{lk}}$ the partial derivative of $J(θ)$with respect to the $j$-th element of $θ^{(k)}$.

Armed with this formula for the derivative, one can then plug it into a standard optimization package and have it minimize $J(θ)$.

## Relation with Logistic Regression
In the special case where $K=2$, one can show that softmax regression reduces to logistic regression.

## References
1. [Softmax Regression - Deep Learning @ Stanford](http://deeplearning.stanford.edu/tutorial/supervised/SoftmaxRegression/#:~:text=Softmax%20regression%20(or%20multinomial%20logistic,kinds%20of%20hand%2Dwritten%20digits.)
2. 





