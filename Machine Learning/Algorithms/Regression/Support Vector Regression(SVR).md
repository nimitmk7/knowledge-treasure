## Introduction
SVR extends the margin concept of [[Support Vector Machines]] to regression, allowing for a specified tolerance of errors within a margin around the predicted regression line or curve.

This approach enables SVR to provide robust predictions even in the presence of noisy data, making it a powerful tool for various regression tasks.

SVR’s **ability to handle non-linear relationships**, **its robustness**
**to outliers**, and **its capacity for high-dimensional data modeling** make
it an invaluable tool in the machine learning practitioner’s toolkit.

## SVM vs SVR
![[Pasted image 20240329131908.png]]
1. **Support Vector Foundation**: - Both SVM and SVR are built on the fundamental concept of support vectors. They identify critical data points crucial for defining the decision boundary (in SVM) or the regression hyperplane (in SVR).
2. **Kernel Trick**: - SVM and SVR both utilize kernel functions to handle non-linear data. This allows them to implicitly operate in high-dimensional feature spaces, making them versatile for capturing complex data relationships.
3. **Loss Functions**: - While the specific loss functions differ (hinge loss for SVM and epsilon-insensitive loss for SVR), both aim to minimize errors while incorporating a margin of tolerance. The idea is to balance fitting the data closely with allowing flexibility.
4. **Hyperparameters**: - Both SVM and SVR involve the use of hyperparameters. SVM has the regularization parameter C, while SVR has the tolerance parameter ε. Adjusting these parameters allows for a trade-off between maximizing the margin or minimizing errors. 

## Core Idea

### Goal
Find a hyperplane that best fits the data points in a n-dimensional continuous space.

## The Margin
It is the error tolerance of the model, also called the ε-insensitive tube. The tube allows some deviation of the data points from the hyperplane without being counted as errors.

ε is a value that we set ourselves. If we allow the model to have a high tolerance, we can set ε to a relatively large value. However, if we do not want the model to have such a high tolerance, we can set ε to a smaller value.

It creates a balance between the complexity of the model and its generalization power.
## The Hyperplane
It is the best possible fit to the data that falls within the ε-insensitive tube. 

## Slack Variables
For any values that fall outside of the ε-insensitive tube, we denote its deviation from the margin as ξ.

We know that these deviations have the potential to exist, but we would still like to minimize them as much as possible.

## Mathematical Model

![[Pasted image 20240329134433.png]]
