

## L1 Regularization
$$
Penalty = \lambda \Sigma^{n}_{i=1} |w_i|
$$
- Weights can go down to zero. 
- Faster computation
- Preferred regularization method for feature elimination
- Linear Regression + L1 Regularization ⮕ Lasso Regression
## L2 Regularization
$$
Penalty = \lambda \Sigma^{n}_{i=1} w_i^2
$$
- Weights cannot go down to 0, but can get very small.
- Preferred regularization to correct model for overfitting
- Linear Regression + L2 Regularization ⮕ Ridge Regression

>[!INFO] $\lambda$ is the hyperparameter that controls the weightage of the regularization term in the loss function.


> [!todo] Add explanation for why L1 can lead to weights going down to zero, and L2 cannot. Use the section from Chip Huyen's book as well.

## Regularization and Bias
When you introduce a penalty in the cost function that has the effect of decreasing the sensitivity of an estimator to the noise of the data, what you are actually doing is, **introducing bias, a fake cost**. When you regularize estimators to reduce their variance, you make them more biased.

## Regularization and Scaling
- Regularization requires us to **scale** the predictor variables first; otherwise the shrinking will affect more strongly the coefficients of predictors that happen to be expressed in small units of measurement.
- If your estimator uses gradient descent, as in the case of neural networks, then it is a good idea to scale the target variable in any case to prevent gradient explosion. 

>[!TODO] Add a section on gradient explosion.
