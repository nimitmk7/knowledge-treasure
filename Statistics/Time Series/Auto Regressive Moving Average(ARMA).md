ARMA is a combination of the [[Auto-Regression(AR)]] and [[Moving Average(MA)]] models.
## Definition
ARMA(p, q) model is a linear generative time-series model that combines the AR(p) and MA(q) models. 
$$ \forall t, \space y_{t} = \sum_{i=1}^{p} a_{i} y_{t-i} + \epsilon_{t} + \sum_{j=1}^{q} b_{j} \epsilon_{t-j} $$
## Example
The predicted variable $y$ can be returns of a stock.
An example of MA(2) model would be: 
$$y_{t} = \epsilon_{t} - \epsilon_{t-1} + 0.8\epsilon_{t-2} $$