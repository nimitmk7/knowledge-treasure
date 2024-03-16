### Definition
The dying ReLU problem refers to the scenario when many ReLU neurons only output values of 0. This happens when the inputs to ReLU are in the negative range.

## Effects of dying ReLU on Neural Network
When most of these neurons return output zero, the gradients fail to flow during backpropagation, and the weights are not updated. Ultimately a large part of the network becomes inactive, and it is unable to learn further.

Because the slope of ReLU in the negative input range is also zero, once it becomes dead (i.e., stuck in negative range and giving output 0), it is likely to remain unrecoverable.

**As long as NOT all the inputs** push ReLU to the negative segment (i.e., some inputs are in the positive range), the neurons can stay active, the weights can get updated, and the network can continue learning.

## Potential Causes:

### 1. High Learning Rate
- If learning rate is set too high, our new weights will end up in the highly negative range, since our old weights will be subtracted from a large number.
$$ W_{ij} = W_{ij} - \eta\left(\frac{\partial E}{\partial W_{ij}}\right)$$
### 2. Large Negative Bias
- A large negative bias term can cause the ReLU activation inputs to become negative. Input to the ReLU function is $W*x + b$.

### 3.  Modification of initialization procedure
- A common way for initializing weights and biases for neural networks is through symmetric probability distributions (e.g., He initialization). However, such a method is prone to the dying ReLU problem due to bad local minima
- It has been demonstrated that using a **randomized asymmetric initialization** can help prevent the dying ReLU problem



