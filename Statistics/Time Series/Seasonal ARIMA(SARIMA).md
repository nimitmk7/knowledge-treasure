Seasonal ARIMA(SARIMA) is an extension of [[Autoregressive Integrated Moving Average(ARIMA)]] model that incorporates seasonality in addition to the non-seasonal components. 

## Definition

SARIMA Model is represented as, 
![[Pasted image 20240514101716.png]]
where, 
$m$ = number of observations per year
$P$ = number of seasonal AR terms
$Q$ = number of seasonal MA terms

We use uppercase notation for the seasonal parts of the model, and lowercase notation for the non-seasonal parts of the model.

The seasonal component of SARIMA models adds the following three components:

1. **Seasonal Autoregressive**($P$): This component captures the relationship between the current value of the series and its past values, specifically at seasonal lags.
2. **Seasonal Integrated** ($D$): Similar to the non-seasonal differencing, this component accounts for the differencing required to remove seasonality from the series.
3. **Seasonal Moving Average** ($Q$): This component models the dependency between the current value and the residual errors of the previous predictions at seasonal lags.

## Example

**$\text{ARIMA(1,0,1)(2,1,0)}_{12}$** model is mathematically expressed as,

$$ y_t - y_{t-12} = W_t$$
$$ W_{t} = \mu + a_{1}W_{t-1} + a_{2}W_{t-12} + a_{3}W_{t-24} + b_{1} \epsilon_{t-1} + \epsilon_{t}$$

