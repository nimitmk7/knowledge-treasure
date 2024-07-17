## Analogy: DiffuseTV
You are standing in an electronics store that sells television sets. However, this store is clearly very different from ones you have visited in the past. Instead of a wide variety of different brands, there are hundreds of identical copies of the same TV connected together in sequence, stretching into the back of the shop as far as you can see. What’s more, the first few TV sets appear to be showing nothing but random static noise.

The shopkeeper comes over to ask if you need assistance. Confused, you ask her about the odd setup. She explains that this is the new DiffuseTV model that is set to revolutionize the entertainment industry and immediately starts telling you how it works, while walking deeper into the shop, alongside the line of TVs.

She explains that during the manufacturing process, the DiffuseTV is exposed to thousands of images of previous TV shows—but each of those images has been gradually corrupted with random static, until it is indistinguishable from pure random noise. The TVs are then designed to undo the random noise, in small steps, essentially trying to predict what the images looked like before the noise was added. You can see that as you walk further into the shop the images on each television set are indeed slightly clearer than the last.

You eventually reach the end of the long line of televisions, where you can see a perfect picture on the last set. While this is certainly clever technology, you are curious to understand how this is useful to the viewer. The shopkeeper continues with her explanation.

Instead of choosing a channel to watch, the viewer chooses a random initial configuration of static. Every configuration will lead to a different output image, and in some models can even be guided by a text prompt that you choose to input. Unlike a normal TV, with a limited range of channels to watch, the DiffuseTV gives the viewer unlimited choice and freedom to generate whatever they would like to appear on the screen!

You purchase a DiffuseTV right away and are relieved to hear that the long line of TVs in the shop is for demonstration purposes only, so you won’t have to also buy a warehouse to store your new device!

## Denoising Diffusion Models
**Core Idea**: We train a deep learning model to de-noise an image over a series of very small steps. If we start from pure random noise, in theory we should be able to keep applying the model until we obtain an image that looks as if it were drawn from the training set.

### Forward Diffusion Process
Suppose we have an image $\mathbf x_0$ that we want to corrupt gradually over a large number of steps(say, $T = 1000$), so that it is indistinguishable from standard Gaussian noise.($\mathbf{x}_{T}$ should have zero mean and unit variance). 

We can define a function $q$ that adds a small amount of Gaussian noise with variance $\beta_t$ to an image $\mathbf x_{t-1}$ to generate a new image $\mathbf{x}_t$. If we keep applying this function, we will generate a sequence of progressively noisier images ($\mathbf x_{0}, \dots, \mathbf x_{T}$) as shown in the below figure.

![[Pasted image 20240708193625.png]]
We can write this update process mathematically as follows: $$\mathbf{ x}_{t} = \sqrt{ 1-\beta_{t} }\mathbf{x}_{t-1} + \sqrt{\beta_{t}} \epsilon_{t-1} $$
where $\epsilon_{t-1}$ is a standard Gaussian with zero mean and unit variance. 

Note that we also scale the input image $\mathbf x_{t-1}$ , to ensure that the variance of the output image $\mathbf{x_t}$ remains constant over time. This way, if we normalize our original image $\mathbf x_0$ to have zero mean and unit variance, then $\mathbf{x}_T$ will approximate a standard Gaussian distribution for large enough $T$, by induction as follows.

1. If we assume that $\mathbf{x}_{t-1}$ has zero mean and unit variance, then $\sqrt{1-\beta_{t}}x_{t-1}$ will have variance $1-\beta_t$ and $\sqrt{\beta_{t}}\epsilon_{t-1}$ will have unit variance $\beta_{t}$ , using the rule that $Var(aX) = a^2Var(X)$.
2. Adding these together, we obtain a new distribution$\mathbf{x}_{t}$ with zero mean and variance $1 - \beta_{t} + \beta_{t} = 1$, using the rule $Var(X+Y) = Var(X) + Var(Y)$ for independent X and Y.
3.  Therefore if $\mathbf x_0$ is normalized to a zero mean and unit variance, then we guarantee that this is also true for all $\mathbf x_{t}$, including the final image $\mathbf x_T$ , which will approximate to a standard Gaussian Distribution.

This is exactly what we need: to be able to easily sample $\mathbf x_T$ and then apply a reverse diffusion process through our trained neural network model!

In other words, our forward noising process $q$ can also be written as follows: 
$$q(\mathbf{x_{t}} | \mathbf{x}_{t-1}) = \mathscr N(\mathbf{x}_{t};\sqrt{ 1-\beta_{t} }\mathbf{x}_{t-1}, \beta_{t}\mathbf I)$$

### The Reparameterization Trick
It would also be useful to be able to jump straight from an image $\mathbf x_0$ to any noised version of the image $\mathbf {x}_t$ without having to go through $t$ applications of $q$. 

If we define $\alpha_{t} = 1 - \beta_{t}$ and $\bar{\alpha_{t}} = \prod_{i=1}^t \alpha_{i}$, then we can write the following: 
$$\mathbf{x}_{t} = \sqrt{\bar\alpha_{t}}\mathbf x_{0} + \sqrt{ 1-\bar{\alpha_{t}}}\epsilon$$

We have a way to jump from the original image $\mathbf x_0$ to any step of the forward diffusion process $\mathbf x_t$. Moreover, we can define the diffusion schedule using the $\bar\alpha_{t}$ values, instead of the $\beta_{t}$ values, with the interpretation that:
1. $\alpha_{t}$ is the variance due to the signal(the original image, $\mathbf{x}_{0}$)
2. $1-\alpha_t$ is the variance due to the noise($\epsilon$)

The forward diffusion process can also be written as: 
$$q(\mathbf{x_{t}} | \mathbf{x}_{0}) = \mathscr N(\mathbf{x}_{t};\sqrt{\alpha_{t} }\mathbf{x}_{0}, (1-\alpha_{t})\mathbf I)$$
## Diffusion Schedules

We are also free to choose a different $\beta_{t}$ at each time step—they don’t all have be the same. How the $\beta_t$ (or $\alpha_{t}$) values change with $t$ is called the diffusion schedule.

The three commonly used simple types are: 
1. Linear
2. Cosine
3. Offset Cosine

The noise level ramps up more slowly in the cosine diffusion schedule. A cosine diffusion schedule adds noise to the image more gradually than a linear diffusion schedule, which improves training efficiency and generation quality.

![[Pasted image 20240710102820.png]]

## The Reverse Diffusion Process
We are looking to build a neural network $p_{\theta}(\mathbf x_{t-1} | \mathbf{x}_{t})$, that can undo the noising process–that is approximate the reverse distribution $q(\mathbf x_{t-1} | \mathbf{x}_{t})$. 

If we can do this, we can sample random noise from $\mathscr N(0, \mathbf I)$ and then apply the reverse diffusion process multiple times in order to generate a novel image. 

![[Pasted image 20240710103248.png]]

1. We sample an image $\mathbf{x}_{0}$ and transform it by $t$ noising steps to get the image $\mathbf{x}_{t} = \sqrt{\bar\alpha_{t}}\mathbf x_{0} + \sqrt{ 1-\bar{\alpha_{t}}}\epsilon$ . 
2. We give this new image and the noising rate $\bar{\alpha}_{t}$ to a neural network and ask it to predict $\epsilon$, taking a gradient step against the squared error between the prediction $\epsilon_{\theta}(\mathbf x_{t})$ and the true $\epsilon$.

Diffusion model actually contains 2 copies of the network. 
1. Actively trained using gradient descent
2. Exponential Moving Average(EMA) of the weights of the actively trained network over previous training steps

==The EMA network is not as susceptible to short-term fluctuations and spikes in the training process, making it more robust for generation than the actively trained network==. We therefore use the EMA network whenever we want to produce generated output from the network.

![[Pasted image 20240710104206.png]]

## The U-Net Denoising Model

![[Pasted image 20240710162443.png]]

 In a similar manner to a variational autoencoder, a U-Net consists of two halves: 
 1. **The downsampling half**, where input images are compressed spatially but expanded channel-wise.
 2. **The upsampling half**, where representations are expanded spatially while the number of channels is reduced.

There are also **skip connections** between equivalent spatially shaped layers in the upsampling and downsampling parts of the network. 

> [!faq] Difference between U-net and VAE(Variational Autoencoder)
> A VAE is sequential: data flows through the network from input to output, one layer after another.  
> 
> A U-net is different, because the skip connections allow information to shortcut parts of the network and flow to later layers. 

### Sinusoidal Embedding
**Core Idea**: We want to be able to ==convert a scalar value(the noise variance) into a distinct higher-dimensional vector== that is able to provide a more complex representation, for use downstream in the network. 

An example, where a scalar value $x$ is encoded as:

$$\gamma(x) = (\sin(2\pi e^{0f}x), \cdots, \sin(2\pi e^{(L-1)f}x), \cos(2\pi e^{0f}x), \cdots, \cos(2\pi e^{(L-1)f}x))$$

If we want the embedding to be of size 32, we choose L = 16, and $f = \frac{\ln(1000)}{L-1}$ to be the maximum scaling factor for the frequencies. 

It produces the embedding pattern shown below:
![[Pasted image 20240712121307.png]]

### Residual Block
A residual block is a group of layers that contains a skip connection that adds the input to the output. Residual blocks help us to build deeper networks that can learn more complex patterns ==without suffering as greatly from vanishing gradient and degradation problems==. 

1. The vanishing gradient problem is the assertion that as the network gets deeper, **the gradient propagated through deeper layers is tiny and therefore learning is very slow**. 

2. The degradation problem is the fact that as neural networks become deeper, they are not necessarily as accurate as their shallower counterparts— **accuracy seems to become saturated at a certain depth and then degrade rapidly**.

![[Pasted image 20240712123524.png | Note that in some residual blocks, we also include an extra Conv2D layer with kernel size 1 on the skip connection, to bring the number of channels in line with the rest of the block.]]

By including a **skip connection** *highway* around the main weighted layers, the block has the option to bypass the complex weight updates and simply pass through the identity mapping. 

It allows the network to be trained to great depth without sacrificing gradient size or network accuracy.

### Downblocks and Upblocks
Each successive **Downblock** increases the number of channels via ResidualBlocks, while also applying a final **AveragePoolingLayer2D** layer in order to halve the size of the image. 

Each ResidualBlock is added to a list for use later by the UpBlock layers as skip connections across the U-Net.

An **Upblock** first applies an **UpSampling2D** layer that doubles the size of the image, through bilinear interpolation. Each successive **Upblock** decreases the number of channels via block_depth(=2) **ResidualBlocks**, while also concatenating the outputs from the **DownBlocks** through skip connections from across the U-Net. 
\
![[Pasted image 20240712124609.png]]
## Sampling from the DDM

We need to start with random noise and use the model to gradually **undo the noise**, until we are left with a recognizable picture of a flower.

Our model is trained to ==predict the total amount of noise that has been added to a given noisy image from the training set==, not just the noise that was added at the last time-step of the noising process. 

However, we do not want to undo the noise all in one go – predicting an image from pure random noise in one shot is clearly not going to work! 

We would rather ==mimic the forward process== and ==undo the predicted noise gradually over many time steps==, to allow the model to adjust to its own predictions. 

We can jump from $x_t$ to $x_{t-1}$ in two steps: 

1. Use our model prediction to calculate an estimate for original $x_0$. 
2. Reapplying the predicted noise to this image, but only over $t-1$ timesteps, to produce $x_{t-1}$.

![[Pasted image 20240713165822.png]]

If we repeat this process over a number of steps, we’ll eventually get back to an estimate for $x_{0}$ that has been guided gradually over many small steps. 

In fact, we are free to choose the number of steps we take, and crucially, it doesn’t have to be the same as the large number of steps in the training noising process .

Mathematically it is:

![[Pasted image 20240713171001.png]]
1. **Predicted** $x_{0}$: It is the estimated image calculated using the noise predicted by our network $\epsilon_{\theta}^{(t)}$ . 
2. We then scale this by the $t-1$ signal rate $\sqrt{\bar\alpha_{t-1}}$ .
3. Reapply the predicted noise, but this time scaled by the $t-1$ noise rate $\sqrt{1-\bar{\alpha}_{t-1} - \sigma_{t}^2 }$.
4. Additional Gaussian random noise $\sigma_{t}\epsilon_{t}$ is also added, with factors $\sigma_{t}$ determining how random we want our generation process to be.

The special case $\sigma_{t} = 0$ for all t corresponds to a type of model known as a **Denoising Diffusion Implicit Model (DDIM)**.

With a DDIM, the generation process is entirely deterministic—that is, the same random noise input will always give the same output. This is desirable as then we have a well-defined mapping between samples from the latent space and the generated outputs in pixel space.












