## Generalization
Suppose that we are given the training set $\mathcal D = \{(\mathbf x_i, y_i), i =1 \dots m\}$ where $\mathbf x_i \in \mathbb R^n$ the label $y_i \in \mathbb R$. 

We need to construct a model such that a *suitably chosen* loss function is minimized for a **different** set of input data, the so-called test set. The ability to correctly *predict* when observing the test set, is called **generalization**.

If the output y is a continuous value, then the supervised learning problem is a regression problem. Otherwise, it is a classification problem.

## Hypothesis

$$g(\mathbf{w},\mathbf{x}) = w_0 + \sum_{j=1}^{M-1}w_j \phi_j(\mathbf x) = \mathbf w^T \mathbf \phi(\mathbf x)$$
where $\phi_j(\mathbf x)$ are know as _basis functions_.


## Loss Function: Mean Squared Error(MSE)

$$L(\mathbf{w}) = \frac{1}{m} \sum_{i=1}^m \{(g(\mathbf{w},x_i)-y_i)\}^2$$

*The loss function chosen for this regression problem, corresponds to the sum of the squares of the displacements of each data point and our hypothesis. The sum of squares in the case of Gaussian errors gives raise to an (unbiased) Maximum Likelihood estimate of the model parameters*.

Now, 
$$MSE = \mathbb{E}[(\hat{y}_i - y_i)^2] = \mathrm{Bias}{({\hat{y}_i}, y_i)}^2 + \mathrm{Var}(\hat{y}_i)$$


## Goal
Select the hypothesis that minimizes the loss function. 
We essentially have to choose 2 things: 

1. Weight vector: $\mathbf{w^*}$ 
2. **$M$** the order of the polynomial. 

**Both** define our hypothesis. If you think about it, the order $M$ defines the model complexity in the sense that the larger $M$ becomes the more the number of weights we need to estimate and store.

## Overfitting
You can reduce the training error to almost zero by selecting a model that is complicated enough (M=9) to perfectly fit the training data (if m is small).

But this is not what you want to do. Because when met with test data, the model will perform far worse than a less complicated model that is closer to the true model (e.g. M=3). 

This is a central observation in statistical learning called **overfitting**. In addition, you may not have the time to iterate over M parameters in real-world settings.

![[Pasted image 20240314221927.png]]

## Regularization

To avoid overfitting we have multiple strategies. One straightforward one is evident by observing the wild oscillations of the $\mathbf{w}$ elements as the model complexity increases. We can penalize such oscillations by introducing the $l_2$ norm of $\mathbf{w}$ in our loss function.
  

$$L(\mathbf{w}) = \frac{1}{2} \sum_{i=1}^m \{(g(\mathbf{w},x_i)-y_i)\}^2 + \frac{\lambda}{2} ||\mathbf{w}||^2$$

  
This type of solution is called **regularization** and because we effectively shrink the weight dynamic range it is also called in statistics, shrinkage or **ridge regression**. 

We have introduced a new parameter $\lambda$ that **regulates the relative importance of the penalty term as compared to the MSE**. This parameter together with the polynomial order is what we call *hyperparameters* and we need to optimize them as both are needed for the determination of our final hypothesis $g$.

If $\lambda$ is very small, the model will most likely lead to overfitting.
If $\lambda$ is very high, the model will most likely lead to underfitting.



