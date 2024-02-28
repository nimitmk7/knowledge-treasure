- Very deep networks using residual connections.

## Motivation

What happens when we continue stacking deeper layers on a "plain" convolutional neural network ?

![[Pasted image 20240224023452.png]]

The deeper model performs worse on both training and test error, and obviously it is not due to overfitting. 

**Hypothesis**: The problem is an optimization problem, deeper models are harder to optimize.

The deeper model should be able to perform at least as well as the shallow model. A solution by construction is copying the learned layers from the shallow model and setting additional layers to identify mapping. 

## Core Idea
Use network layers to fit a residual mapping instead of directly tryin to fit a desired underlying mapping.

![[Pasted image 20240224024036.png]]


## Formulation

The striking difference between ResNets and earlier architectures are the **skip connections**. Shortcut connections are those skipping one or more layers. The shortcut connections simply perform identity mapping, and their outputs are added to the outputs of the stacked layers.

### Example

![[Pasted image 20240224024632.png]]

Each layer consists of a residual module $f_i$ and a skip connection bypassing $f_i$. Since layers in residual networks can comprise multiple convolutional layers, we refer to them as residual blocks. With $y_{i-1}$ as is input, the output of the i-th block is recursively defined as

$y_i = f_i(y_{i−1}) + y_{i−1}$

where $f_i(x)$ is some sequence of convolutions, batch normalization, and Rectified Linear Units(ReLU) as nonlinearities. In the figure above we have three blocks. Each $f_i(x)$ is defined by

$f_i(x) = W_i^{(1)} * \sigma(B (W_i^{(2)} * \sigma(B(x))))$

where $W_i^{(1)}$ and $W_i^{(2)}$ are weight matrices, · denotes convolution, $B(x)$ is batch normalization and $\sigma(x) ≡ max(x, 0)$. 


## Advantages

1. Identity shortcut connections add neither extra parameter nor computational complexity. The entire network can still be trained end-to-end by SGD with backpropagation, and can be easily implemented using common libraries without modifying the solvers. 
2. Dropping out individual neurons during training leads to a network that is equivalent to averaging over an ensemble of exponentially many networks. Entire layers can be removed from plain residual networks without impacting performance, indicating that they do not strongly depend on each other. Thus we can choose no of layers as per computational cost requirements. [Residual Networks Behave Like Ensembles of Relatively Shallow Networks](https://arxiv.org/pdf/1605.06431.pdf) 





