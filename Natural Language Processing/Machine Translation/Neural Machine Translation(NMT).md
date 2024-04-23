 It involves doing Machine Translation with a single neural network. The neural network architecture is called sequence-to-sequence (aka **seq2seq**) and it involves two **RNNs**(LSTMs).

## Architecture

![[Pasted image 20240421193257.png | 300]]

### Encoder
It takes the a source sequence as input and encodes it into a **fixed-size** "context vector" $\phi$.

We use an RNN to do this mathematically, which evolves its hidden state as follows:
$$h_{t} = f(x_{t}, h_{t-1})$$
The context vector is generated from the sequence of hidden states,
$$\mathbf \phi = q(\mathbf h_1, ..., \mathbf h_{Tx})$$
If we use a bidirectional RNN, the architecture would look like this:
![[Pasted image 20240421193648.png]]
> [!tip] 
>  Seq2Seq encoders will often do something that initially look strange: they will process the input sequence in reverse. 
>  
>  This is actually done on purpose with the idea being that the last thing that the encoder “sees” will roughly corresponds to the first thing that the model outputs; this makes it easier for the decoder to “get started” on the output.

### Decoder
It uses the context vector from encoder output as a “seed” from which to generate an output sequence. We’d like to use it as a language model that’s "aware" of the **words that it’s generated so far** and of the **input**.

The decoder directly calculates,
$$\mathbf{\hat y}   = \arg \max_y p(\mathbf y | \mathbf \phi)$$
We can write this as:
$$\arg \max_y p(\mathbf y| \mathbf \phi) = \arg \max_y p(y_1, y_2, ..., y_{Ty} | \phi)$$
and then use the product rule of probability to decompose this to:
$$\arg \max_y p(y_{Ty} | y_1, ..., y_{Ty-1}, \mathbf \phi)  \dots p(y_2|y_1, \mathbf \phi ) p(y_1| \mathbf \phi) $$
We can now write,
$$\mathbf{\hat y}   = \arg \max_y p(\mathbf y | \mathbf \phi) = \arg \max \prod_{t=1}^{T_y} p(y_t | y_1, ..., y_{t-1}, \mathbf \phi) $$
In this equation $p(y_t | y_1, ..., y_{t-1}, \mathbf \phi)$ is a probability distribution represented by a softmax across all all the words of the vocabulary. 

We can use an RNN to model the conditional probabilities 
$$LSTM = g(s_t,  y_{t-1}, \phi ) = p(y_t | y_1, ..., y_{t-1}, \mathbf \phi) $$

Once the decoder is set up with its context, we’ll pass in a special token to signify the start of output generation; in literature, this is usually an token appended to the end of the input (there’s also one at the end of the output). 

Then, we’ll run all stacked layers of LSTM, one after the other, following at the end with a softmax on the final layer’s output that can tell us the most probable output word.

_Both_ the encoder and decoder are trained at the same time, so that they both learn the same context vector representation as shown next.

![[Pasted image 20240423104547.png]]








