It is an alternative to [[ReLU]]. It is defined as: $$\text{swish}(x) = x \cdot \text{sigmoid}(x) = \frac{x}{e^{-x}+1}$$
It is visually similar to ReLU, with the key difference being that it is smooth, which helps to alleviate vanishing gradient problem. 

![[Pasted image 20240707164545.png]]
