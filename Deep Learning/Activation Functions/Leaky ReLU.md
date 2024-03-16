**Leaky Rectified Linear Unit**, or **Leaky ReLU**, is a type of activation function based on ReLU, but it has a small slope for negative values instead of a flat slope. The slope coefficient is determined before training, i.e. it is not learnt during training.

## Formulation

$$
g(z) = max(ax, x)
$$
where $a$ is a small constant, typically set to value like 0.01.


![[Pasted image 20240220065331.png]]

## Intuition

When x > 0, Leaky ReLU behaves like the ReLU function, returning x.
When x < 0, Leaky ReLU returns a small negative value proportional to the input x.

## Advantages


## Disadvantages




## ReLU vs Leaky ReLU

- The parameter that controls the slope of the function(a) can be difficult to tune, which can lead to suboptimal performance.
- Leaky ReLU can also be less computationally efficient because it requires additional computation to calculate the slope of the function for non-positive inputs. This extra computation can slow down the training process, especially when dealing with large datasets.

On the other hand, ReLU is a popular activation function due to its simplicity and effectiveness in many deep learning applications which is maybe why it's considered the "gold standard".


