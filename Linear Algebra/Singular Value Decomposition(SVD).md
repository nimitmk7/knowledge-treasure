## Definition

In linear algebra, the singular value decomposition (SVD) is a factorization of a real or complex matrix into a rotation, followed by a rescaling followed by another rotation.

SVD of $m$ x $n$ complex matrix A is a factorization of the form
$$ A = UDV^T $$
U, V are both [orthogonal matrices](https://en.wikipedia.org/wiki/Orthogonal_matrix). D is a [diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix), not necessarily square though.

Elements along the diagonal of D are the singular values of matrix M.
Columns of U are the left-singular vectors.
Columns of V are the right-singular vectors.

$U, V^T$are rotations and $D$ is stretching.

## SVD and [[Eigen-value decomposition]]

We can actually interpret the singular value decomposition of $A$ in terms of the eigen-value decomposition of functions of $A$.

![[Pasted image 20240206163439.png]]

The left-singular vectors of A(a.k.a $U$) are the eigenvectors of$AA^T$.
The right-singular vectors of A(a.k.a $V$) are the eigenvectors of $A^TA$. 
The non-zero singular values of A are the square roots of the eigenvalues of $AA^T$ .The same is true for $A^TA$.

## Example

![[Pasted image 20240206163929.png]]

## Intuition


## References

1. https://youtu.be/mBcLRGuAFUk?si=PiqFn2RSNjNBy2kJ
2. https://www.deeplearningbook.org/ Chapter 2, 2.8
3. Wikipedia
4. https://towardsdatascience.com/understanding-singular-value-decomposition-and-its-application-in-data-science-388a54be95d

