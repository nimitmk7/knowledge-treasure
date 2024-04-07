## Motivation
SGD has trouble navigating ravines, i.e. areas where the surface curves much more steeply in one dimension than another, which are common around local optima.

In these scenarios, SGD oscillates across the slopes of the ravine while only making hesitant progress along the bottom towards the local optimum.

Momentum is a method that helps accelerate SGD in the relevant direction and dampens oscillations. It also helps navigate local minima and saddles which are quite common in higher dimensions, due to the velocity it provides to SGD.

1. SGD w/o momentum

![[Pasted image 20240208220923.png]]

2. SGD with momentum

![[Pasted image 20240208220929.png]]
## Method
Adding a fraction $\gamma$ of the update vector of the past time step to the current update vector:

$$
v_t = \gamma v_{t-1} + \eta\Delta_{\theta}J(\theta)
$$
$$
\theta = \theta - v_t
$$
>[!INFO] 
> Some implementations exchange the signs in the equations. The momentum term $\gamma$ is usually set to 0.9 or a similar value.

Essentially, when using momentum, ==we push a ball down a hill==. The ball accumulates momentum as it rolls downhill, becoming faster and faster on the way (until it reaches its terminal velocity if there is air resistance, i.e. $\gamma<1$.

==The same thing happens to our parameter updates: The momentum term increases for dimensions whose gradients point in the same directions and reduces updates for dimensions whose gradients change directions==. As a result, we gain faster convergence and reduced oscillation.

![[Pasted image 20240208224224.png]]


**Core idea**: Build up “velocity” as a running mean of gradients.

## Python implementation

```python
def stochastic_gradient_descent_with_momentum(learning_rate: float, momentum_param: float, epochs: int, w, x_train, y_train):

	M = w.shape[0]
	
	n = x_train.shape[0]
	
	# Initializing the momentum vector
	
	v = np.zeros(w.shape)
	
	epoch_loss = np.zeros(shape=(epochs, 1))
	
	for i in range(epochs):
	
	row_index = np.random.randint(0, n)
	
	x_sample = x_train[row_index, :]
	
	for j in range(M):
	
	y = y_train[j]
	
	y_hat = np.dot(w, x_train[j, :])
	
	gradient = (2 * (y_hat - y) * x_sample[j])
	
	v[j] = momentum_param * v[j] + learning_rate * gradient
	
	w[j] = w[j] - v[j]
	
	epoch_loss[i] = calculate_loss(x_train, y_train, w)
	
	return w, epoch_loss
```


## Advantages



## Disadvantages


## References
1. https://www.ruder.io/optimizing-gradient-descent/#momentum
2. http://cs231n.stanford.edu/slides/2017/cs231n_2017_lecture7.pdf
3. 