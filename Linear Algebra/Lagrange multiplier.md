The Lagrange multipliers" technique is a way to solve constrained optimization problems. 

- It helps you find the maximum or minimum of a multivariate function $f(x,y,...)$ when there is some constraint on the input values you are allowed to use.
- This technique only applies to constraints that look like this: $$g(x,y,..) = c$$Here, $g$ is another multivariable function with the same input space as  $f$, and $c$ is some constant.

## Core idea
- The maximum/minimum would occur at at point when the 2 curves(function curve, constraint curve) barely touch each other. 

![[Pasted image 20240213153917.png]]
![[Lagrange_multiplier.png]]
## Mathematical Form

$$
\nabla f(x^*, y^*) = -\lambda\nabla g(x^*, y^*)
$$

where $\lambda$ is the Lagrange multiplier.


## Example

Suppose we want to maximize the function $f(x, y) = x + y$ subject to the constraint $g(x, y) = x^2 + y^2 - 4 = 0$. Our objective is to find the maximum value of $f(x, y)$ while satisfying the constraint.

We can setup the Lagrangian as follows:

$$
\mathcal{L}(x, y, \lambda) = x + y - \lambda(x^2 + y^2 - 4)
$$
To find the maximum value of $f(x, y)$ subject to the constraint, we need to solve the following system of equations:

$$
\begin{cases}
\frac{\partial \mathcal{L}}{\partial x} = 1 - 2\lambda x = 0 \\
\frac{\partial \mathcal{L}}{\partial y} = 1 - 2\lambda y = 0 \\
g(x, y) = x^2 + y^2 - 4 = 0
\end{cases}
$$


From the first two equations, we can solve for $\lambda$:

$$
2\lambda x = 1 \implies \lambda = \frac{1}{2x}
$$

$$
2\lambda y = 1 \implies \lambda = \frac{1}{2y}
$$
Since both equations must hold, we have:

$$
\frac{1}{2x} = \frac{1}{2y}
$$

Solving for \(x\) and \(y\), we find that \(x = y\). Substituting this into the constraint equation:

$$
x^2 + x^2 - 4 = 2x^2 - 4 = 0
$$

Solving for $x$, we get $x = y = \pm \sqrt{2}$.

Now, we can calculate the value of \(\lambda\):

$$
\lambda = \frac{1}{2x} = \frac{1}{2\sqrt{2}}
$$

Finally, we can find the maximum value of \(f(x, y)\) by plugging these values into the objective function:

$$
f(x, y) = x + y = \sqrt{2} + \sqrt{2} = 2\sqrt{2}
$$

So, the maximum value of $f(x, y)$ subject to the constraint is $2\sqrt{2}$ when $x = y = \pm \sqrt{2}$.

![[Langrange_example.png]]


## References
1. https://www.khanacademy.org/math/multivariable-calculus/applications-of-multivariable-derivatives/constrained-optimization/a/lagrange-multipliers-single-constraint 
2. 
