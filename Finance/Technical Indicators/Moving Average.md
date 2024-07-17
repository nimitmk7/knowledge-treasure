The moving average helps to level the price data over a specified period by creating a constantly updated average price.

 1. A simple moving average (SMA) is a calculation that takes the arithmetic mean of a given set of prices over a specific number of days in the past.
 2. An exponential moving average (EMA) is a weighted average that gives greater importance to the price of a stock in more recent days, making it an indicator that is more responsive to new information.

## Simple Moving Average
$$SMA = \frac{A_{1} + A_{2} + \dots + A_{n}}{n}$$
**where**:
$A =$ Price of asset in period $n$
$n=$ Number of time periods

## Exponential Moving Average
$$EMA_{t} = \left[V_{t}\times\left(\frac{s}{1+d}\right)\right] + EMA_{y} \times \left[1 -\left(\frac{s}{1+d}\right) \right]$$
**where:**
$EMA_{t} =$ EMA today
$V_t$ = Value today
$EMA_{y}$ = EMA yesterday
$s=$ Smoothing
$d=$ Number of days

