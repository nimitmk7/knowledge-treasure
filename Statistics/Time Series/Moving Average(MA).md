## Definition
MA(q) model is a linear generative time-series model based on the $q^{th}$ order Markov assumption.

$$ \forall t, \space y_{t} = \epsilon_{t} + \sum_{j=1}^{q} b_{j} \epsilon_{t-j} $$ where

1. $\epsilon_{t}$ are correlated random variables with $\mu = 0$ and variance = $\sigma$.
2. $b_{1}, \dots , b_{q}$ are MA coefficients. 
3. $y_t$ is the observed stochastic process. 

## Example
The predicted variable $y$ can be returns of a stock.
An example of MA(2) model would be: 
$$y_{t} = \epsilon_{t} - \epsilon_{t-1} + 0.8\epsilon_{t-2} $$
