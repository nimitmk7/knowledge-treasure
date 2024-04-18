## Motivation
1. Avoid the exploding/vanishing gradient problem of vanilla RNNs.

## Core Idea
- Instead of using the same feedback loop connection for events that happened long ago, and events that happen a few intervals ago, to make a prediction about the next sequence,  LSTM uses 2 separate paths to make predictions:
  1. For long term memory
  2. For short term memory


## Long-term memory

## Short-term memory

### LSTM Cell Architecture
![[Pasted image 20240403143603.png]]
$c_{(t)}$ → long-term state/memory
$h_{(t)}$ → short-term state/memory

**Key Idea**: Network can learn
1. What to store in long-term state
2. What to throw away from long-term state
3. What to read from long-term state

At each time step, some memories are dropped, and some memories are added.

### Steps
1. **Forget Gate** : Determine what % of the Long-Term memory is remembered.
2. **Input Gate**: One block creates a potential long term memory from the short-term memory and the input,  and other block determines the % of the potential long-term memory to add to the existing long term memory.
3. **Output Gate**: Update the short-term memory with the help of the new long term memory. Called the **output gate**.
#### Roles
**Input Gate**: Learn to recognize an important input.
**Forget Gate**: Store the important input in the long-term state, and preserve it for as long as it is needed.
**Output Gate**: Extract it whenever it is needed.

#### LSTM Computation Equations
$$i_{(t)} = \sigma(W_{xi}^\top x_{(t)} + W_{hi}h_{(t-1)}+b_i)$$
$$f_{(t)}= \sigma(W_{xf}^\top x_{(t)} + W_{hf}h_{(t-1)}+b_f)$$
$$o_{(t)}= \sigma(W_{xo}^\top x_{(t)} + W_{ho}h_{(t-1)}+b_o)$$
$$g_{(t)}= \sigma(W_{xg}^\top x_{(t)} + W_{hg}h_{(t-1)}+b_g)$$
$$c_{(t)} = f_{t} \otimes c_{(t-1)} + i_{(t)} \otimes g_{{(t)}}  $$
$$
y_{(t)} = h_{(t)} = o_{{t}} \otimes \tanh(c_{(t)})  
$$

#### Analysis
- The main layer is the one that outputs $g_{(t)}$. It analyzes the current inputs $x_{(t)}$ and the previous(short-term) state $h_{(t-1)}$. In a basic cell, there is nothing other than this layer, and its output goes straight out to $y_{(t)}$ and $h_{(t)}$ . But in an LSTM cell, its output does not go straight out; instead its most important parts are stored in the long-term state(and the rest is dropped).
