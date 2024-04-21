## Premise
When we focus on making predictions based on a fixed window of context (i.e. the $n$ previous words), in some cases, the window may not be sufficient to capture the context.

**Example**:  An article discusses the history of Spain and France and somewhere later in the text, it reads “The two countries went on a battle”; clearly the information presented in this sentence alone is not sufficient to identify the name of the two countries.

So we need to have an Neural Network architecture which provides the required long memory(upto a point), and so the obvious choice is a RNN(LSTM) based architecture.

![[Pasted image 20240420232812.png | 500]]


### RNN Advantages
1. Can process **any length** input.
2. Computation for step $t$ can (in theory) use information from **many steps back**
3. **Model size doesn’t increase** for longer input context
4. Same weights applied on every timestep, so there is **symmetry** in how inputs are processed.

### RNN Disadvantages
1. Recurrent Computation is **slow**.
2. In practice, difficult to access information from **many steps back**.

## Training

1. We start with a big corpus of text which is a sequence of tokens $\mathbf x_1, ..., \mathbf x_{T}$ where T is the number of words / tokens in the corpus.
2. Every time step, we feed one word at a time to the LSTM and compute the output probability distribution $\mathbf{\hat  y}_t$, which is, by construction, ==a _conditional_ probability distribution of every word in the vocabulary given the words we have seen so far==.
3. The loss function at time step $t$ is the CE between the predicted probability distribution $and the distribution that corresponds to the one-hot encoded next token. $$J_t(\theta) = CE(\mathbf{\hat  y}_t, \mathbf{y}_t) = - \sum_j^{|V|} \mathbf{y}_{t,j} \log \mathbf{\hat y}_{t,j} = - \log \mathbf{\hat y}_{t,j}$$
4. Average all the t-step losses. 
$$J(\theta) = \frac{1}{T} \sum_t J_t(\theta)$$

**Visual Explanation**:
![[Pasted image 20240420232551.png]]

> [!note] In practice **we don’t compute the total loss over the whole corpus but we train over a finite span** and compute gradients over that span iterating on a stochastic gradient decent optimization algorithm.

- **Words / one-hot vectors**: $$ x^{(t)} \in  \mathbb R^{|V|}$$ 
- **Word embeddings**: $$e^{(t)} = \mathbf Ex^{(t)}$$
- **Hidden States**: $$h^{(t)} = \sigma(W_{h}h^{(t-1)} + W_{e}e^{(t)} + b_{1})$$, where  $h^{(0)}$ is the initial hidden state.
- **Output distribution**: $$\hat y = softmax(Uh^{t} + b_{2}) \in \mathbb R^{|V|}$$
### Practical Considerations
- Computing loss and gradients across entire corpus $x^{(1)}, \dots ,x^{(T)}$ is too expensive!
- In practice, consider $x^{(1)}, \dots ,x^{(T)}$ as a **sentence**(or a **document**)
- So, Compute loss for a sentence (actually a batch of sentences), compute gradients and update weights. Repeat.





