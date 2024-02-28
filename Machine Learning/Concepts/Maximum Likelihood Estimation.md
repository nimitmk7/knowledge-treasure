## Likelihood to Maximize
- The maximum likelihood estimation framework says the following:
	- For any model, we create a likelihood function that is basically the probability of seeing the results we actually saw.
	- Then we maximize this likelihood function with respect to the unknown parameters, the **weights/co-efficients**. In other words, we choose the coefficients so that the resulting model is most in line with what we actually observed.
### Likelihood function

$$
L(w)= P(y|x;w)= \Pi_{i=1}^{n} P(y^{(i)}|x^{(i)};w)
$$
## Intuition

There are lots of different distributions for different types of data. The reason you want to fit a distribution to your data is it can be easier to work with and it is also more general - it applies to every experiment of the same type.


