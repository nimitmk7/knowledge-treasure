Bayesian filtering is ==a mathematical tool that uses measurement knowledge to construct a state probability density function==. 

It is a recursive algorithm which consists of 2 parts: 
1. Prediction
2. Innovation

## Model

**Assumption**: True state is an unobserved Markov Process.

>[!INFO] What is a Markov Process ?
>
>A **Markov process** is a stochastic model describing a sequence of possible events in which the probability of each event depends only on the state attained in the previous event. 
>
> Essentially, in our context the present state is a complete summary of the past, or a sufficient statistic for all past measurements and actions.

## Framework 
Here $x$ denotes state, $z$ denotes observations, $u$ denotes actions.
### Given
-  Stream of observations $z$ and action data $u$: $$ d_{t} = \{u_{1}, z_{{1},\space \dots \space,} u_{t}, z_{t}\}$$
- Sensor Model $P(z/x)$
- Action Model $P(x’/u,x)$
- Prior Probability of System state $P(x)$

### Wanted
- Estimate the state $X$ of a dynamical system
- The posterior of a state is also called **Belief**: $$ Bel(x_{t}) = P(x_{t}|u_{1}, z_{1}, \dots, u_{t}, z_{t})$$
### Markov Assumption

![[Pasted image 20240404225407.png]]
$P(z_t | x_{0:t}, z_{1:t-1}, u_{1:1}) = P(z_{t}|x_{t})$
$P(x_{t}| x_{1:t-1}, z_{1:t-1}. u_{{1:t}}) = P(x_{t}|x_{t-1}, u_{t})$

- Underlying assumptions
	- Independent Noise
	- Perfect Model, no approximation errors

## Formulation
![[Pasted image 20240404231011.png]]
Here, 
$z$ is observation, $u$ is action, $x$ is state.

## Algorithm

![[Pasted image 20240404231135.png]]


## Helpful Videos
<iframe width="560" height="315" src="https://www.youtube.com/embed/xamzdNUN1o0?si=fuNeQsRtmur1Xd4M&amp;start=2410" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


## Application to Robotics
It is used for calculating the probabilities of multiple beliefs to allow a robot to infer its position and orientation. Essentially, it allows robots to **continuously update their most likely position** within a co-ordinate system **based on the most recently acquired sensor data.** 

Refer to [[Kalman Filter]].









