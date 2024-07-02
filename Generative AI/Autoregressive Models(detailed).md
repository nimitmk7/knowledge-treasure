## Neural Models

$$p(x_{1}, x_{2}, x_{3}, x_{4}) = p(x_{1})p(x_{2}|x_{1})p_{neural}(x_{3}|x_{1},x_{2})p_{neural}(x_{4}|x_{1}, x_{2}, x_{3})$$
- The model assumes a specific functional form for the conditionals.
- It works to the extent that the neural network is sufficiently powerful that it can well approximate these conditional probabilities, which could be potentially very complicated.
	- In principle, a sufficiently deep neural network can approximate any function.

## Motivating Example: MNIST

- Given a dataset $D$ of handwritten digit images(binarized MNIST)
	- ![[Pasted image 20240627084252.png]]
- Each image has n = 28 * 28 = 784 pixels. Each pixel can either be black(0) or white(1).
- **Goal**: Learn a probability distribution $p(x) = p(x_1, x_{2}, \cdots, x_{784})$ over $x \in \{0,1 \}$ such that when $x \sim p(x)$, $x$ looks like a digit.
- **Challenge**: There are lot of different possible images, we need to be able to assign a probability to each one of them.
- Two step process:
	1. Parameterize a model family $\{ p_{\theta}(x), \theta \in \Theta \}$
	2. Search for model parameters $\theta$ based on training data $D$.

## Autoregressive Models

- We can pick an ordering of all the random variables i.e. raster scan ordering of pixels from top-left($X_{{1}}$) to bottom-right($X_{n=784}$)
- Without loss of generality, we can use chain rule for factorization, $$p(x_{1}, \cdots, x_{784}) = p(x_{1})p(x_{2}|x_{1})p(x_{3}|x_{1}, x_{2})\cdots p(x_{n}|x_{1}, \cdots, x_{n-1}) $$
- Some conditionals are too complex to be stored in tabular form. Instead, we **assume** $$p(x_1, · · · , x_{784}) = p_{CPT}(x_1; α^1)p_{logit}(x_2 | x_1; \alpha^2)p_{logit}(x_3 | x_1, x_2; α^3 )· · · p_{logit}(x_n | x_1, · · · , x_{n−1}; α^n )$$
- More explicitly, 
	- $p_{CPT}(X_{1};\alpha^{1}) = \alpha^1, p_{CPT}(X_{0};\alpha^{1}) = 1-\alpha^1$
	- $p_{logit}(X_{2}=1\space|\space x_{1};\alpha^{2})=\sigma(\alpha_{0}^2 \space+\space \alpha_{1}^2x_{1})$
	- $p_{logit}({X_3}=1\space|\space x_{1}, x_{2};\alpha^{3})=\sigma(\alpha_{0}^3 \space+\space \alpha_{1}^3x_{1} + \alpha_{2}^{3}x_{2})$

> [!WARNING] 
> Here power of $\alpha$ notation means new $\alpha$ class of variables.
 
- Note: This is a **modeling assumption**. We are using parameterized functions (e.g., logistic regression above) to predict next pixel given all the previous ones. Called **autoregressive** model.

## Fully Visible Sigmoid Belief Network(FVSBN)

![[Pasted image 20240627171406.png]]
- The conditional variables $X_i | x_1, \cdots, X_{i-1}$ are Bernoulli with parameters.$$\hat x_i = p(X_{i} = 1|x_1, · · · , x_{i−1}; \alpha^i) = p(X_{i} = 1|x_{<i};\alpha^i) = \sigma\left( \alpha_{0}^i + \sum_{j=1}^{i-1}\alpha_{j}^ix_{j} \right)$$
- How to evaluate $p(x_{1}, \cdots, x_{784})$? Multiply all the conditionals(factors).
	- In the above example: $$\begin{align}
p(X_1 = 0, X_2 = 1, X_3 = 1, X_4 = 0) = (1 − \hat x_1) × \hat x_2 × \hat x_3 × (1 − \hat x_4) \\ = (1 − \hat x_1) \space × \space \hat x_2(X_{1} =0) \space × \space \hat x_{3}(X_{1}=0, X_{2}=1) \space×\space(1-\hat x_{4}(X_{1}=0, X_{2}=1, X_{3}=1))\end{align}$$
- How to sample from $p(x_1, \cdots, x_{784})$?
	1. Sample $\bar x_1 \sim p(x_1)$ (np.random.choice($[1,0]$, $p = [\hat x_1, 1-\hat x_{1}]$))
	2. Sample $\bar x_2 \sim p(x_{2}|x_{1} = \bar x_{1})$
	3. Sample $\bar x_3 \sim p(x_{3}|x_{1} = \bar x_{1}, x_{2} = \bar x_{2})$
- How many parameters (in the $\alpha_{i}$ vectors)?
	- $1 + 2 + 3 + \cdots + n \approx n^2/2$

## NADE: Neural Autoregressive Density Estimation

![[Pasted image 20240627173552.png]]
- To improve model: use one layer neural network instead of logistic regression. It is a slightly more flexible model.
	- $h_i = \sigma(A_ix_{<i} + c_i)$
	- $\hat x_i = p(x_i |x_1, · · · , x_{i−1}; \underbrace {A_i , c_i , α_i , b_i}_{\text{parameters}}) = \sigma(\alpha_ih_i + b_i)$
- For example, 
	- $h_{2} = \sigma \left( \underbrace{\left( {\vdots} \right)}_{A_{2}}x_{1} + \underbrace{\left( {\vdots} \right)}_{c_{2}}\right)$
	- $h_{3} = \sigma \left( \underbrace{\left( {\vdots} \right)}_{A_{3}}x_{1} + \underbrace{\left( {\vdots \space\vdots} \right)}_{c_{3}}\right)$
- Tie weights to *reduce* the number of parameters and *speed up* the computation(see blue dots in figure):
	- $h_i = \sigma(W_{.,<i},x_{<i} + c_i)$
	- $\hat x_i = p(x_i |x_1, · · · , x_{i−1}) = \sigma(\alpha_ih_i + b_i)$
- For example, ![[Pasted image 20240627185031.png]]
- If $h_{i} \in R^d$ , how many total parameters? Linear in $n$: weights $W \in R^{d×n}$ , biases $c \in R^d$ , and n logistic regression coefficient vectors $α_i , b_i \in R^{d+1}$ . Probability is evaluated in $O(nd)$.


 