Generative artificial intelligence (generative AI) is a type of AI that can create new content and ideas, including conversations, stories, images, videos, and music.
## Introduction

Watch: 
<iframe width="560" height="315" src="https://www.youtube.com/embed/XZ0PMRWXBEU?si=u7_mwRnlUWbxcfyi" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


**Challenge**: Understand complex, unstructured inputs.(images, natural language, computational speech/audio, sensor inputs)

> What I cannot create, I cannot understand - Richard Feynman
> What I understand, I can create - Generative Modeling

### Statistical Generative Models
A statistical generative model is a probability distribution $p(x)$
- **Data**: samples(e.g. images of bedrooms)
- **Prior knowledge**: parametric form(like Gaussian), loss function(like Max. Likelihood), optimization algorithm, etc.
![[Pasted image 20240515230522.png]]
- It is generative because **sampling from $p(x)** generates new data samples, that look like the ones used in training.

#### A simulator for data generation

![[Pasted image 20240515230814.png]]
1. Instead of data as an input in case of traditional ML/DL models, here we think of **data as an output.**
2. We are interested in simulators which can controlled through **control signals**.
3. You can query the simulator with potential datapoints, and the model will be able to tell you how likely they are to be produced by the data simulator.

### Data Generation in Real World

![[Pasted image 20240515231617.png]]
![[Pasted image 20240515231722.png]]
![[Pasted image 20240515231751.png]]



## Categories of Models

### Diffusion Models

### Generative Adversarial Networks(GANs)

### Variational Autoencoders

### Transformer-Based Models



## References
1. [What is Generative AI? - Amazon Web Services](https://aws.amazon.com/what-is/generative-ai/)
2. 