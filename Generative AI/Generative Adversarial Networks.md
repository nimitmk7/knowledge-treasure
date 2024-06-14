## Analogy

- **Brickki** -  A company that specializes in producing high-quality blocks/bricks of all shapes and sizes.
- **Nick** is the head of quality control. 
- Competitor(**BricBlock**) making counterfeit copies of Briccki bricks and is mixing them into bags received by your customers. 
- Nick has to become an expert at telling the difference between counterfeit and original ones, to intercept them on the production line itself.
- Over time, by listening to customer feedback, Nick gradually becomes more adept at spotting fakes.
- Forgers(Brickblock) react to this by making changes to the forgery so that the difference is even harder to spot.
- The process continues, with 
	- Forgers iteratively updating their brick creating technologies
	- Nick iteratively becoming more accomplished at intercepting the fakes.

- With every iteration, it becomes more and more difficult to tell the difference between the real Brickki bricks and those created by BricBlock.
- It seems that this simple game of cat and mouse is enough to drive significant improvement in both the quality of the forgery and the quality of the detection.

Nick → Discriminator
BrickBlock → Generator

## Introduction

A GAN is a battle between two adversaries, the **generator** and the **discriminator.** 

The **generator** tries to convert random noise into observations that look as if they have been sampled from the original dataset.

The **discriminator** tries to predict whether an observation comes from the original dataset or is one of the generator’s forgeries.

![[Pasted image 20240612112312.png]]

At the start of the process, the generator outputs noisy images and the discriminator predicts randomly. 

==The key to GANs lies in how we alternate the training of the two network==s, so that as the generator becomes more adept at fooling the discriminator, the discriminator must adapt in order to maintain its ability to correctly identify which observations are fake. 

This drives the generator to find new ways to fool the discriminator, and so the cycle continues.

## Deep Convolutional GAN(DCGAN)

### The Discriminator
**Goal**: Predict if an image is real or fake → Supervised classification

**Possible Simplest Architecture**: Stacked convolutional layers with a single output node.

### The Generator
Input: A vector drawn from a multivariate standard normal distribution. 
Output: An image of the same size as an image in the original training data.

It is similar to the decoder in VAE, as it fulfills exactly the same purpose. Converts a vector in latent space to an image. 

> [!NOTE] 
> The concept of mapping from a latent space back to the original domain is very common in generative modeling, as it gives us the ability to manipulate vectors in the latent space to change high-level fea‐ tures of images in the original domain.

### Training

1. Train the discriminator by creating a training set where some of the images are *real observations* from the training set and same are *fake outputs* from the generator.
	1. This is a supervised learning classification problem, where labels are 1 for the real images and 0 for the fake images, with binary cross entropy as the loss function.
2. To train the generator, we need to find a way of scoring each generated image so that it can optimize toward high-scoring images. We use the *discriminator* to get the score.
3. Crucially, we must alternate the training of these two networks, making sure that we only update the weights of one network at a time. For example, during the generator training process, only the generator’s weights are updated.
4. If we allowed the discriminator’s weights to change as well, the discriminator would just adjust so that it is more likely to predict the generated images to be real, which is not the desired outcome. We want generated images to be predicted close to 1 (real) because the generator is strong, not because the discriminator is weak.



![[Pasted image 20240613105507.png]]


