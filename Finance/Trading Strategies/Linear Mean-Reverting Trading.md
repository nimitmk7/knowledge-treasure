Once we determine that: 
1. The price series is mean reverting, and 
2. The half-life of mean reversion for the price series short enough for our trading horizon, 

We can easily trade this price series profitably using a simple linear strategy. 

**Core idea**: Take positions that profit from the price moving back toward its mean.

## Strategy
1. Price above mean → Take a short position
2. Price below mean → Take a long position
3. The further the price is from mean, the larger the position. 

It is called linear because the position size changes linearly with the deviation from the mean.

## Calculating the signal
For all the moving averages and standard deviations, the look-back period is equal to the half-life of mean reversion.

1. Calculate the moving average of the time series.
2. Calculate the moving standard deviation of the time series.
3. Normalized Deviation = (Current Price - Moving Average)/Moving Standard Deviation

The normalization (dividing by standard deviation) helps to adjust for changing volatility, providing a form of risk management.

## Extension to Portfolio
Given a set of securities, a mean-reverting portfolio with short half-life can be determined using the [[Johansen test]], which identifies co-integrated assets. 

The "best" eigenvector from this test determines the optimal weights of each asset in the portfolio.

The strategy trades units of the entire portfolio rather than individual assets. Negative weights indicate short positions, positive weights indicate long positions. 

## Pros and Cons of Mean-Reverting Strategies

### Pros
1. It is often fairly easy to construct mean-reverting strategies because we are not limited to trading instruments that are intrinsically stationary. We can pick and choose from a great variety of co-integrating stocks and ETFs to create our own stationary, mean-reverting portfolio.
2. There is often a good fundamental story behind a mean-reverting pair.
3. They span a great variety of time scales. 
	1. At one extreme, market-making strategies rely on prices that mean-revert in a matter of seconds. 
	2. At the other extreme, fundamental investors invest in undervalued stocks for years and patiently wait for their prices to revert to their “fair” value.

### Cons
1. The high consistency of mean-reverting strategy may lead to its eventual downfall. It lulls traders into over-confidence and over-leverage as a result.
2. When a mean-reverting strategy suddenly breaks down, perhaps because of a fundamental reason that is discernible only in hindsight, it often occurs when we are trading it at maximum leverage after an unbroken string of successes. So the rare loss is often very painful and sometimes catastrophic.

> Risk management for mean reverting is particularly important, and particularly difficult since the usual stop losses cannot be logically deployed.




