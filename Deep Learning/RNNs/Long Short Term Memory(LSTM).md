## Motivation
1. Avoid the exploding/vanishing gradient problem of vanilla RNNs.
2. Learn Long Range dependencies in sequence Data

## Core Idea
- Instead of using the same feedback loop connection for events that happened long ago, and events that happen a few intervals ago, to make a prediction about the next sequence,  LSTM uses 2 separate paths to make predictions:
  1. Long term memory
  2. Short term memory

## LSTM Cell Architecture

![[Pasted image 20240429123414.png]]
$C_{t}$ → long-term state/memory(**Cell State**)
$h_{t}$ → short-term state/memory(**Hidden State**)
$f_t$ → forget gate
$i_t$ → input gate
$o_t$ → output gate
#### Forget Gate
This gate is used to decide **what information should be discarded** from the cell state. It looks at the previous hidden state $h_t−1$ and the current input $x_{t}$ and **outputs a number between 0 and 1** for each number in the cell state $C_{t−1}$.

The gate uses a **sigmoid function** in order to do that. 
$$f_t = \sigma(W_{f} · [h_{t−1}, x_t] + b_f )$$
![[Pasted image 20240429141542.png]]
> **Nutshell**: Determine what % of the Long-Term memory is remembered.

### Input Gate
This gate is used to choose ==to what extent new information will be stored in the==
==cell state.== It involves a **sigmoid layer** that decides which values to update and a **tanh layer** that creates a vector of new candidate values.


$$i_t = \sigma(W_i· [h_{t−1}, x_t] + b_i)$$
$$\bar C_t = tanh(W_C \cdot [h_{t-1}, x_t] + b_C)$$ ![[Pasted image 20240429143413.png]]
> **Nutshell**: One block creates a potential long term memory from the short-term memory and the input,  and other block determines the % of the potential long-term memory to add to the existing long term memory

### Output Gate
The output gate ==controls the data that will be used from the cell state to compute the output==. It filters the cell state through a **sigmoid layer** to decide what parts of the cell state will be output.



$$o_t = \sigma(W_o \cdot [h_t-1, x_t] + b_o)$$
$$h_t = o_t * tanh(C_t)$$
![[Pasted image 20240429141745.png]]
> **Nutshell**: Update the short-term memory with the help of the new long term memory


### Cell State Update
$$C_{t} = f_{t} * C_{t} + i_{t} * \bar C_{t}$$

![[Pasted image 20240429142624.png]]

**Key Idea**: Network can learn
1. What to store in long-term state
2. What to throw away from long-term state
3. What to read from long-term state

> [!TIP]
> At each time step, some memories are dropped, and some memories are added.

### Roles
**Input Gate**: Learn to recognize an important input.
**Forget Gate**: Store the important input in the long-term state, and preserve it for as long as it is needed.
**Output Gate**: Extract important memory whenever it is needed.

### Putting it all together
![[lstm_animation.gif]]

## Variants

### [[Peephole LSTM]]
### [[Gated Recurrent Unit(GRU)]]
