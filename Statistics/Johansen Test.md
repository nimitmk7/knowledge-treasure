It is the multi-asset version of the ADF test. 

$$\Delta Y(t) = \Lambda Y(t-1) + M + Bt + A_{1}\Delta Y(t-1) + \dots + A_{k}\Delta Y(t-k) + \varepsilon_{t}$$
$$∆Y(t)=Y(t)- Y (t-1), ∆Y(t-1)=Y(t-1) - Y(t-2)$$


1. The delta Y (∆Y) in (1) stands for the price changes of the portfolio components. 
2. The Y’s are $n \times 1$ vectors representing $n$ price series belonging to $n$ portfolio components.
3. $M$ is a $n \times 1$ vector of means and $\varepsilon_{t}$ is a $n \times 1$ vector of Gaussian errors.
4. The coefficients, $\Lambda$, $A_1$ to $A_k$ etc. stand for $n \times n$ matrices.

$H_{0}$ : $\Lambda = 0$, Component price changes $∆Y(t)$ does not depend on price levels $Y(t-1)$ 

$H_1$: $\Lambda \neq 0$ 

- When  the null hypothesis is true, we do not have co-integration among the components and it is not possible to form a stationary portfolio from them.
- To reject $H_{0}$, the statement that $(\Lambda =0)$ must be rejected by obtaining a critical value of the Johansen test statistic.
- The test statistic r, is constructed based on the eigen-value decomposition of the $n \times n$ matrix $\Lambda$. 
	- When all eigen-values are zero, that would mean that the component series are not co-integrated.
	- Whereas when some of the eigen values contain negative values, it would imply that a linear combination of the component series can be created, which would result in a stationary portfolio.
- Another interpretation of the r statistic is that ==it defines the number of independent portfolios that can be formed by various linear combinations of the components==. 
	- If r equals zero, then the components are not co-integrated and zero stationary portfolios can be constructed from them.
- The Johansen test usually provides the relevant critical values of the test statistic r.
- For simplicity, we can assume drift term to be zero($B = 0$), and the number of lags is 1.

## Dynamic Weights

- The elements of the eigenvectors of L can be used as weights (a.k.a. hedge ratios) to multiply each of the n assets to form the stationary portfolio.
	- The eigenvector corresponding to the largest eigenvalue (a.k.a. principal component) is preferably used for this purpose because it represents the “strongest” co-integrating relationships between the assets.
	- These weights need not add up to 1.
- These weights are often normalized to add up to one, but this is not required for pairs trading.

