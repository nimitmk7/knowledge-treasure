## Motivation
In the context of regression it is a statistical measure of how well the regression line approximates the actual data. It evaluates the scatter of the data points around the fitted regression line.

## Formulation and Intuition
**The question to be answered is**:

How much of the variation in y is explained by the variation in the x or the line?

1. Total variation in y is variation of each point from the mean, which is
$$
TSS = \Sigma(y_i - \bar y)^2
$$
2. Unexplained variation in y(due to residuals):
$$
RSS = \Sigma(y_i - \hat y)^2
$$

Thus the final value is, 
$$
R^2 = 1 - RSS/TSS
$$
as 1 - unexplained/total = explained/total.

- If line is a good fit, RSS is small. $R^2$ is close to 1.
- If line is a bad fit, RSS is large, $R^2$ is close to 0.

>[!WARN] 
> $R^2$ should only be used for continuous variables. Prefer using other metrics like **[[Mutual Information]]** for categorical variables. 





## Range of values
We should expect $R^2$ to be bounded between 0 and 1 only if a linear regression model is fit, and it is evaluated on the same data it is fitted on. 

Else, the definition of  $R^2$ being $R^2 = 1 - RSS/TSS$​​ can lead to negative values. How ?

$R^2$ compares the fit of the chosen model with that of a horizontal straight line (the null hypothesis), which is $y$ = $\bar y$. If the chosen model fits worse than a horizontal line, then $R^2$ is negative. 

Note that $R^2$ is not always the square of anything, so it can have a negative value without violating any rules of math. $R^2$ is negative only when the chosen model does not follow the trend of the data, so fits worse than a horizontal line.

![[Pasted image 20240204173021.png]]
## Issues
1. Says nothing about the prediction error
2. $R^2$ can be anywhere between 0 and 1 just by changing the range of X, even with the variance staying exactly the same, and no change in the coefficients. Thus ==$R^2$ can be used as a criterion for comparing linear models as long as you control the range of the data carefully.== E.g. by standardizing the data, or defining a hard range, so you can compare apples to apples, not apples to oranges.
3. It is based on the assumption that the following equation holds: TSS = RSS + SSE. Thus $R^2$ is inappropriate to use for non-linear models. 

## References
1. https://tnwei.github.io/posts/negative-r2/
2. https://stats.stackexchange.com/questions/12900/when-is-r-squared-negative