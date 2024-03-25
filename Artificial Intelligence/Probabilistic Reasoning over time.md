## Bayes Filter
Consider an embodied agent(a robot) that moves in an environment.

![[Pasted image 20240323221634.png]]

State of the environment($s_t$)
1. Dynamic Variables: Pose(6D)
2. Static state Variables: Location of obstacles, walls, etc.

### Quantities of Interest
1. **Action** $a_t$: Agent first takes an action and then sense the environment.
2. **Measurement** $z_t$ : Represent row of predictions out of an object detection
3. **State Transition Model** $P(S_t | S_{1:t-1}, a_{1:t}, z_{1:t-1})$ : Generative model of state transition.

#### Markov Assumption
At its core, the Markov assumption proposes that the future state of a process relies solely on the current state by disregarding the journey to the current state. This attribute is commonly known as the **memoryless** aspect or "absence of memory" disregarding in Markov processes.

A complete state $S_t$ is a complete summary of the past.

Using this assumption, the state transition model can be re-written as:
$P(S_t | S_{t-1}, a_t)$ 

4. Generative Model of Measurements: $P(Z_t\space|\space S_t)$

## Dynamic Bayesian Networks






