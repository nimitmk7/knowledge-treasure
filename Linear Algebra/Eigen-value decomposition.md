## Definition and Mathematical Formulation
An eigenvector of a square matrix $A$ is a nonzero vector $v$ such that multiplication by
$A$ alters only the scale of $v$ : 
$$
Av = λv
$$
- Scalar λ is known as the **eigenvalue** corresponding to this **eigenvector**. 
- If $v$ is an eigenvector of A, then so is any rescaled vector $sv$ for $s$ ∈ $R$,$s\neq0$. Moreover, $sv$ still has the same eigenvalue. For this reason, we usually look only for unit eigenvectors.

Suppose that a matrix A has n linearly independent eigenvectors {$v^{(1)}$, . . . , $v^{(n)}$} with corresponding eigenvalues {λ1,...,λn}. We may concatenate all the eigenvectors to form a matrix V with one eigenvector per column: V = $[v^{(1)},..., v^{(n)}]$. Likewise, we can concatenate the eigenvalues to form a vector λ = $[\lambda_1, . . . , \lambda_n]^T.$ The eigen-decomposition of A is then given by

$$
A = Vdiag(\lambda)V^{-1}
$$

> [!NOTE] Not every matrix can be decomposed into eigenvalues and eigenvectors. In some cases, the decomposition exists but involves complex rather than real numbers.

## Intuition

**A vector is a quantity which has both magnitude and direction. The general effect of matrix **A** on the vectors in **x** is a combination of rotation and stretching. 
$$
t_1 = Ax_1
$$
$$t_2 = Ax_2$$
For example, it changes both the direction and magnitude of the vector **x1** to give the transformed vector **t1**.

![[Pasted image 20240206165055.png]]


However, for vector **x2** only the magnitude changes after transformation. In fact, **x2** and **t2** have the same direction. Matrix **A** only stretches **x2** in the same direction and gives the vector **t2** which has a bigger magnitude. 

The only way to change the magnitude of a vector without changing its direction is by multiplying it with a scalar. So if we have a vector **u**, and _λ_ is a scalar quantity then _λ_**u** has the same direction and a different magnitude. So for a vector like **x2** in figure 2, the effect of multiplying by **A** is like multiplying it with a scalar quantity like _λ._.

$$
t_2 = Ax_2 = \lambda x_2
$$

This is not true for all the vectors in **x**. In fact, for each matrix **A,** only some of the vectors have this property. These special vectors are called the **eigenvectors** of **A** and their corresponding scalar quantity λ is called an eigenvalue of **A** for that eigenvector. 

So the eigenvector of an _n×n_ matrix **A** is defined as a nonzero vector **u** such that:
$$
Au = \lambda u
$$
In addition, if you have any other vectors in the form of _a_**u** where _a_ is a scalar, then by placing it in the previous equation we get:
$$
A(au) = aAu = a(\lambda u)
$$
which means that any vector which has the same direction as the eigenvector **u** (or the opposite direction if _a_ is negative) is also an eigenvector with the same corresponding eigenvalue.

## Example

![[Pasted image 20240206165601.png]]



