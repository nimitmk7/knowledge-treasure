
## Stochastic Gradient Descent

### Loss function

$$L(\mathbf w) = \frac{1}{m} \sum_{i=1}^m (\mathbf w^T \mathbf x_i - y_i)^2$$
### Partial Derivative
$$\frac{\partial L(\mathbf w)}{\partial w_j} = \frac{2}{m} \sum_{i=1}^m (\mathbf w^T \mathbf x_i - y_i) x_i^{(j)}$$
## Problems with SGD
1. What if the loss function has a local minima or a saddle point ? Saddle points are much more common in high dimension
   - Zero gradient value, so SGD gets stuck. ![[Pasted image 20240208223800.png]]
2. What if loss changes quickly in one direction and slowly in another? What does gradient descent do? 
	- Loss function has high condition number: ratio of largest to smallest singular value of Hessian matrix is large.
	- Very slow progress along shallow dimension, jitter along steep direction.![[Pasted image 20240208224014.png]]
3. Our gradients come from minibatches, so they can be noisy!

## Optimizing Gradient Descent

[[Momentum Optimizer]]
[[AdaGrad]]
[[RMSProp]]
[[Adam Optimizer]]
