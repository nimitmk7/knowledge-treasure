## How to calculate Log Returns ?

$$
lr_t = ln(price_t/price_{t-1})
$$
_where $price_t$ is the price of the asset at time t and $price_{t-1}$ is the price of the asset at time t-1.


## Advantages of Log Returns

1. They are time additive. 
	- That is, if we have a series of log returns for a period of time, we can add them up to get the total log return for that period. Simple returns are not time additive.
2. Log returns are symmetric around zero. 
	- Positive and negative log returns are equally far from zero on a logarithmic scale. Why ?
	- The log function suppresses big positive values while emphasizing small negative values, a natural consequence of logarithms.(Observe the changing slop of the log graph around zero)
	- ![[Pasted image 20240131201959.png]]
	
3. Log returns are normally distributed, if we assume that prices follow a log normal distribution.
4. Log returns simplifies complex calculations. 
	- If you want to calculate the compound return for a period with multiple assets, you can use the sum of the log returns for each asset, rather than calculating the product of the simple returns for each asset. 
	- This can be especially useful in portfolios with many assets, where calculating the compound return using simple returns can be computationally intensive and error-prone.

## Disadvantages of Log Returns

1. They are not portfolio additive, but simple returns are.
	- The weighted average of log returns of individual stocks is not equal to the portfolio return. In fact, log returns are not a linear function of asset weights
2. Higher volatility widens the gap between simple and log returns.

## References
1. https://www.youtube.com/watch?v=PtoUlt3V0CI 
2. https://medium.com/quant-factory/why-we-use-log-return-in-finance-349f8a2dc695
3. https://gregorygundersen.com/blog/2022/02/06/log-returns/