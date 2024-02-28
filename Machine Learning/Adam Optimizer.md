ADAM = **Ad**aptive **M**oment Estimation  

Adam = RMSProp + Momentum

Adam stores the momentum for each parameter separately using update equations that consist of exponentially weighted moving averages for both the gradients and the squared gradients in the form.

Whereas momentum can be seen as a ball running down a slope, Adam behaves like a heavy ball with friction, which thus prefers flat minima in the error surface. 
$$
s_t = \beta_1 s_{t-1} + (1-\beta_1)(\frac{\partial E(w)}{\partial w_i})
$$

$$
r_t = \beta_2 r_{t-1} + (1-\beta_2)(\frac{\partial E(w)}{\partial w_i})^2
$$
$$
\hat s_t = \frac{s_t}{1-\beta_1}
$$
$$
\hat r_t = \frac{r_t}{1-\beta_2}
$$
$$
w_i = w_i - \eta \frac{\hat s_t}{\sqrt {\hat r_t} + \delta }
$$

- $\delta$ is a small constant, say $10^{-8}$ , that ensures numerical stability in the event that $r_i$ is close to zero. 
- Here the factors $1/(1-\hat \beta_1)$ and $1/(1-\hat \beta_2)$ correct for a bias introduced by initializing $s_t$ and $r_t$ to zero. 
- Typical values for the parameters $\beta_1$ and $\beta_2$ are 0.9 and 0.99 respectively.

## Disadvantages

1. It accumulates squared gradients(especially from the very start), which makes the associated weight updates very small, which can make training extremely slow in the late phases.
