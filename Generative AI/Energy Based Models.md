Energy-based models are a broad class of generative model that borrow a key idea from modeling physical systems: 

> The probability of an event can be expressed using a **Boltzmann distribution**, a specific function that normalizes a real-valued energy function between 0 and 1.

This distribution was originally formulated in 1868 by Ludwig Boltzmann, who used it to describe gases in thermal equilibrium.
### Analogy

Diane Mixx was head coach of the long-distance running team in the fictional French town of Long-au-Vin. She was well known for her exceptional abilities as a trainer and had acquired a reputation for being able to turn even the most mediocre of athletes into world-class runners.

Her methods were based around assessing the energy levels of each athlete. Over years of working with athletes of all abilities, she had developed an incredibly accurate sense of just how much energy a particular athlete had left after a race, just by looking at them. The lower an athlete’s energy level, the better—elite athletes always gave everything they had during the race!

To keep her skills sharp, she regularly trained herself by measuring the contrast between her energy sensing abilities on known elite athletes and the best athletes from her club. She ensured that the divergence between her predictions for these two groups was as large as possible, so that people would take her seriously if she said that she had found a true elite athlete within her club.

The real magic was her ability to convert a mediocre runner into a top-class runner. The process was simple—she measured the current energy level of the athlete and worked out the optimal set of adjustments the athlete needed to make to improve their performance next time. Then, after making these adjustments, she measured the athlete’s energy level again, looking for it to be slightly lower than before, explaining the improved performance on the track. This process of assessing the optimal adjustments and taking a small step in the right direction would continue until eventually the athlete was indistinguishable from a world-class runner.

## Core Concept
Energy based models attempt to model the true data-generating distribution using a **Boltzmann distribution**. $$p(x) = \frac {e^{-E(x)}}{\int_{\hat x \in X} e ^{E(\hat x)}}$$
In practice, this amounts of training a neural network $E(x)$ to output low scores for likely observations(so $p(x)$ is close to 1) and high scores for unlikely observations(so $p(x)$ is close to 0).

### Challenges
1. It is not clear how we should use our model for sampling new observations–we can use it to generate a score given an observation, but how do we generate an observation that has a low score ?
2. The normalizing denominator **contains an integral that is intractable** for all but the simplest of problems. If we can’t calculate it, we cannot use MLE(Maximum Likelihood estimation) to train the model, as this requires that $p(x)$ is a valid probability distribution.

So we use approximation techniques to ensure we never need to calculate the intractable denominator. 

We sidestep the tricky intractable denominator problem by using a technique called **contrastive divergence** (for training) and a technique called **Langevin dynamics** (for sampling). 

## The Energy Function
The energy function $E_{\theta}(x)$ is a neural network with parameters $\theta$ that can transform an input image $x$ into a scalar value. 

One of the popular activation functions is [[Swish]]. It doesn’t have the vanishing gradient problem, which is important for energy-based models.

### Example
A set of stacked 2D convolution layers, with swish activation function, with the final layer as a single fully connected unit, with a linear activation function. So essentially we get a model that converts the input image into a scalar energy value.
## Sampling using Langevin Dynamics

The technique makes use of the fact that we can compute the gradient of the energy function with respect to its input. 

If we start from a random point in the sample space and take small steps in the opposite direction of the calculated gradient, we will gradually reduce the energy function.

### Difference between training grad descent vs Sampling Gradient Descent 

When training a neural network, we calculate the ==gradient of the loss function with respect to the parameters of the network== (i.e., the weights) using back-propagation. Then we ==update the parameters a small amount in the direction of the negative gradient==, so that over many iterations, ==we gradually minimize the loss.==

With Langevin dynamics, we ==keep the neural network weights fixed and calculate the gradient of the output with respect to the input==. Then we ==update the input a small amount in the direction of the negative gradient, so that over many iterations, we gradually minimize the output (the energy score)==.

Both processes utilize the same idea (gradient descent), but are applied to different functions and with respect to different entities.

### Formulation

$$x^k = x^{k-1} - \eta\nabla_{x}E_{\theta}(x^{k-1})+\omega$$

where $\omega \sim \mathscr N(0, \sigma)$ and $x^0 \sim \mathscr U(-1,1)$. $\eta$ is the step size hyper-parameter that must be tuned. 

> [!INFO] Notation clarification
> $\mathscr U(-1,1)$ is the uniform distribution on the range [-1,1]

## Training with Contrastive Divergence
We ==cannot apply maximum likelihood estimation==, because the energy function does not output a probability. It outputs a score that does not integrate to 1 across the sample space.

The value that we want to minimize (as always) is the negative log-likelihood of the data: $$\mathscr L = -\mathbb E_{x \sim data}[\log p_{\theta}(\mathbf x)]$$
When $p_{\theta}(x)$ has the form of a Boltzmann distribution, with the energy function $E_{\theta}(x)$ , it can be shown that the gradient of this value can be written as follows:
$$\nabla_{\theta}\mathscr L = \mathbb E_{x \sim data}[\nabla_{\theta}E_{\theta}(\mathbf x)] - E_{x \sim model}[\nabla_{\theta}E_{\theta}(\mathbf x)]$$

This intuitively makes a lot of sense-we want to train the model to:
1. output large negative energy scores for real observations, and 
2. large positive energy scores for generated fake observations 

so that the contrast between these two extremes is as large as possible.

In other words, we can calculate the difference between the energy scores of real and fake samples and use this as our loss function.

To calculate the energy scores of fake samples, we would need to be able to sample exactly from the distribution $p_{\theta}(\mathbf x)$, which isn’t possible due to the intractable denominator.

Instead we can use our Langevin sampling procedure to generate a set of observations with low energy scores. The process would need to run for infinitely many steps to produce a perfect sample(which is obviously impractical), so instead we run for some small number of steps, on the assumption that this is good enough to produce a meaningful loss function.

![[Pasted image 20240708092453.png]]

