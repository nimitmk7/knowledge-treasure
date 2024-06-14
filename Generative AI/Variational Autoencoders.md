## The Encoder
In an auto-encoder, each data point is mapped directly to one point in the latent space. In a variational auto-encoder, each image is instead mapped to a multivariate normal distribution around a point in the latent space.

![[Pasted image 20240605213031.png]]

The encoder only needs to map each input to a mean vector and a variance vector and does not need to worry about covariance between dimensions. 
Variational autoencoders ==assume that there is no correlation between dimensions in the latent space==.

Variance values are always positive, so we actually choose to map to the logarithm of the variance, as this can take any real number in the range 
$(−\infty, \infty)$.

This way we can use a neural network as the encoder to ==perform the mapping from the input data to the mean and log variance vectors==.

### Formulation

Input data point → Encode to 2 vectors that together define a multivariate normal distribution in the latent space:
1. z_mean: Mean point of distribution
2. z_log_var: Logarithm of the variance of each dimension

We can sample a point z from the distribution defined by these values using the following equation: 

$$z = z\_mean + z\_sigma * epsilon$$
where

$z\_\text{sigma} = \exp(z\_\log\_var * 0.5)$
$epsilon \sim N(0, I)$


## The Decoder
The decoder of a variational autoencoder is identical to the decoder of a plain autoencoder. 

The full architecture diagram is shown in the figure below.
![[Pasted image 20240605213952.png]]

Now, since we are sampling a random point from an area around z_mean, the decoder must ensure that ==all points in the same neighborhood produce very similar images when decoded, so that the reconstruction loss remains small==. 

This is a very nice property that ensures that even when we choose a point in the latent space that has never been seen by the decoder, it is likely to decode to an image that is well formed.
## The Loss Function

VAE Loss = Reconstruction Loss + KL Divergence term

In a VAE, we want to measure how much our normal distribution with parameters z_mean and z_log_var differs from a standard normal distribution.

In this special case, KL divergence equals: 

$$D_{KL}[N(\mu, \sigma)\space||\space N(0,1)] = -\frac{1}{2} \sum(1+\log(\sigma^2) - \mu^2 -\sigma^2)$$

The sum is taken over all the dimensions in the latent space. kl_loss is minimized to 0 when z_mean = 0 and z_log_var = 0(as log(1) is 0) for all dimensions. As these two terms start to differ from 0, kl_loss increases.

In summary, the KL divergence term penalizes the network for encoding observa‐tions to z_mean and z_log_var variables that differ significantly from the parameters of a standard normal distribution, namely z_mean = 0 and z_log_var = 0.





