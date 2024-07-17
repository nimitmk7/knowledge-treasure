A portfolio consists of assets $a_1, …, a_{n}$ in a given proportion. Formally a portfolio is an ordered n-tuple of real numbers: $$\Theta = (\theta_{1}, \theta_{2}, \dots, \theta_{n})$$
where $theta_i$ is the number of units of an asset $a_i$. 

## Weights

The weight $w_i$ of an asset $a_i$ is the percentage of the value of the asset contained in the portfolio at time $t=0$:

$$w_{i} = \frac{\theta_{i} V_{i,0}}{\sum_{j=1}^n \theta_{j} V_{j, 0}}$$
where $v_i$ is the value of $a_i$, $n$ is the number of assets, index 0 corresponds to time 0.

The sum of weights will always be 1.
$$w_{1} + \dots + w_{n} = 1$$
## Portfolio Return 

The expected return of a portfolio is the weighted average of the portfolio asset returns. $$\mu = \varepsilon(\sum_{i=1}^n w_{i} R_{i}) = \sum_{i=1}^n w_{i}\mu_{i}$$


