## Core Idea
Bagging or **Bootstrap Aggregation** is an ensemble learning method used to reduce variance by combining weak learners having high variance. 
(Refer to: [[Bias-Variance Tradeoff]])

The weak learners are homogeneous, so they are working on the same dataset,
and they each take a random sample of data for training, with replacement. 

By doing this, we can train and learn from each of the weak learners independently and in parallel. The weak learners are then aggregated via a deterministic averaging process.

![[Pasted image 20240416163402.png]]
## Assumptions
-  Bootstrap samples must have “good statistical properties”
	- Must be drawn from the underlying data distribution and independently from each other.
	- Considered representative and independent instances of true data distribution (almost i.i.d)

To verify the above approximation, we need to meet 2 conditions

1. **Representation**: Size of the initial dataset $N$ must be **large enough** that it can capture the complexity of the underlying data distribution.
2. **Independence**: Size of the dataset $N$ must be large enough relative to size of the bootstrap sample $B$ , such that **samples are not too correlated**.

## The Algorithm

### 1. Bootstrapping
It helps create diverse samples of the data for training the weak learners. For each learner, we select data points from the set at random with replacement, allowing for the selection of the same data point multiple times. 

As a result, instances of the data can be repeated in the sample.

### 2. Parallel Training
Bootstrapped samples are trained with different weak learners, independently
and in parallel.

### 3. Aggregation
An average or majority of the predictions are taken to compute a more accurate
prediction. 

- Regression problems use **soft voting**, in which an average of all the outputs of the weak-learners are used as the prediction.
- Classification problems use **hard/majority voting**, in which the class with the most votes is taken as the prediction

## Formulation
Consider we have the following: 
- Data in pairs ($X_i, Y_i$) for $(i = 1, 2, 3, ..., n)$

- $X_i∈ R_d$ where R_d is the d-dimensional predictor variable

-  $Y_i∈:$
	- For regression: $R$
	- For classification ($J$ classes): $0, 1, 2, ..., J − 1$

- Target Function:
	- For regression: $E[Y |X = x]$
	- For classification (J classes): $P[Y = j|X = x]$ for $j = (0, 1, 2, ..., j − 1)$

- Function Estimator: $\hat g(•) = h_n((X_1, Y_1), (X_2, Y_2), ... ,(X_n, Y_n))(•):R^d → R$ 

-  $h_n$ defines the estimator as a function of the data.

1. Construct a Bootstrap Sample

-  Randomly draw instance $(X,Y)$ $n$ times from dataset with replacement.
- Bootstrapped sample training set $(X_1, Y_1), (X_2, Y_2), ..., (X_N, Y_N)$

2. Computing Bootstrap Estimate $\hat g(•)$

 - Plug-in data from step 1 into the equation: 
 $$g(•) = h_n((X_1, Y_1), (X_2, Y_2), ... ,(X_n, Y_n))(•)$$

3. Generating the Bagged Estimate $\hat g_{bag}(•)$

- Repeat steps 1 and 2 M times, where M is the number of weak learners you are using.
- Yields $\hat g^k(•)$ where $(k = 1, 2, ...,M)$, for each learner/estimator.
- Bagged Estimator: $$\frac{1}{M} \sum_{i=1}^{M} \hat g_{i}(•) $$
- In theory: $\hat g_{bag}(•) = E[\hat g^*(·)]$ as $M → ∞.
- For classification problems, Brieman proposed that instead of taking the average we take the output from each weak classifier among J classes as votes for constructing the bagged classifier, and the class with the most votes would become such

## Working Example
Assume we are given a dataset of size N = 1000: $$L = [(X_1, Y_1), (X_2, Y_2), ..., (X_N, Y_N)]$$
### 1. Bootstrapping
We will generate M = 5 training subsets from the original dataset L of size n = 50 by randomly sampling 50 instances from the dataset, with replacement.

$$\begin{pmatrix}
(X_{1,1}, Y_{{1,1}}) & (X_{1,2}, Y_{{1,2}}) & \cdots & (X_{1,50}, Y_{{1,50}}) \\ (X_{2,1}, Y_{{2,1}}) & (X_{2,2}, Y_{{2,2}}) & \cdots & (X_{2,50}, Y_{{2,50}}) \\ \vdots & \vdots & \ddots & \vdots \\ (X_{5,1}, Y_{{5,1}}) & (X_{5,2}, Y_{{5,2}}) & \cdots & (X_{5,50}, Y_{{5,50}})
\end{pmatrix}
$$
### 2. Training the weak learners
We will now train 5 weak learners, one for each of the training subsets we bootstrapped. Each weak learner has a unique regression/classification algorithm, and each weak learner will be trained in parallel to generate an output via estimator h which takes a subset of the data as input.

**a) Regression**

$\hat{g}_1(\cdot) = h_1((X_{1,1}, Y_{1,1}), (X_{1,2}, Y_{1,2}), ..., (X_{1,50}, Y_{1,50}))(\cdot) = 1.55$
$\hat{g}_2(\cdot) = h_2((X_{2,1}, Y_{2,1}), (X_{2,2}, Y_{2,2}), ..., (X_{2,50}, Y_{2,50}))(\cdot) = 1.32$
$\hat{g}_3(\cdot) = h_{3}((X_{3,1}, Y_{3,1}), (X_{3,2}, Y_{3,2}), ..., (X_{3,50}, Y_{3,50}))(\cdot) = 1.73$
$\hat{g}_4(\cdot) = h_4((X_{4,1}, Y_{4,1}), (X_{4,2}, Y_{4,2}), ..., (X_{4,50}, Y_{4,50}))(\cdot) = 1.29$
$\hat{g}_5(\cdot) = h_1((X_{5,1}, Y_{5,1}), (X_{5,2}, Y_{5,2}), ..., (X_{5,50}, Y_{5,50}))(\cdot) = 1.81$

**(b) Classification** (Assume 3 classes)

$\hat{g}_1(\cdot) = h_1((X_{1,1}, Y_{1,1}), (X_{1,2}, Y_{1,2}), ..., (X_{1,50}, Y_{1,50}))(\cdot) = 0$
$\hat{g}_2(\cdot) = h_2((X_{2,1}, Y_{2,1}), (X_{2,2}, Y_{2,2}), ..., (X_{2,50}, Y_{2,50}))(\cdot) = 1$
$\hat{g}_3(\cdot) = h_{3}((X_{3,1}, Y_{3,1}), (X_{3,2}, Y_{3,2}), ..., (X_{3,50}, Y_{3,50}))(\cdot) = 0$
$\hat{g}_4(\cdot) = h_4((X_{4,1}, Y_{4,1}), (X_{4,2}, Y_{4,2}), ..., (X_{4,50}, Y_{4,50}))(\cdot) = 0$
$\hat{g}_5(\cdot) = h_1((X_{5,1}, Y_{5,1}), (X_{5,2}, Y_{5,2}), ..., (X_{5,50}, Y_{5,50}))(\cdot) = 2$

### 3. Aggregation
We will now aggregate the estimates from the weak learners to generate a bagged estimate, which is ideally a more accurate estimate with reduced variance.

**(a) Regression: Average(Soft Voting)**
$$\hat g_{bag} = \frac{1}{M} \sum_{i=1}^{M} \hat g_{i}(•) = \frac{1}{5} \sum_{i=1}^{5} \hat g_{i}(•) $$
(b) **Classification: Hard/Majority Voting**

Votes for Class 0: 3
Votes for Class 1: 1
Votes for Class 2: 1

$$\hat g_{bag} = 0$$

## Why Bagging works ?


## Algorithms using Bagging
[[Random Forest]]

## Use case

Bagging is useful for **reducing variance when training on noisy datasets**. This is helpful more so for **high-dimensional data, where missing or outlier values can lead to higher training variance**, making the learning model more prone to overfitting.



