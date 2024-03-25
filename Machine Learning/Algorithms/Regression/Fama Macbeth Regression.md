$$
Return(t,s)-r_F=α(s)+β_1(s)∗Factor_1(t)+β_2(s)∗Factor_2(t)+…+ε(t,s)
$$
- Multi-Input, Multi-Target Regression
- Intent of this regression is explanatory, not predictive, because the regression targets are returns that are not SHIFTED FORWARD($Return(t,s)$ instead of $Return(t+1,s)$).

## Target
- $Return(t,s)$ is the return of a stock at time t.
- $r_F$ is the risk-free rate over same period t

## Regressors
- The [[Fama French Factors]] are the regressors.
- Each factor is indicated with sub-index. 
- The $Factor_i$ are called “time series factors.” They vary from with time but do NOT vary across stocks.

## Co-efficients
- The $β_i(s)$  in red are the regression coefficients (the “betas”). 
- They are dimensionless weights.
- The $β_i(s)$ are  called “factor loadings or factor exposures” of that stock s.
- The $β_i(s)$ DO vary across stocks.

## Intercept
- The α(s) is regression intercept.
- It is set as a constant in time. 

## Error
- The noise term ε(t,s) is included to represent white noise.

## Factor "Risk Premia"
- A factor's “risk premia” measures how much profit the investor gets per unit of risk invested in that factor.
- The factors with the largest “**risk premia**” (the most profitable factors) are:
1. Mom (momentum)
2. Mkt-RF (market excess return over the risk-free rate)

- 
