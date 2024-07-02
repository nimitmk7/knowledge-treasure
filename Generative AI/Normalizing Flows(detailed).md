Normalizing flows is a technique used in Machine learning to build complex probability distributions by by transforming simple probability distributions. 

In particular, they have been applied a lot in the context of generative modeling, as they allow us to construct models with some useful properties.

## Mathematical Framework

### Premise
Suppose we have a continuous random variable $z$ with some simple probability distribution like the spherical Gaussian. $$ z \sim p_{\theta}(z) = \mathscr N(z;0,I)$$
> The choice of spherical Gaussian is done because it allows for easy sampling and density evaluation.

The key idea is to transform this simple distribution with some function $f$ into a more complicated distribution. And we’ll formulate $f$ as a sequence of invertible transformations, so the overall transformation is also invertible for any $x$.
$$ x = f_{\theta}(z) = f_{K} \circ \dots \circ f_{2} \circ f_{1}(z)$$
Here, each $f_i$ is invertible(bijective). So for any $x$, we have exactly one $z$ that could’ve been sampled to produce it. 
![[Pasted image 20240629200333.png]]

Now, say we ask what the density is of a given $x$ value ? $$p_{\theta}(x) = \space?$$
Well, $$p_{\theta}(x) \neq p_{\theta}(f_{\theta}^{-1}(x))$$
But rather, it depends on the behavior of $f$. To understand this, we need to look at the change of variables formula.\
### Change of Variables Formula

#### Formula
$$p_{\theta}(x) = p_{\theta}(f^{-1}(x)) \left| \det\left(\frac{\partial f^{-1}(x)}{\partial x}\right) \right|$$

where $f: Z → X, f \space \text{is invertible}$,

The second term is magnitude of the determinant of the [[Jacobian]] of $f^{-1}$. 
#### Meaning

$$p_{\theta}(x) = p_{\theta}(z)\left| \det \left( \frac {\partial z} {\partial x} \right) \right|$$
Formula breakdown:
1. We are mapping $x$ to its inverse $z$.
2. We are evaluating the probability density of $z$.
3. Then we are multiplying it by some scalar magnitude

A valid probability density function must integrate to 1 over its domain.  

The magnitude of the Jacobian determinant which basically indicates how much a transformation locally expands or contracts space is necessary to ensure that $p(x)$ satisfies this requirement.

#### Example
- $z$, $x$ are 2-dimensional.
- ![[Pasted image 20240629222217.png | 300]]
- $$J = \begin{bmatrix}
 \frac{\partial x_{1}}{\partial z_{1}} & \frac{\partial x_{1}}{\partial z_{2}} \\ \frac{\partial x_{2}}{\partial z_{1}} & \frac{\partial x_{2}}{\partial z_{2}}
\end{bmatrix}$$
- $$J = \begin{bmatrix}
 2 & 0 \\ 0 & 2
\end{bmatrix}$$
- Locally, this transformation is stretching both dimensions by a factor of 2. ![[Pasted image 20240629222829.png | 300]]
- So likewise according to the determinant, the area should increase by a factor of 4. $\det(J) =2 \cdot 2 - 0 \cdot 0 = 4$
- The change of variables formula actually contains the Jacobian $\frac{\partial z}{\partial x}$ which is the reciprocal of the original Jacobian $\frac{\partial x}{\partial z}$.
- We know that total probability of both regions(one in X, and other in Z) should be equal to 1.0
- So the change of variables formula is saying that, in order for that to be true, each point in region of $X$ has to have 1/4th as much density as $z$. 

- We don’t actually care if the Jacobian preserves orientation or not. If the original Jacobian has $-2$ as its fourth element, the determinant is $-4$, but we only carry about the magnitude of the determinant.

### Normalizing Flow
We call this series of bijective transformations as a **normalizing flow**. 

After $z$ is initially sampled, it flows through this sequence of transformations with its density function being renormalized at each step to ensure it remains valid. 

![[Pasted image 20240629223946.png]]
## Generative Model Likelihood
### Uses of Generative Modeling
Generative modelling is all about learning to approximate some observed data distribution(natural images, speech, audio, text). 
The generative models are then used to:
1. Sample new data points with similar characteristics to the training data.
	1. ![[Pasted image 20240630104041.png | 400]]
2. Conditionally complete a sample.
	1. ![[Pasted image 20240630103723.png]]
3. Evaluate probabilities or densities of samples.
	1. ![[Pasted image 20240630103820.png]]
4. Infer latent variables that allow us to reduce the dimensionality of the data.
5. Expose interpretable structure that enables generating samples with specific characteristics.

### Using Normalizing flows in Generative Modeling

One way to use normalizing flows in generative modeling is to ==parameterize the likelihood as a normalizing flow==. So $z$ will be the latent variable, and $x$ will be the observed variable.

$$p_{\theta}(x) = p_{\theta}(z) \left| \det\left(\frac{\partial f^{-1}(x)}{\partial x}\right) \right|$$
Taking a log on both sides, we get:
$$\log p_{\theta}(x) = \log p_{\theta}(z) + \log\left| \det\left(\frac{\partial f^{-1}(x)}{\partial x}\right) \right|$$

Now, we know that the overall transformation $f$ was specifically constructed to be the composition of a sequence of functions. So, we can transform the log determinant term in the above equation to:

$$\log p_{\theta}(x) = \log p_{\theta}(z) + \sum_{i=1}^K\log\left| \det\left(\frac{\partial f_{i}^{-1}}{\partial z_{i}}\right) \right|$$

## Comparison with VAE and GAN

### VAE
- In VAE, we can only approximate a lower bound on the log likelihood(ELBO) and this is what we optimize during training.
- We have encoder network parameterizing an approximate posterior($q_{\phi}(z|x)$) as computing the exact posterior is intractable.
### GAN
- In GAN, we avoid log-likelihood evaluation altogether, and instead optimize a min-max objective involving the discriminator and generator.
- There is no encoder for inferring the latents and the generator is not guaranteed to have full support over the data.
### NF
- In NF, the setup allows us to directly train out model via maximum likelihood, as we can calculate the log marginal likelihood exactly.
- The flow-based framework allows for exact posterior inference(via $z = f^{-1}(x)$). The invertibility of $f$ guarantees that we can find the unique $z$ that generates a given $x$. 

## NICE Architecture

Now, we have the sequence of potentially high dimensional bijective functions whose Jacobian determinant we need to be able to compute. This can be an expensive computation.

So the basic trick often used is to ensure that the Jacobian is Triangular. Because the determinant of a triangular matrix is just the product of diagonal elements. So we don’t have to take into account any of the off-diagonal elements.
![[Pasted image 20240630110946.png]]

We ensure that the Jacobian of each transformation is triangular by the use of coupling layers. 

### Coupling Layer

1. Let $z \in \mathbb{R}^D$ be the input to a coupling layer. 
2. We partition $z$ into 2 subsets → $z_{1:d}$ and $z_{d+1:D}$
3. For the first subset, we simply apply the identity function, copying over the elements to the same positions in the layer’s output $x$. $$x_{1:d} = z_{1:d}$$
4. For the second subset, we use 2 functions $m$ and $g$. $$x_{d+1:D} = g(z_{d+1:D}; m(z_{{1:d}}))$$
5. So, deriving the Jacobian: $$\frac{\partial x}{\partial z} = 
\begin{bmatrix} \\
 \mathbf{I_{d}} & 0\\
 \frac{\partial x_{d+1:D}}{\partial z_{1:d}} & \frac{\partial x_{d+1:D}}{\partial z_{d+1:D}}\\
\end{bmatrix}$$
6. Now we need to make sure that the lower right block is lower triangular, to ensure that the full Jacobian is triangular. 
7. The lower left block, however will have no restrictions.
8. As the elements in the 1st subset are left unchanged by the coupling layer, we’ll also need to alternate roles between the two subsets as we compose multiple coupling layers together for inverting this transformation.
9. For inverting this transformation, 
	1. We can invert the identity function easily. $$z_{1:d} = x_{1:d}$$
	2. $g$ is required to be invertible with respect to its first argument given the second, since we can trivially recover its second argument. $$z_{d+1:D} = g^{-1}(x_{d+1:D}; m(x_{1:d}))$$
	3. The simplest $g$ can be just an addition(an additive coupling layer). $$g(z_{d+1:D}; m(z_{{1:d}})) = z_{d+1:D} + m(z_{1:d})$$

### Scaling Matrix
1. The additive coupling layers have a unit Jacobian determinant, so they neither contract nor expand space. Even a composition of several of these preserves volume.
2. But we actually want the model to perform expansions and contractions at different points in space to allow for more flexibility to match the data distributions. 
3. As we start with a simple Gaussian, we won’t be able to model multi-modal distributions without this capability. 
	1. The model should be free to increase the amount of latent space used to represent high density of the data.
	2. This allows the learner to give more weight (i.e. model more variation) on some dimensions and less in others.
4. So, as we decode from latent space to data space, the first transformation before the coupling layers will be a diagonal scaling matrix. ![[Pasted image 20240630132423.png | 300]]
5. The smaller the scaling factor, the less important the latent dimension is. In the limit as an element of $S$($S_{i,i}$) approaches 0, the corresponding latent dimension is effectively removed. 

The overall objective now consists of: 
$$\log(p_{\theta}(x)) = \sum_{i=1}^D[\log(p_{\theta}(f^{-1}(x)_{i}))-\log(|S_{ii}|)]$$















## References

<iframe width="560" height="315" src="https://www.youtube.com/embed/i7LjDvsLWCg?si=eJaG-OsCCRvRdNvx" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


