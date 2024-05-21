Watch this video: 
<iframe width="560" height="315" src="https://www.youtube.com/embed/en2bmeB4QUo?si=TkUcZvx1waVOsuvt" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>  

## Formulation

- L → loss function
- $\alpha$ → step size
- $(x_i, y_i)$ → training data

1. **Initialize model with a constant value**: The y value of the sample point with minimum loss value
$$H_0(x) = \min \sum^n_{i=1}L(y_i,H(x_i))$$
2. **Begin ensemble process** for m = 1 to M (where, M = no. of trees we would like to have)
3. **Compute Gradient / Pseudo-Residual**: Take the Partial Derivative of L w.r.t $H_m(x_i)$, where m is the iteration step.
$$r_{i,m} = − \frac{\partial L((y_i),H_m(x_i))}{\partial H_{m}(x_i)} \text{for i = 1 to n}$$
4. **Build a new tree on the gradient** $r_{i,m}$ and compute the terminal regions. The key here is that we don’t build the tree on $y_i$ rather we fit the residual
$$R_{j,m} \space for \space j = 1...J_m \space \text{where}, j = \text{the terminal nodes}$$
5. **Compute Gamma**. $$\gamma_{j,m} = \min_{\gamma}\sum_{i=1}^n L(y_i,H_{m−1}(x_i) + \gamma))$$
6. **Make a new prediction**.
$$ \text{Update} \space H_{m}(x_{i}) = H_{m-1}(x_{i}) + \alpha \sum_{j=1}^{J_{m}} \gamma_{j,m} H_{m}(x_{i})$$
7. **Repeat Steps 2-6** for M iterations to generate the new $H_M(x_{i})$

## Worked Example
We are trying to predict income given city, age and gender of a person. 

| City         | Age | Gender | Income(in $K) |
| ------------ | --- | ------ | ------------- |
| NYC          | 35  | M      | 90            |
| Philadelphia | 36  | F      | 75            |
| Boston       | 40  | F      | 60            |

1. Loss function: $$l(H(x_{i}, y_{i})) = \frac{1}{2} (y_{i} - H(x_{i}))^2 $$
2. Derivative of the Loss Function $(l(H))$ w.r.t $H$:  $$\frac{\partial L}{\partial H(x_{i})} = -\frac{2}{n}(y_{i} - H(x_{i}))$$
3. Getting the initial prediction for our dataset by predicting the value that minimizes the loss(which is the mean): $$
\begin{align} \\
\frac{\partial L}{\partial H(x_{i})}= 0  \\
-\frac{2}{3}[(90-y) + (75-y) + (60-y)]= 0 \\
y =75
\end{align}$$
4. The pseudo-residuals are calculated as follows: $$r_{i,m}= - \left[\frac{\partial{L(y, H(x_{i}))}}{\partial H(x_{i})} \right]_{F(x) = F_{m-1}(x)} \text{for} \space i = 1,2,\dots,n$$ $$r_{1,1} = 15, \space r_{2,1} = 0,\space r_{3,1} = -15$$
5. Now we create a tree for the above residuals. Let’s split the tree on the ’age’ feature. We use age 40 as the splitting criteria. Hence, we have 2 samples of in one leaf and 1 samples in the other leaf.  ![[Grad_boost_iter_1.png]]
	
6. Compute Gammas for each terminal node.
	$$\gamma_{(i,m)} = \min_{\gamma} \sum L(y_{i}, H_{m-1}(x_{i}) + \gamma)
	$$
	For terminal node 1,
	$$
	\begin{align}
	\frac{\partial L}{\partial \gamma} = 0  \\
\sum_{i=1}^K (y_{i} - (H_{0} + \gamma)) = 0  \\
(90-(75+\gamma)) + (75-(75+\gamma)) = 0 \\
\gamma_{1,1} = 7.5 \\
	\end{align}
	$$
	For terminal node 2, 
	$$
	\begin{align}
	\frac{\partial L}{\partial \gamma} = 0  \\
    \sum_{i=1}^K (y_{i} - (H_{0} + \gamma)) = 0  \\
    (60-(75+\gamma)) = 0 \\
     \gamma_{3,1} = -15 \\
	 \end{align}
	$$
7. Now given the learning rate, $\alpha= 0.1$, we calculate the new predictions:
	NYC → 75 + 0.1(7.5) = 75.75
	Philadelphia → 75 + 0.1(7.5) = 75.75
	Boston → 75 + 0.1(-15) = 73.5

We can see that the new prediction for the first sample has increased closer to the actual value of 90 while the prediction for the last data point has gone down closer to the actual value of 60.

By repeating the iterations we will get closer to the target values.

## Why it works ?

New Model = Previous Model + Step in the direction of decreasing Loss



