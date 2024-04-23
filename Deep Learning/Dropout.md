## Premise
The deep neural networks have different architectures, sometimes shallow, sometimes very deep trying to generalise on the given dataset. 

But, in this pursuit of trying too hard to learn different features from the dataset, they sometimes learn the **statistical noise** in the dataset. 

This definitely improves the model performance on the training dataset but fails massively on new data points (test dataset). This is the problem of **overfitting.**

To tackle this problem we have various regularisation techniques that penalise the weights of the network but this wasn’t enough

## Definition
It is a technique used in neural networks to prevent overfitting the training data by randomly dropping out neurons with probability $p$ > 0. It forces the model to avoid relying too much on particular sets of features.

![[Pasted image 20240423105513.png | 400]]
### Working example

Input $x: \{1,2,3,4,5\}$
$p: 0.2$

Forward propagation(I/P Layer) → $x:\{1,0,3,4,5\}$ or $x:\{1,2,0,4,5\}$ or …

Similarly, it applied to the hidden layers. For instance, if the hidden layers have 1000 neurons (nodes) and a dropout is applied with drop probability = 0.5, then 500 neurons would be randomly dropped in every iteration (batch).

## During training
At each training iteration, dropout randomly selects a subset of neurons to be temporarily ignored. This means their outputs are set to zero, effectively removing them from the network for that particular iteration. 

The selection of neurons to drop out is typically done randomly, and the dropout rate $\mathbf p$ is a hyper-parameter that determines the probability of any given neuron being dropped out.

## During testing
During the testing or inference phase, dropout is **not applied**. Instead, the full network is used. 

However, to compensate for the fact that more neurons are active during testing than during training, the **outputs of all neurons are scaled down by the dropout rate**. 

This scaling **ensures that the expected output of each neuron remains approximately the same** during training and testing.

![[Pasted image 20240423113303.png]]
## Practical Considerations
- Generally, for the input layers, the keep probability, i.e. $1- p$, is closer to 1, 0.8 being the best as suggested by the authors. 

- For the hidden layers, the greater the $p$ more sparse the model, where 0.5 is the most optimized keep probability, that states dropping 50% of the nodes

## How does it solve overfitting ?
To be precise, the main motive of training is to decrease the loss function, given all the units (neurons). So in overfitting, a unit may change in a way that fixes up the mistakes of the other units. This leads to complex co-adaptations, which in turn leads to the overfitting problem because this complex co-adaptation fails to generalize on the unseen dataset.

Now, if we use dropout, it prevents these units to fix up the mistake of other units, thus preventing co-adaptation, as in every iteration the presence of a unit is highly unreliable. So by randomly dropping a few units (nodes), it forces the layers to take more or less responsibility for the input by taking a probabilistic approach.

This ensures that the model is getting generalized and hence reducing the overfitting problem.

![Hidden layer features](dropout_example.png)

## The Math

Forward propagation equation in standard neural net:
$z_i = w_ix_i + b_{i}$
$y_i = f(z_i)$

Forward propagation equation in neural net with dropout:
$r_{i} \sim \text{Bernoulli}(p)$
$x’_i = r_{i} * x_i$
$z_i = w_ix'_i + b_{i}$
$y_i = f(z_i)$

So before we calculate **z,** the input to the layer is sampled and multiplied element-wise with the independent Bernoulli variables. **r** denotes the Bernoulli random variables each of which has a probability $p$ of being 1. 

Basically, **r** acts as a mask to the input variable, which ensures only a few units are kept according to the keep probability of a dropout.

## Hands-on

### PyTorch
Adding dropout to your PyTorch models is very straightforward with the `torch.nn.Dropout` class, which takes in the dropout rate – the probability of a neuron being deactivated – as a parameter.

``` Python

class Net(nn.Module):
  def __init__(self, input_shape=(3,32,32)):
    super(Net, self).__init__()
    
    self.conv1 = nn.Conv2d(3, 32, 3)
    self.conv2 = nn.Conv2d(32, 64, 3)
    self.conv3 = nn.Conv2d(64, 128, 3)
    
    self.pool = nn.MaxPool2d(2,2)


    n_size = self._get_conv_output(input_shape)
    
    self.fc1 = nn.Linear(n_size, 512)
    self.fc2 = nn.Linear(512, 10)


    # Define proportion or neurons to dropout
    self.dropout = nn.Dropout(0.25)
      
  def forward(self, x):
    x = self._forward_features(x)
    x = x.view(x.size(0), -1)
    
	# Apply dropout
    x = self.dropout(x)
    x = F.relu(self.fc1(x))
    
    # Apply dropout
    x = self.dropout(x)
    x = self.fc2(x)
    return x
```

## References
1. [Dropout in Neural Networks - Towards Data Science](https://towardsdatascience.com/dropout-in-neural-networks-47a162d621d9)
2. [Implementing Dropout Regularization in PyTorch](https://wandb.ai/authors/ayusht/reports/Implementing-Dropout-Regularization-in-PyTorch--VmlldzoxNTgwOTE) 