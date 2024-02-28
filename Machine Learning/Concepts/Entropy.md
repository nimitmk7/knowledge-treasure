## Motivation
A cornerstone of information theory is the idea of quantifying how much information there is in a message. More generally, this can be used to quantify the information in an event and a random variable, called entropy, and is calculated using probability.

## Intuition
The intuition behind quantifying information is the idea of measuring how much surprise there is in an event. Those events that are rare (low probability) are more surprising and therefore have more information than those events that are common (high probability).

> The basic intuition behind information theory is that learning that an unlikely event has occurred is more informative than learning that a likely event has occurred.

# Formulation
An outcome $x_t$ carries information that is a function of the probability of this outcome $P(x_t)$ by, 

$I(x_t) = \log_2 \frac{1}{P(x_t)} = - \log_2 P(x_t)$


Entropy over a probability distribution is:

$$H(P) =  - \mathbb{E} \log_2 P(x) = -\sum_x P(x) log_2P(x)$$
### Example

Entropy over distribution of letters in some text.
![[Pasted image 20240214100948.png]]

### Binary case

$H(p) = - [p \log_2 p + (1-p) \log_2(1-p)]$

Entropy of $p$, where $p$ is the probability that the outcome is 1, is:

![[Pasted image 20240214101418.png]]
As you can see the maximum entropy is when the outcome is most unpredictable i.e. when a 1 can show up with uniform probability (in this case equal probability to a 0, or probability of 0 showing up and 1 showing up is equal, which signals maximum uncertainty).

## Relative Entropy/KL divergence

**Kullback-Leibler divergence** metric (relative entropy) is a statistical measurement from information theory that is commonly used to quantify the difference between one probability distribution from a reference probability distribution.

$$
D_{KL}(P(x)||Q(x)) =  \mathbb{E}[\ln P(x) - \ln Q(x)] = \sum_xp(x) ln \left( \frac{p_x}{q_x} \right)
$$
If the two distributions are identical, $KL=0$ - in general however $KL(P||Q) \ge 0$. One key element to understand is that $KL$ is not a true distance metric as its asymmetric.

![[Pasted image 20240214134235.png]]
- For detailed explanation watch: 

<iframe width="560" height="315" src="https://www.youtube.com/embed/q0AkK8aYbLY?si=E6kUSiLP4W7OkHQV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


