The t-distribution, also known as the Student’s t-distribution, is a type of probability distribution that is similar to the normal distribution with its bell shape but has heavier tails. 

It is used for estimating population parameters for small sample sizes or unknown variances. T-distributions have a greater chance for extreme values than normal distributions, and as a result have fatter tails.

The t-distribution is the basis for computing t-tests in statistics.

### Formula

$$
t = \frac{\bar x - \mu}{s/ \sqrt N}  
$$
where 
$\mu$ is population mean
$\bar x$ is sample mean
s is estimator for population standard deviation or sample variance
N is no of given independent measures

s is defined as 
![[Pasted image 20240130103554.png]]
Student's t-distribution is defined as the distribution of the random variable t which is (very loosely) the "best" that we can do not knowing $\sigma$ .

### Degrees of Freedom

In statistics, degrees of freedom are the number of values in a statistical calculation that are free to vary. 

The degrees of freedom in a t-distribution are N-1, where N is the sample size. This is because one degree of freedom is reserved for estimating the mean. Why ?

> [!NOTE] Consider a data sample consisting of 5 positive integers, and the value of the five integers must have an average of 6. If four items within the data set are {3,8,5 and 4}, the fifth number is bound to be 10. As we could only choose the first 4 numbers at random, the degree of freedom is 4.
>
> 


### Limitations

The t-distribution can skew exactness relative to the normal distribution. Its shortcoming only arises when there’s a need for perfect normality.

The t-distribution should only be used when the population standard deviation is not known. If the population standard deviation is known and the sample size is large enough, the normal distribution should be used for better results.







