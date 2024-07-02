## Premise

Consider a hidden layer in a neural network. The activations from the previous layer are simply the inputs to this layer. 

In other words, if we are able to somehow normalize the activations from each previous layer then the gradient descent will converge better during training. This is precisely what the Batch Norm layer does for us.

Purposes of using batch norm layer: 
1. Faster Convergence
2. Regularization

## Definition
Batch normalization works by **normalizing the output of a previous activation layer by subtracting the batch mean and dividing by the batch standard deviation**. 

Batch norm is just another network layer that gets inserted between a hidden layer and the next hidden layer. Its job is to take the outputs from the first hidden layer and normalize them before passing them on as the input of the next hidden layer.

## Formulation

![[Pasted image 20240520161349.png | The Batch Norm layer normalizes activations from Layer 1 before they reach layer 2]]

Just like the parameters (eg. weights, bias) of any network layer, a Batch Norm layer also has parameters of its own:

1. ==Two learnable parameters called beta and gamma.==
2. Two non-learnable parameters (Mean Moving Average and Variance Moving Average) are saved as part of the ‘state’ of the Batch Norm layer.

![[Pasted image 20240520161454.png | Parameters of a Batch Norm layer]]

During training, we feed the network one mini-batch of data at a time. During the forward pass, each layer of the network processes that mini-batch of data. The Batch Norm layer processes its data as follows:

![[Pasted image 20240520161554.png]]

### Steps: Training

1. **Activations**: The activations from the previous layer are passed as input to the Batch Norm. There is one activation vector for each feature in the data.
2. **Calculate Mean and Variance**: For each activation vector separately, calculate the mean and variance of all the values in the mini-batch.
3. **Normalize**: Calculate the normalized values for each activation feature vector using the corresponding mean and variance. These normalized values now have zero mean and unit variance.
4. **Scale and Shift:** Unlike the input layer, which requires all normalized values to have zero mean and unit variance, Batch Norm ==allows its values to be **shifted** (to a different mean) and **scaled** (to a different variance)==. It does this by multiplying the normalized values by a factor, gamma, and adding to it a factor, beta. Note that this is an element-wise multiply, not a matrix multiply. These factors are learnable, and hyper-parameters to be set by the model designer.
5. **Moving Averages**: Batch Norm also keeps a running count of the Exponential Moving Average (EMA) of the mean and variance. During training, it simply calculates this EMA but ==does not do anything with it==. At the end of training, it simply ==saves this value as part of the layer’s state, for use during the Inference phase==.
### Steps: Inference

We use the saved mean and variance values for the Batch Norm during Inference.
![[Pasted image 20240520162156.png]]

## Role as a Regularization Agent
The batch norm layer ==introduces some noise and randomness to the layer inputs==. This noise comes from the fact that the normalization is based on the mini-batch statistics, which vary from batch to batch.

This means that the network sees slightly different inputs for each batch, which can prevent it from memorizing the training data and overfitting, thus acting as a form of regularization.

> [!WARNING] Batch normalization generally is not enough to properly regularize on its own and is normally used along with Dropout.
## References
1. [Batch Norm Explained Visually](https://towardsdatascience.com/batch-norm-explained-visually-how-it-works-and-why-neural-networks-need-it-b18919692739)
2. 