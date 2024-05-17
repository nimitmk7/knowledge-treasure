## Formulation

AR(p) model is a linear generative time-series model based on the $p^{th}$ order Markov assumption.

$$ \forall t, \space y_{t} = \sum_{i=1}^{p} a_{i} y_{t-i} + \epsilon_{t}$$ where

1. $\epsilon_{t}$ are correlated random variables with $\mu = 0$ and variance = $\sigma$.
2. $a_{1}, \dots , a_{p}$ are AR coefficients. 
3. $y_t$ is the observed stochastic process. 

## Example
The predicted variable $y$ can be sales of a product, or price of a stock, etc. 
An example of AR(2) model would be: 
$$y_{t} = 0.5 y_{t-1} + 0.7y_{t-2} $$





