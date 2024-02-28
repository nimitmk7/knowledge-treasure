## What is Variance ?

The term variance refers to a statistical measurement of the spread between numbers in a data set. More specifically, variance measures how far each number in the set is from the mean (average), and thus from every other number in the set.

It is calculated as:

![[Pasted image 20240129110842.png]]

### Pros and Cons
**Pro**: Treats all deviations from the mean the same regardless of their direction. The squared deviations cannot sum to zero and give the appearance of no variability at all in the data.
**Cons**: 
	1. One drawback to variance, though, is that it gives added weight to outliers. These are the numbers far from the mean. Squaring these numbers can skew the data.
	2. Not easily interpreted. So, users often employ it primarily to take the square root of its value, which indicates the standard deviation of the data

## What is Covariance ?

Covariance is a measure of the relationship between two random variables. The metric evaluates how much – to what extent – the variables change together. In other words, it is essentially a measure of the variance between two variables.

Let us say X and Y are any two variables, whose relationship has to be calculated. Thus the covariance of these two variables is denoted by Cov(X,Y).
![[Pasted image 20240129111932.png]]

Where,

- xi = data value of x
- yi = data value of y
- x̄ = mean of x
- ȳ = mean of y
- N = number of data values.


![[Pasted image 20240129112011.png]]
Cov(X, Y) > 0 : Both the variables move in the same direction.
Cov(X, Y) = 0 : There is no relation between two variables.
Cov(X, Y) < 0:  Both the variables move in the opposite direction.


## References
1. https://www.investopedia.com/terms/v/variance.asp
2. https://byjus.com/maths/covariance 
