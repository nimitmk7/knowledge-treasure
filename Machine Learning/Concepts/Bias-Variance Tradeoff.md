## Motivation
Whenever we discuss model prediction, it’s important to understand prediction errors (bias and variance). There is a tradeoff between a model’s ability to minimize bias and variance. Gaining a proper understanding of these errors would help us not only to build accurate models but also to avoid the mistake of overfitting and under-fitting.

## What is Bias ?
Bias is the difference between the average prediction of our model and the correct value which we are trying to predict. 

Model with high bias pays very little attention to the training data and oversimplifies the model. It always leads to high error on training and test data

## What is Variance ?
Variance is the variability of model prediction for a given data point or a value which tells us spread of our data. ==Model with high variance pays a lot of attention to training data and does not generalize on the data which it hasn’t seen before.== As a result, such models perform very well on training data but has high error rates on test data.

## Irreducible Error
Irreducible error is the error that can’t be reduced by creating good models. It is a measure of the amount of noise in our data. Here it is important to understand that no matter how good we make our model, our data will have certain amount of noise or irreducible error that can not be removed.

## Formulation
Let the variable we are trying to predict as Y and other covariates as X. We assume there is a relationship between the two such that

$$
Y = f(X) + e
$$

where e is the error term and it’s normally distributed with a mean of 0. We will make a model $\hat f(X)$  of $f(X)$ using linear regression or any other modeling technique.

The expected squared error at a point $x$ is:
$$Err(x) = E[(Y- \hat f(x))^2]$$

which can be decomposed as:
$$Err(x) = (E[\hat f(x)] - f(x))^2 \space+ E[(\hat f(x) - E[\hat f(x)])]^2 + \sigma_e^2$$
The components:
1. $\mathrm Bias^2$
2. $\mathrm Variance$
3.  $\mathrm Irreducible Error$

## Visual Overview

![[Pasted image 20240315065752.png]]
## Application in Machine Learning

### Under-fitting
In supervised learning, under-fitting **happens when a model unable to capture the underlying pattern of the data**. 

These models usually have **high bias and low variance**. 

It happens when 

1. We have very less amount of data to build an accurate model 
2. When we try to build a linear model with a nonlinear data(or using a simple model to fit complex data patterns.)

### Overfitting
In supervised learning, overfitting happens when **our model captures the noise along with the underlying pattern in data**.

These models have low bias and high variance.

1. It happens when we train our model a lot over noisy dataset. 
2. Fitting a very complex model to a very simple data distribution.

## The Tradeoff
Model too simple(Very few parameters) $\rightarrow$ High Bias, Low Variance
Model too complex(Large no. of parameters) $\rightarrow$ High Variance, Low Bias

 So we need to find the right/good balance without overfitting and underfitting the data.

This tradeoff in complexity is why there is a tradeoff between bias and variance. An algorithm can’t be more complex and less complex at the same time.

![[Pasted image 20240315090442.png]]


### Total Error Minimization
To build a good model, we need to find a good balance between bias and variance such that it minimizes the total error.

![[Pasted image 20240315070645.png]]

An optimal balance of bias and variance would never overfit or underfit the model.

### During the training process
Early in training the bias is large because the predictor output is far from the target function. 

The variance is very small because the data has had little influence yet. Late in training the bias is small because the predictor has learned the underlying function. 

However if train for too long then the predictor will also have learned the noise specific to the dataset (overfitting). In such case the variance will be large because the noise varies between training and test datasets.

## References
1. https://towardsdatascience.com/understanding-the-bias-variance-tradeoff-165e6942b229
2. https://pantelis.github.io/artificial-intelligence/aiml-common/lectures/regression/linear-regression/linear_regression.html

