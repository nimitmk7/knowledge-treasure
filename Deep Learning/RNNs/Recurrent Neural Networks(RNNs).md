## Motivation
A great many learning tasks require **dealing with sequential data**. Image captioning, speech synthesis, and music generation all require that models produce outputs consisting of sequences.

In other domains, such as time series prediction, video analysis, and musical information retrieval, a model must learn from inputs that are sequences. 

These demands often arise simultaneously: tasks such as translating passages of text from one natural language to another, engaging in dialogue, or controlling a robot, demand that models both ingest and output sequentially structured data.

## Introduction
Recurrent neural networks (RNNs) are deep learning models that capture the dynamics of sequences via **_recurrent_ connections**, which can be thought of as cycles in the network of nodes.

Recurrent neural networks are _unrolled_ across time steps (or sequence steps), with the _same_ underlying parameters applied at each step.

While the standard connections are applied _synchronously_ to propagate each layer’s activations to the subsequent layer _at the same time step_, the recurrent connections are _dynamic_, passing information across adjacent time steps

**Essence**: A recurrent neural network looks very much like a feedforward neural network, except it also has connections pointing backward.

Watch this video: [Recurrent Neural Networks (RNNs), Clearly Explained!!!](https://youtu.be/AsNTP8Kwu80?si=Ork-tMCdpzQ2_Sm5)

<iframe width="560" height="315" src="https://www.youtube.com/embed/AsNTP8Kwu80?si=FZbNjjDKjHhXMuQX" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Recurrent Neurons and Layers

### Single node RNN

![[Pasted image 20240403130106.png]]
At each *time* step $t$(also called a frame), this recurrent neuron receives the inputs $\mathbf x_{(t)}$ as well as its own output from a previous step $\hat y_{(t-1)}$. Since there is no previous output at the first time step, it is generally set to 0.

### Single Layer RNN

Every neuron now receives both the input vector $\mathbf x_{(t)}$ and the output vector from the previous step $\hat y_{(t-1)}$. Note that both the inputs and outputs are now vectors. 
![[Pasted image 20240403130742.png]]
Each recurrent neuron has 2 sets of weights(weight vectors): 
1. For the inputs $\mathbf x_{(t)}$ : $\mathbf{w_{x}}$
2. For the outputs of previous step $\hat y_{(t-1)}$ : $\mathbf{w_{\hat y}}$

So for the whole layer, 2 weight matrices: $\mathbf{W_{x}}$ and $\mathbf{W_{\hat y}}$

#### Output for a single data point
$$
\hat{y}_{(t)} = \phi(\mathbf{W_{x}}^\top \mathbf x_{(t)} + \mathbf{W_{\hat y}}\mathbf{\hat y_{(t-1)}} + \mathbf{b}) 
$$

#### Output for a mini-batch
$$
\hat{Y}_{(t)} = \phi(\mathbf X_{(t)}\mathbf{W_{x}}  + \mathbf{\hat Y_{(t-1)}}\mathbf{W_{\hat y}} + \mathbf{b}) 
$$
which is equal to:
$$\hat{Y}_{(t)} = \phi([\mathbf X_{(t)} \space \mathbf{\hat Y_{(t-1)}]\mathbf{W}}\ + \mathbf{b})$$ with
$$ W = [W_{x}\space W_{\hat{y}}]^\top $$
##### Dimensions and Details
1. $\hat{Y}_{(t)}$ → $m \times n_{neurons}$ matrix. Layer’s outputs at time step $t$ for each instance in the mini-batch(size $m$).
2. $X_{(t)}$ → $m \times n_{inputs}$ matrix. Inputs for all instances(No. of input features is $n_{inputs}$ )
3. $W_{x}$ → $n_{inputs} \times n_{neurons}$
4. $W_{\hat{y}}$ → $n_{neurons} \times n_{neurons}$
5. $b$ → $n_{neurons} \times 1$ 

### Memory Cells
Since the output of a recurrent neuron at time step $t$ is a function of all the inputs from previous time steps, you could say it is a form of *memory*. A part of a neural network that preserves some state across time steps is called a *memory cell*. 

A single recurrent neuron, or a layer of recurrent neurons, is a very basic cell, capable of learning only short term patterns. 

A cell’s state at time step t, denoted $h_{}(t)$ (the “h” stands for “hidden”), is a function of some inputs at that time step and its state at the previous time step: $h_{}(t) = f(x_{(t)}, h_{(t–1)})$. 

Its output at time step t, denoted $\hat{y}_{(t)}$, is also a function of the previous state and the current inputs. In the case of the basic cells, the output is just equal to the state, but in more complex cells this is not always the case.

![[Pasted image 20240403134054.png]]
### Input and Output Sequences

![[Pasted image 20240403134848.png]]
1. **Sequence-to-Sequence**: Simultaneously take a sequence of inputs and produce a sequence of outputs.
	- **Example**: forecast time series, such as your home’s daily power consumption: you feed it the data over the last N days, and you train it to output the power consumption shifted by one day into the future (i.e., from N – 1 days ago to tomorrow).
2. **Sequence-to-Vector**: Feed the network a sequence of inputs and ignore all outputs except for the last one. 
	- **Example**: you could feed the network a sequence of words corresponding to a movie review, and the network would output a sentiment score (e.g., from –1 [hate] to +1 [love]).
3. **Vector-to-sequence**: Feed the network the same input vector over and over again at each time step and let it output a sequence.
	- **Example**: The input could be an image (or the output of a CNN), and the output could be a caption for that image.
4. **Encoder-Decoder**: Sequence-to-vector + Vector-to-sequence network. 
	- **Example**: You would feed the network a sentence in one language, the encoder would convert this sentence into a single vector representation, and then the decoder would decode this vector into a sentence in another language.
## Training RNNs
**Trick**: Unroll it through time and then use regular back-propagation → BPTT
![[Pasted image 20240403142145.png]]
- Forward Pass through the unrolled network.
- Evaluate the output sequence using a loss function:
$ℒ(Y_{(0)}, Y_{(1)}, …​, Y_{(T)}; Ŷ_{(0)}, Ŷ_{(1)}, …​, Ŷ_{(T)})$ (where $Y_{(i)}$ is the $i^{th}$ target, $Ŷ_{(i)}$ is the $i_{th}$ prediction, and $T$ is the max time step)

> [!INFO] This loss function might ignore some outputs depending on the use-case. Example: Sequence to Vector RNN

- Propagate the gradients of the loss function backward through the unrolled network.
> Since the same parameters $\mathbf W$ and $\mathbf b$ are used at each time step, their gradients will be tweaked multiple times during back-propagation. 

- Perform gradient descent step and update the parameters.

## Advantages
1. Handle **multiple time series inputs** simultaneously
2. Predict **multiple values at once**,
3. Capture complex **non-linear relationships** in the data
4. They are **dynamic** and can perform online learning
5. Excellently handles **variable-length** sequences


## Issues
1. Suffers from exploding/vanishing gradient problem.
2. 




## Types
1. [[Long Short Term Memory(LSTM)]]
2. [[Gated Recurrent Unit]]
3. 


## References
1. https://d2l.ai/
2. Hands-on Machine Learning book
3. https://mlquant.notion.site/F-Prediction-RNNs-409e415a8f6a485db2c32b2e5bdd96c4
4. 


