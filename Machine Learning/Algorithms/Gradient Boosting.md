Watch this video: 
<iframe width="560" height="315" src="https://www.youtube.com/embed/en2bmeB4QUo?si=TkUcZvx1waVOsuvt" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>  

## Formulation

- L → loss function
- $\alpha$ → step size
- $(x_i, y_i)$ → training data

1. **Initialize model with a constant value**: The y value of the sample point with minimum loss value
$$H_0(x) = argmin \sum^n_{i=1}L(y_i,H(x_i))$$

2. **Begin ensemble process** for m = 1 to M (where, M = no. of trees we would like to have)
3. **Compute Gradient / Pseudo-Residual**: Take the Partial Derivative of L w.r.t $H_m(x_i)$, where m is the iteration step.
$$r_{i,m} = − \frac{\partial L((y_i),H_m(x_i))}{\partial H_{m}(x_i)} \text{for i = 1 to n}$$
4. **Build a new tree on the gradient** $r_{i,m}$ and compute the terminal regions. The key here is that we don’t build the tree on $y_i$ rather we fit the residual

$$R_{j,m} \space for \space j = 1...J_m \space \text{where}, j = \text{the terminal nodes}$$

5. **Compute Gamma**. $$\gamma_{j,m} = argmin\sum L(y_i,H_m−1(x_i) + \gamma H_{m}(x_{i}))$$
6. **Make a new prediction**.
$$ \text{Update} \space H_{m} = H_{m-1}(x_{i}) + \alpha \sum_{j=1}^{J_{m}} \gamma_{j,m} H_{m}(x_{i})$$
7. **Repeat Steps 2-6** for M iterations to generate the new $H_M(x_{i})$

## Worked Example



## Why it works ?

New Model = Previous Model + Step in the direction of decreasing Loss

