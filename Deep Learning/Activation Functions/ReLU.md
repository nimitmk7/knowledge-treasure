**ReLU** stands for Rectified Linear Unit. 

## Formulation

$$
g(z) = max\{0,z\}
$$

![[Pasted image 20240216101652.png]]
## Intuition
- The function **returns 0 if it receives any negative input, but for any positive value x it returns that value back**

## Advantages
1. **Computational Simplicity**
2. **Representational Sparsity**: An important benefit of the rectifier function is that it is capable of outputting a true zero value.
	- This is unlike the tanh and sigmoid activation functions that learn to approximate a zero output, e.g. a value very close to zero, but not a true zero value.
	
	-  This means that negative inputs can output true zero values allowing the activation of hidden layers in neural networks to contain one or more true zero values. This is called a sparse representation and is a desirable property in representational learning as it can accelerate learning and simplify the model.
3.  The rectifier function mostly looks and **acts like a linear activation function**. 
	- In general, a neural network is easier to optimize when its behavior is linear or close to linear.
4. **Avoid the problem of vanishing gradients**: Key to this property is that networks trained with this activation function almost completely avoid the problem of vanishing gradients, as the gradients remain proportional to the node activations.

## Disadvantages
1. Non-differentiable at zero.
2. Unbounded 
3. The gradients for negative input are zero, which means for activations in that region, the weights are not updated during backpropagation. This can create dead neurons that never get activated. (This can be handled by reducing the learning rate and bias.)
4. ReLU output is not zero-centered and it does hurt the neural network performance. The gradient of the weights during backpropagation are either all be positive, or all negative. This could introduce undesirable zig-zagging dynamics in the gradient updates for the weights. (Can be handled by batchnorm)
5. The mean value of activation is not zero. From ReLU, there is a positive bias in the network for subsequent layers, as the mean activation is larger than zero. The positive mean shift in the next layers slows down learning.

## Improvements

1. [[Leaky ReLU]]
2. [[Randomized Leaky ReLU]]
3. [[ELU and SELU]]
4. [[Concatenated ReLU (CReLU)]]
5. [[ReLU-6]]

## References

1. https://machinelearningmastery.com/rectified-linear-activation-function-for-deep-learning-neural-networks/ 
2. https://www.deeplearningbook.org/
3. https://prateekvishnu.medium.com/activation-functions-in-neural-networks-bf5c542d5fec 
4. https://ai.stackexchange.com/questions/40576/why-use-relu-over-leaky-relu


