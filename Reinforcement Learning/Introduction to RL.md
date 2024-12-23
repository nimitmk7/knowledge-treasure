# Many Faces of Reinforcement Learning
RL sits at the intersection of many sciences, it is essentially the field of making optimal decisions.
![[Pasted image 20241222165652.png]]

# Characteristics of RL

What makes reinforcement learning different from other machine
learning paradigms?

1. There is no supervisor, only a reward signal
2. Feedback is delayed, not instantaneous
3. Time really matters (sequential, non i.i.d data)
4. Agent’s actions affect the subsequent data it receives

# Examples of RL
1. Fly stunt manoeuvres in a helicopter
2. Defeat the world champion at Backgammon
3. Manage an investment portfolio
4. Control a power station
5. Make a humanoid robot walk
6. Play many different Atari games better than humans

# Reference

<iframe width="560" height="315" src="https://www.youtube.com/embed/2pWv7GOvuf0?si=WQAjoHJk97629Ysx" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# The RL Problem

## Reward
- A reward $R_{t}$ is a scalar feedback signal.
- Indicates how well agent is doing at step $t$
- The agent’s job is to maximize cumulative reward. 

Reinforcement learning is based on the reward hypothesis.

> [!INFO] Definition(Reward Hypothesis)
> All goals can be described by the maximization of expected cumulative reward.

### Examples
- Fly stunt manoeuvres in a helicopter
	- +ve reward for following desired trajectory
	- −ve reward for crashing
- Defeat the world champion at Backgammon
	- +/−ve reward for winning/losing a game
- Manage an investment portfolio
	- +ve reward for each $ in bank
- Control a power station
	- +ve reward for producing power
	- −ve reward for exceeding safety thresholds
- Make a humanoid robot walk
	- +ve reward for forward motion
	- −ve reward for falling over
- Play many different Atari games better than humans
	- +/−ve reward for increasing/decreasing score
### Sequential Decision Making

- **Goal**: select actions to maximise total future reward
- Actions make have long term consequences
- Reward may be delayed
- It may be better to sacrifice intermediate reward to gain more long-term reward.
- Examples:
	- A financial investment (may take months to mature)
	- Refuelling a helicopter (might prevent a crash in several hours)
	- Blocking opponent moves (might help winning chances many moves from now)
## Environments

### Agent and Environment

![[Pasted image 20241222173242.png | 500]]
![[Pasted image 20241222173410.png | 500]]
- At each step $t$ the agent: 
	- Executes action $A_t$ 
	- Receives observation $O_t$ 
	- Receives scalar reward $R_t$ 
- The environment: 
	- Receives action $A_t$ 
	- Emits observation $O_{t+1}$ 
	- Emits scalar reward $R_{t+1}$ 
- t increments at env. step

## State

### History and State
- The **history** is the sequence of observations, actions, rewards. $$H_{t} = O_{1}, R_{1}, A_{1}, \dots, A_{t-1}, O_{t}, R_{t}$$
- i.e. all observable variables upto time $t$.
- i.e. the sensorimotor stream of a robot or embodied agent.
- ==What happens next depends on the history==:
	1. The agent selects actions
	2. The environment selects observations/rewards
- **State** is the information used to determine what happens next.
- Formally, state is a function of the history: $$S_{t} = f(H_{t})$$
### Environment State
- The environment state $S_{t}^e$ is the environment’s private representation.
- i.e. whatever data the environment uses to pick the next observation/reward
- The environment state is not usually visible to the agent
- Even if $S_{t}^e$ is visible, it may contain irrelevant information.
![[Pasted image 20241222194133.png | 500]]

### Agent State
- The agent state $S_{t}^a$ is the agent’s internal representation. 
- i.e. whatever information the agent uses to pick the next action.
- i.e. it is the information used by reinforcement learning algorithms.
- It can be any function of history: $$S_{t}^a =f(H_{t})$$
 ![[Pasted image 20241222202601.png | 500]]
### Information State
An information state (a.k.a. Markov state) contains all useful information from the history.

> [!INFO] Definition
>  A state $S_t$ is Markov if and only if $$P[S_{t+1}|S_{{t}}] = P[S_{t+1} |S_{1}, \dots, S_{t}]$$ 

- “The future is independent of the past given the present”. $$H_{1:t} → S_{t} → H_{t+1:\infty}$$
- Once the state is known, the history may be thrown away
- i.e. The state is a sufficient statistic of the future.
- The environment state $S_{t}^e$ is Markov.
- The history $H_{t}$ is Markov. (**Tautology!!!**)
### Rat Example

![[Pasted image 20241222203525.png]]

- What if agent state = last 3 items in sequence?
	- Rat will be electrocuted.
- What if agent state = counts for lights, bells and levers?
	- Rat will get food.
- What if agent state = complete sequence?
	- We don’t know.

### Fully Observable Environments
**Full observability**: agent directly observes environment state. $$O_{t} = S_{t}^a = S_{t}^e$$
- Agent state = environment state = information state.
- Formally, this is a **Markov decision process** (MDP) 
### Partially Observable Environments

- **Partial observability**: agent indirectly observes environment: 
	- A robot with camera vision isn’t told its absolute location.
	- A trading agent only observes current prices.
	- A poker playing agent only observes public cards

- Now agent state $\neq$ environment state.
- Formally this is a **partially observable Markov decision process** (POMDP).
- Agent must construct its own state representation $S_{t}^a$.
	- **Method 1** : Complete history: $S_{t}^a = H_{t}$
	- **Method 2**: Beliefs of environment state: $S_{t}^a = (\mathbb P[S_{t}^e = s^1], ..., P[S_{t}^e = s^n])$
		- A probability distribution over environment states, stating where agent is in the environment
		- Use the result of this distribution to take decisions.
	- **Method 3**: Recurrent neural network: $S_{t}^a = \sigma(S_{t−1}^aW_{s} + O_{t}W_{o})$
		- Linear combination of last agent state and the last observation, along with an activation function == New State







