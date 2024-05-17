It is an extension of the [[Auto Regressive Moving Average(ARMA)]] model, with the addition of an integration component. 

_ARMA_ models must work on _stationary_ time series. A stationary time series is a series that’s statistical properties, such as mean and variance, do not change over time. 

Unfortunately, majority of real world time series are not stationary, and thus they must often be transformed in order to make them stationary. The process of transformation is referred to as integration. 

The transformation process employed is called differencing, where we take a given $d^{th}$ difference of the series until the series is stationary. We can see this in the image directly below, where a non-stationary time series was transformed by taking the second differences of the series:

![[Pasted image 20240514100956.png]]
The _ARIMA_ Model can therefore finally be expressed with the notation:
$$\text{ARIMA}(p,d,q)$$
where
$p$ is the order of AR model component, 
$d$ is the number of differences to conduct on time series,
$q$ is the order of the MA model component.

## References
1. https://medium.com/analytics-vidhya/a-thorough-introduction-to-arima-models-987a24e9ff71
2. 