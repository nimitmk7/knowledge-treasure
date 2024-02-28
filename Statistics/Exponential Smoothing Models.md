## Motivation

A criticism of the moving average(MA) method is that it puts equal weight on each value in a typical moving average when making a forecast. But we reasonably expect later observations to be weighted more heavily because later observations contain more up-to-date information. 

Exponential smoothing bases its forecasts on a weighted average of past observations, with ==more weight put on the more recent observations==. Like moving averages, exponential smoothing can be used to filter noise and to forecast.

**Core Idea**: The basic idea of this model is to assume that the future will be more or less the same as the (recent) past.

## Types

| Trend(Yes/No) | Seasonality(Yes/No) | Exponential Smoothing Model |
| ---- | ---- | ---- |
| No | No | Simple |
| Yes | No | Holt's method |
| Yes | Yes | Winter's method |

## Formulation

The level is the average value around which the series varies over time. As you can observe in the figure below, the level is a smoothed version of the series(demand in the example).

![[Pasted image 20240220123140.png]]
### Update estimate of a level
$$ L_t=αY_t+(1-α)L_{t-1} $$
where $L_t$ is the level of the series at time t. This level is not observable and can only be estimated, it is where we think the series would be if there were no random noise. $Y_t$ is is the current observation and $L_{t-1}$ is the previous level.

### Exponential Smoothing Factor
Here, $0\lt\alpha\leq1$ , and is called the exponential smoothing factor. It controls how much importance the model will allocate to the most recent observation compared to the the older observations.

When exponential smoothing is used for noise reduction, the exponential smoothing factor alpha is defined as:
$$\alpha = \frac{2}{n+1}$$
where n is the number of days in the span(the lookback window) used to calculate **EMA**(Exponential Moving Average).

Thus, ==to define the degree of smoothing, you can either define the window span or alpha==. Thus exponential smoothing is a window function. 

### Making a forecast
$$ F_{t+k}=L_t $$
Thus, the forecast includes a part of the previous series value as well as a part of the previous forecast. 

### Unfolding the estimate


$$L_t=αY_t+α(1-α)Y_{t-1}+ α(1-α)^2 Y_{t-2}+ α(1-α)^2 Y_{t-3}+ … $$
### Alpha's significance
If $\alpha$  is close to 0, $1-\alpha$ is close to 1, and weights decrease very slowly, meaning that observations from the distant past continue to have a large influence in the next forecast, and this, in turn, means that the graph of the forecasts will be relatively smooth.

>[!INFO] 
> There is no universally accepted value of $\alpha$, practitioners recommend 
> a value between 0.1 and 0.2., and some implementations have an optimization feature to find this optimal value of $\alpha$ , as is the case for the ExponentialSmoothing function of the `statsmodels.tsa` library

### Holt's Exponential Smoothing
- If the series has a trend, the exponential smoothing needs to deal with trend explicitly to avoid lagged forecasts. Holt's method includes a trend term.

$$ L_t=αY_t+(1-α)(L_{t-1}+T_{t-1}) $$
where
$$ T_t=β(L_t-L_{t-1})+(1-β)T_{t-1}$$

The trend term represents an estimate of the change in the series from one period to the next. The new smoothing parameter $\beta$ controls how quickly the method reacts to perceived changes in trend. If $\beta$ is small, the method reacts slowly.

#### Forecast

$$F_{t+k} = L_t + kT_t$$


>[!INFO]
>Some practitioners set $\beta$ equal to $\alpha$, and some software packages have an optimization option available for the best smoothing constants, $\alpha$, and $\beta$ as is the case for the ExponentialSmoothing function of the `statsmodels.tsa` library.

### Winter's Exponential Smoothing
- If the series has seasonality, the exponential smoothing needs to deal with the seasonality explicitly. Winter's method includes a multiplicative seasonal index for the period t.
$$ L_t=α \frac{Y_t}{S_{t-M}} +(1-α)(L_{t-1}+T_{t-1}))$$

$$ T_t=β(L_t-L_{t-1})+(1-β)T_{t-1}$$
$$S_t=\gamma\frac{Y_t}{L_t} +(1-\gamma)S_{t-M}$$
$$F_{t+k}=(L_t+kT_t)S_{t+k-M}$$

Here, $M$ refers to the number of seasons(4 for quarterly data, 12 for monthly data). The new smoothing constant $\gamma$ controls how quickly the method reacts to perceived changes in the pattern of seasonality: If $\gamma$ is small, the method reacts slowly.
## Application

- Smoothing is a technique that is normally applied to prices, not usually to returns. 
- 

## Code implementation

Use `pandas.ewm` library or `statsmodels.tsa` library.

