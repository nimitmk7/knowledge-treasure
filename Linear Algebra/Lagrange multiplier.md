The "Lagrange multipliers" technique is a way to solve constrained optimization problems. 

- It helps you find the maximum or minimum of a multivariate function $f(x,y,...)$ when there is some constraint on the input values you are allowed to use.
- This technique only applies to constraints that look like this: $$g(x,y,..) = c$$Here,  $g$ is another multivariable function with the same input space as  $f$, and $c$ is some constant.

## Core idea
- The maximum/minimum would occur at at point when the 2 curves(function curve, constraint curve) barely touch each other. 

![[Pasted image 20240213153917.png]]

## Mathematical Form

$$
\nabla f(x^*, y^*) = \lambda\nabla g(x^*, y^*)
$$

where $\lambda$ is the Lagrange multiplier.









## References
1. https://www.khanacademy.org/math/multivariable-calculus/applications-of-multivariable-derivatives/constrained-optimization/a/lagrange-multipliers-single-constraint 
2. 
