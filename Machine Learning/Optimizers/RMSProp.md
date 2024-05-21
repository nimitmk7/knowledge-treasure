Replace the squared gradients of [[AdaGrad]] with an exponentially weighted average.
## Core idea
The core idea of exponential weighting is to apply a set of weights recursively. The magnitude of the weighting factor decreases exponentially as the data ages, and never reaches zero. The parameter $\beta$ controls the weightage of the accumulated sum relative to the current value.
## Method

$$
r_t = \beta r_{t-1} + (1-\beta)(\frac{\partial E(w)}{\partial w_i})^2
$$
$$
w_i = w_i - \frac{\eta}{\sqrt{r_t}+\delta}(\frac{\partial E(w)}{\partial w_i})
$$
- $\beta$ is the exponential smoothing parameter and $0<\beta<1$.
- Typical value is $\beta$ = 0.9

## Improvement
1. [[Adam Optimizer]]

