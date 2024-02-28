## Definition

A random walk predicts that the value at time **t** will be equal to the last period value + a stochastic(non-systematic) component , $ε_t$ , that is a white centered noise which means that it is IID with mean **0** and variance **fixed**.

$$
P_t = P_{t-1} +  ε_t
$$
- A random walk not can't be predicted. So the best prediction of next price is the current price as E($ε_t$) = 0.
- But if prices are random walks, then there is no point in applying deep learning or machine learning to predict prices. Also can't be predicted using classical statistics that require the predicted series(and residual) to be stationary.
- Thus, we need to show that prices are not random walks, or at least not all the time.

## Types of Random Walk

They all say that the value at time "t" will be equal to the last period value plus a stochastic (non-deterministic) component $ε_t$ ,  but in addition, they may include an additional term that affects the steepness of the random walk. 

This additional term may be a constant, alpha, called a constant drift, or a deterministic trend $\beta$t. 

All these share 2 basic characteristics:

1. All are non-mean reverting processes that can move away from the mean either in a positive or negative direction.
2. The variance evolves over time and goes to infinity as time goes to infinity.

![[random_walk.png]]


## Why is random walk not stationary ?

Lets take an example of random walk without drift, and assume time series started at time 0.

> [!INFO]
> $\mu$ denotes error term in the following equations, instead of mean.
> 

![[Pasted image 20240130215924.png]]
![[Pasted image 20240130220002.png]]
- **Mean is constant, so fulfills the first condition of stationarity**.

![[Pasted image 20240130220233.png]] 
**But variance increases with t, which breaks the premise of stationarity.**

## Tests for Random walk

### 1. AR(1) mean reversion test

In the AR(1) test you model the price series with a regression that says: the price today is equal to a constant plus beta (a slope) times yesterday's price plus a white noise error. 

So, based on this model you do a regression: You take the price as the dependent variable, and the one-day-lag of the price (that is, yesterday's price) as an independent variable. You do a regular regression with intercept, and you look at the resulting beta coefficient you get as the output of the regression.

If |$\beta$| > 1, price will not revert to mean
Else, price is mean reverting, we do not have a random walk.

#### Example
• **Random Walk with drift**: 

$$
P_t = \mu + P_{t-1} +  ε_t
$$

• **Regression test for random walk**:
$$
P_t = \mu + \beta P_{t-1} +  ε_t
$$

Tries to identify a “unit root”, β=1. When β=1 there is no tendency for mean reversion since any epsilon shock to $P_t$  will be carried forward completely through the unit lagged dependent variable $P_{t-1}$ .
.

•**Test**:

$H_0$: β=1 (random walk)

$H_1$: |β|<1 (not random walk. aka there is mean reversion))

**Note:** This AR(1) regression is not valid because the terms are correlated, but it is a first approach.

#### 2. Dickey Fuller Test for Mean Reversion

A version of the AR(1) test that partly corrects the autocorrelated residuals. It modifies the random walk equation by subtracting $P_{t-1}$ on both sides and renaming $\beta_{0}$ to $\beta_1$. 


• **Regression test for random walk**:
$$
P_t = \alpha + \beta_0 P_{t-1} +  ε_t
$$
• **Equivalent to:**
 $$
  P_t-P_{t-1}=α+β_1 P_{t-1}+ ε_t
 $$
$$
∆P=α+β_1 P_{t-1}+ ε_t
$$

Where $β_1=(β_0-1)$  by subtracting $P_{t-1}$ from both sides

•Test:

$H_0$: $β_1$ =0 (Random walk. No mean reversion)

$H_1$: $β_1$≠0 (Not random walk. Mean reversion is possible. Especially if $\beta_1$ 
is negative)

#### 3. Augmented Dickey Fuller Test for Mean Reversion

A statistical test to determine the presence of stationarity in an AR times series of order p.

- $H_0$ : Presence of a unit root **OR** non-stationarity
- $H_1$ : Depends on nature of time series

![[Pasted image 20240131164150.png]]











