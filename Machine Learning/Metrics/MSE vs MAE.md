1. MSE gives relatively higher weight to large errors. 
	- Thus, when we are trying to avoid large errors in the model predictions, as outliers disproportionately affect MSE more than MAE.
	- Thus, MAE is more robust to outliers
2. Computation-wise, MSE is easier to use.
	- MSE’s gradient calculation is more straightforward than MAE.
	- MAE’s gradient calculation requires some linear programming
3. MSE corresponds to maximizing the likelihood of Gaussian random variables, MAE does not.
4. MSE is minimized by the conditional mean, whereas MAE is minimized by the conditional median.

