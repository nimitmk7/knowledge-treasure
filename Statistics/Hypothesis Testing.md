It is an act in statistics whereby an analyst tests an assumption regarding a population parameter. He uses sample data to assess the plausibility of an assumption. 

The null hypothesis($H_0$) is usually a hypothesis of equality between population parameters.
The alternative hypothesis($H_1$) is effectively the opposite of a null hypothesis.

Thus, they are mutually exclusive, and only one can be true. However, one of the two hypotheses will always be true.

## Steps
1. Formulate the hypothesis to be tested.
2. Determine the appropriate test statistic and calculate it using the sample data.
3. Comparison of test statistic to critical region to draw initial conclusions.
4. Calculation of p-value
5. Analyze the results and either reject the null hypothesis, or fail to reject it.

## Details

$H_0$: Statement being tested. 
$H_1$: Statement we will adopt if evidence suggests we reject the $H_0$

### Forms of $H_1$:
1. $\mu\neq\mu_0$ ($H_0$ : $\mu=\mu_0$)
2. $\mu>\mu_0$($H_0$: $\mu\leq\mu_0$)
3. $\mu<\mu_0$($H_0$: $\mu\geq\mu_0$)

### Statistic to evaluate
$$
Z = \frac{\bar x - \mu_0}{s/\sqrt{n}}
$$
where 
$\bar x$ is the sample mean,
$s$ is the sample standard deviation
$n$ is the sample size

Compare the value to the critical region based on the confidence interval for the case, and accordingly act on the hypothesis.

### Distribution to use ?

![[Pasted image 20240205084419.png]]




> [!INFO] 
> 1. The equality sign is always with the null hypothesis.
> 2. The alternative hypothesis is the claim for which we are seeking statistical proof.

### Using the p-value:
- The p-value is the probability of obtaining test results at least as extreme as the result actually observed, under the assumption that the null hypothesis is correct. 
- A very small _p_-value means that such an extreme observed outcome would be very unlikely under the null hypothesis.

$p-value < \alpha$: Reject $H_0$
$p-value \geq \alpha$: Reject $H_1$

where $\alpha$ is significance level(0.01 for 99% CI, 0.05 for 95% CI)

#### Intuition
Given that we assume $H_0$ to be correct, if the probability of getting our observed results is very low(insignificant to our benchmark), it means that the observed results don't align/comply with the hypotheses. It implies that our assumptions are false, and we reject the $H_0$.

## Benefits
1. It helps assess the accuracy of new ideas or theories by testing them against data. This allows researchers to determine whether the evidence supports their hypothesis, helping to avoid false claims and conclusions
2. Hypothesis testing also provides a framework for decision-making based on data rather than personal opinions or biases.
3. By relying on statistical analysis, hypothesis testing helps to reduce the effects of chance and confounding variables, providing a robust framework for making informed conclusions.

## References
1. https://www.investopedia.com/terms/h/hypothesistesting.asp
2. 



