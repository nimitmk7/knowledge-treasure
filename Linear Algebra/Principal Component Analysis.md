## Motivation 

- Working with high dimensional data is difficult:
	1. Hard to analyze
	2. Difficult to interpret
	3. Impossible to visualize
	4. Expensive to store
- But there's a catch: **It is overcomplete, many dimensions are redundant and can be explained by a combination of other dimensions**.
- Dimensionality reduction exploits structure and correlation and allows us to work with a more compact representation of the data, ideally without losing information.
	**Similar Example**: mp3 for music, jpeg for images

## Definition

- Orthogonal projection of the data onto a lower dimensional linear space, known as the *principal subspace*, such that the variance of the projected data is maximized.
- Linear projection that minimizes the average projection cost, defined as the mean squared distance between the data points and projections.
## Premise

Consider a dataset of observations {$x_n$} where $n = 1,2,3,..,N$, and $x_n$ is a euclidean variable with dimensionality D. 

The mean is zero, and the covariance matrix is 

$$
S = \frac{1}{N} \Sigma^{N}_{n=1} x_nx_n^{T}
$$
We assume that there exists a low-dimensional compressed representation(code).
$$
z_n = B^T x_n \in R^M
$$  
 of $x_n$ where we define the projection matrix:
$$
B:= [b_1,...,b_M] \in R^{D \times M}
$$
We assume that columns of B are orthonormal so that $b_i^{\top}b_j=0$  if and only if $i\neq j$ and $b_i^{\top}b_i=1$. 
## Objective
We seek an M-dimensional subspace $U\subseteq R^D$ , $dim(U) = M < D$ onto which we project the data, while maximizing the variance of the projected data. 

We denote the projected data by $\bar x_n \in U$ and their co-ordinates(w.r.t to the basis vectors of U) by $z_n$.   

![[Pasted image 20240213174622.png]]
### Maximum Variance Perspective
![[Pasted image 20240213174622.png]]
If we interpret information content in the data as how "space filling" the dataset is, then we can describe the information contained in the data by looking at the spread of the data.

The variance is an indicator of the spread of data, and we can derive PCA as a dimensionality reduction algorithm that maximizes the variance in the low-dimensionality reduction algorithm to retain as much information as possible.

==*Retaining max. information after data compression = Capturing largest amount of variance*==

## The Math

## Direction with max. variance

We start by seeking a single vector $b_1\in R^D$ that maximizes the variance of projected data. i.e. we aim to maximize variance of first co-ordinate $z_1$ of $z\in R^M$  so that:

$$
V_1 := \mathbb V[z_1] = \frac{1}{N} \sum^{N}_{n=1}z_{1n}^2
$$

is maximized, where we exploited the i.i.d assumption of the data and defined $z_{1n}$ as the first co-ordinate of the low-dimensional representation $z_{1n}\in \mathbb R^M$  of $x_n\in \mathbb R^D$. Note that the first component of $z_n$ is given by

$$
z_{1n} = b_1^\top x_n
$$
> [!NOTE] The vector $b_1$ will be the first column of the matrix $B$ and therefore the first of $M$ orthonormal basis vectors that span the lower-dimensional subspace.

i.e. it is the coordinate of the orthogonal projection of xn onto the one- dimensional subspace spanned by $b_1$. Thus, 
$$
V_1 = \frac{1}{N} \sum^{N}_{n=1}(b_1^\top x_n)^2 = \frac{1}{N} \sum^{N}_{n=1} b_1^\top x_n x_n^\top b_1
$$
$$
= b_1^\top(\frac{1}{N} \sum^{N}_{n=1}x_nx_n^\top)b_1 = b_1^\top Sb_1
$$

where $S$ is the covariance matrix .

Notice that arbitrarily increasing the magnitude of the vector $b_1$ increases $V_1$, that is, a vector b1 that is two times longer can result in $V_1$ that is potentially four times larger. Therefore, we restrict all solutions to $\lVert b_1\rVert ^2  = 1$, which results in a constrained optimization problem in which we seek the direction along which the data varies most.

So we have a constrained optimization problem.

$$
\max_{b1} b_1^\top Sb_1
$$
subject to $$
\lVert b_1\rVert ^2  = 1
$$
We will use Lagrangian multiplier to solve this.

$$
\mathfrak L(b_1,\lambda) = b_1^\top Sb_1 + \lambda_1(1-b_1^\top b_1)
$$

The partial derivatives of L with respect to $b_1$ and $\lambda_1$ are:
$$
\frac {\partial\mathfrak L}{\partial b_1} = 2b_1^\top S-2\lambda_1b_1^{\top}, \frac {\partial\mathfrak L}{\partial \lambda_1} = 1 - b_1^{\top}b_1
$$
Setting them to zero gives us:

$$
Sb_1 = \lambda_1b_1, b_1^\top b_1=1
$$
Thus $b_1$ is an eigen vector of the covariance matrix $S$, and $\lambda_1$ is its corresponding eigen value.

Our variance objective can be rewritten as:

$$
V_1 = b_1^\top Sb_1 = \lambda_1b_1^{\top}b_1 = \lambda_1
$$
==**Variance of the data projected onto a 1-dimensional sub-space equals the eigenvalue that is associated with the basis vector $b_1$ that spans this subspace**.==

So to maximize the variance, we choose the basis vector associated with the largest eigenvalue of the data covariance matrix. $\Rightarrow$ **First Principal Component**

We can determine the effect/contribution of the principal component $b_1$ in the original data space by mapping the coordinate $z_{1n}$ back into data space, which gives us the projected data point.

$$
\bar x_n = b_1 z_{1n} = b_1b_1^{\top}x_n \in \mathbb R^D
$$
### Generalizing to m co-ordinates


## References
1. https://mml-book.github.io/book/mml-book.pdf 
2. 