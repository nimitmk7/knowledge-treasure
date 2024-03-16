## Motivation
We need an algorithm that will help us:
1. Search the weight space efficiently and find the optimal set $w*$ w.r.t to a Loss Function
2. Be able to handle a massive amount of data.


## Specification
The global minimum of a nicely convex Loss function can be obtained by solving the following equation for $w$: 

$$L'(w) = \frac{dL}{dw} = 0$$ 

where $L'(w)$ is the derivative of $L$ with respect to $w$. 

![[Pasted image 20240315091344.png]]

In most practical cases we wont be able to find such solution easily and therefore we work as follows: 

We know that the  derivative $L'(w)$ gives the slope of $L(w)$ at the point $w$. In other words, it speciﬁes how to scale a small change in the $w$ space to obtain the corresponding change in the output: 

$$ L(w + \epsilon) \approx L(w) + \epsilon L'(w)$$

The derivative is therefore useful for minimizing a function because it tells us how to change $w$ in order to make a small changes in $L(w$). We can **thus reduce $L(w)$ by moving $w$ in small steps with the opposite sign of the derivative**.This technique is called gradient descent

### Vector input
We often minimize loss functions that have multiple inputs: $L: \mathbb R^n→ \mathbb R$. For functions with multiple inputs, we must make use of the concept of partial derivatives. The partial derivative $\frac{\partial L}{\partial \mathbf w_i}$ measures how $L$ changes as only the variable $\mathbf w_i$ increases at point $\mathbf w$. The _gradient_ generalizes the notion of derivative to the case where the derivative is with respect to a vector: the gradient of $L$ is the vector containing all the partial derivatives, denoted

$$\nabla_{\mathbf w} L(\mathbf w)$$

Element $i$ of the gradient is the partial derivative $\frac{\partial L}{\partial \mathbf w_i}$.

In the generic case: 

$$\mathbf w_{k+1} = \mathbf w_k - \eta \nabla_{\mathbf w} L(\mathbf w_k)$$

where $\eta$ is the **scalar learning rate** that is a hyper-parameter that needs to be optimized (searched over).

![[Pasted image 20240315091807.png]]

### Complex Loss Function

![[Pasted image 20240315092245.png]]
Refer to this animation:

![](images/sgd.gif)


## Stochastic Gradient Descent
To calculate the new $\mathbf w$ each iteration we need to calculate the $\frac{\partial L}{\partial \mathbf w_i}$ across the training dataset for the potentially many parameters of the problem. 

As a toy example, lets assume that we have a Gaussian distributed error in our regression model i.e. the Cross Entropy (CE) is the MSE loss function 

$$L(\mathbf w) = \frac{1}{m} \sum_{i=1}^m (\mathbf w^T \mathbf x_i - y_i)^2$$

Its partial derivative is 

$$\frac{\partial L(\mathbf w)}{\partial w_j} = \frac{2}{m} \sum_{i=1}^m (\mathbf w^T \mathbf x_i - y_i) x_i^{(j)}$$

which necessitates going over the whole dataset at each iteration, which would be extremely slow.

Therefore, we perform an approximation to the gradient descent involving two steps:

1. We **define a mini-batch over which we estimate the gradient**. When the mini-batch size is 1, we implement the Stochastic Gradient Descent algorithm. Note in practice people may refer to SGD but may mean mini-batch. 
   
2. We define a schedule of learning rates instead of sticking to only one value. 

The main advantage of Mini-batch GD over Stochastic GD is that you can get a performance boost from hardware optimization of matrix operations, especially when using GPUs.

The following video showcases the advantages of SGD over GD, 
<iframe width="560" height="315" src="https://www.youtube.com/embed/UmathvAKj80?si=mCMmq2FvXSW9jAFv" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Problems with SGD
1. What if the loss function has a local minima or a saddle point ? Saddle points are much more common in high dimension
   - Zero gradient value, so SGD gets stuck. ![[Pasted image 20240208223800.png]]
2. What if loss changes quickly in one direction and slowly in another? What does gradient descent do? 
	- Loss function has high condition number: ratio of largest to smallest singular value of Hessian matrix is large.
	- Very slow progress along shallow dimension, jitter along steep direction.![[Pasted image 20240208224014.png]]
3. Our gradients come from minibatches, so they can be noisy!

For an overview of the various algorithms that are considered enhancements of SGD,  you can read [this](https://www.ruder.io/optimizing-gradient-descent/) blog post and python implementations are also included in the [d2l.ai book](https://d2l.ai/).
## Optimizing Gradient Descent

[[Momentum Optimizer]]
[[AdaGrad]]
[[RMSProp]]
[[Adam Optimizer]]

