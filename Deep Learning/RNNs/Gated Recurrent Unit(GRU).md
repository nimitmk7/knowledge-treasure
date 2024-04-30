This variant combines the forget and input gates into a single **update gate**, and merges the cell state and hidden state.

![[Pasted image 20240429151017.png]]

## Equations
### Reset Gate
The reset gate determines which information from the previous
time step should be forgotten or reset. It takes the previous hidden state and
the current input and computes the reset gate value.

$$ r_{t} = \sigma(W_{r}\cdot[h_{t-1}, x_{t}]) $$
$$\bar{h_{t}} = \tanh(W\cdot[r_{t}*h_{t-1}, x_{t}])$$
### Update Gate
The update gate decides what information should be passed
from the previous hidden state to the current hidden state. It calculates a value
between 0 and 1 to blend the previous state and a new candidate state.
$$z_t = \sigma (W_{z} \cdot [h_{t-1}, x_{t}])$$

### Cell State Update
$$h_{t} = (1-z_{t})*h_{t-1} + z_{t} * \bar h_{t}$$

### Putting it all together
![[gru_animation.gif]]

## Advantages
1. GRUs have a simpler architecture with **fewer parameters** compared to LSTMs, which makes them **computationally less expensive** and **easier to train**.
