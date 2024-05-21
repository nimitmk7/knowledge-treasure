AdaGrad stands for **Ada**ptive **Grad**ient.

## Motivation
Optimal learning rate depends on the local curvature of the error surface, and moreover that this curvature can vary according to the direction in parameter space. Thus we need algorithms that use different learning rates for each parameter in the network, which will be adjusted automatically during training.

## Key Idea

- Reduce each learning rate parameter over time by using the accumulated sum of squares of all the derivatives calculated for that parameter. 
- Thus, parameters associated with high curvature are reduced most rapidly. 

> [!NOTE] Curvature is the rate of change of direction of a curve with respect to distance along the curve. It is a measure of how much a curve's direction changes over a small distance traveled

## Method 

$$
r_t = r_{t-1} + (\frac{\partial E(w)}{\partial w_i})^2
$$
$$
w_i = w_i - \frac{\eta}{\sqrt{r_t}+\delta}(\frac{\partial E(w)}{\partial w_i})
$$

where 
δ is a small constant, say $10^{−8}$, that ensures numerical stability in the event that r is close to zero. The algorithm is initialized with r = 0.

## Disadvantages

1.  It accumulates the squared gradients from the very start of training, and so the associated weight updates can become very small, which can slow down training too much in the later phases.



## Improvements
[[RMSProp]]
[[Adam Optimizer]]

