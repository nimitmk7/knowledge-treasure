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
# Inside an RL agent

- An RL agent may include one or more of these components: 
	- **Policy**: agent’s behaviour function 
	- **Value function**: how good is each state and/or action 
	- **Model**: agent’s representation of the environment
## Policy
- A policy is the agent’s behavior
- It is a map from state to action
- **Deterministic Policy**: $a = \pi(s)$
- **Stochastic Policy**: $\pi(a|s) = \mathbb P[A=a|S=s]$
## Value Function
- It is prediction of future reward
- Used to evaluate the goodness/badness of states
- And therefore to select between actions, e.g. $$v_{\pi}(s)= \mathbb E_{\pi}[R_{t} + \gamma R_{t+1}+\gamma^2R_{t+2} + \dots \space |\space S_{t} = s]$$
	- The discount factor $\gamma$ is used to assign more weight to rewards in immediate future than far future. 

## Model
- A model predicts what the environment will do next.
- **Transitions**: $P$ predicts the next state(i.e. dynamics)
- **Rewards**: $R$ predicts the next(immediate) reward. 
 $$P_{ss'}^a = \mathbb{P}[S' = s' | S=s, A=a]$$
 $$R_{s}^a = \mathbb E[R|S=s, A=a]$$
 

## Maze Example
- Rewards: -1 per time-step
- Actions: N,E,S,W
- State: Agent’s location
- ![[Pasted image 20250116165643.png | 300]]
### Policy
- Arrows represent policy $\pi(s)$ for each state $s$.
- ![[Pasted image 20250116165753.png | 300]]
### Value Function
- Numbers represent value $v_{\pi}(s)$ of each state $s$.
- ![[Pasted image 20250116165904.png | 300]]

### Model
- Agent may have an internal model of the environment.
- Dynamics: how actions change the state.
- Rewards: how much reward from each state.
- The model may be imperfect
- Grid layout represents transition model $P_{ss'}^a$
- Numbers represent immediate reward $R_{s}^a$ from each state $s$ (same for all $a$).
- ![[Pasted image 20250116170127.png | 300]]

# Categorizing RL agents(1)
- Value Based
	- No Policy(Implicit)
	- Value Function
- Policy Based
	- Policy
	- No Value Function
- Actor Critic
	- Policy
	- Value Function

# Categorizing RL agents(2)
- Model Free
	- Policy and/or Value Function
	- No model
- Model Based
	- Policy and/or Value Function
	- Model
![[Pasted image 20250116171553.png | 300]]

# Problems within RL
2 fundamental problems in sequential decision making:
1. Reinforcement Learning
	1. Environment is initially unknown
	2. The agent interacts with the environment
	3. The agent improves its policy. 
2. Planning:
	1. A model of the environment is known
	2. The agent performs computations with its model(without any external interaction).
	3. The agent improves its policy.

## Atari Example
### RL

![[Pasted image 20250116171942.png | 500]]

- Rules of the game are unknown. 
- Learn directly from interactive game-play.
- Pick actions on joystick, see pixels and scores
### Planning

- Rules of the game are known
- Can query emulator
	- perfect model inside agent’s brain 
- If I take action $a$ from state $s$: 
	- what would the next state be? 
	- what would the score be? 
- Plan ahead to find optimal policy.
	- e.g. tree search
![[Pasted image 20250116172152.png]]
## Exploration and Exploitation
- Reinforcement learning is like trial-and-error learning.
- The agent should discover a good policy, from its experiences of the environment, without losing too much reward along the way.
- **Exploration** finds more information about the environment.
- **Exploitation** exploits known information to maximize reward.
- It is usually important to explore as well as exploit.
### Examples
- Restaurant Selection
	- **Exploitation** Go to your favorite restaurant 
	- **Exploration** Try a new restaurant 
- Online Banner Advertisements 
	- **Exploitation** Show the most successful advert 
	- **Exploration** Show a different advert 
- Oil Drilling 
	- **Exploitation Drill** at the best known location 
	- **Exploration Drill** at a new location 
- Game Playing 
	- **Exploitation** Play the move you believe is best 
	- **Exploration** Play an experimental move

## Prediction and Control
- Prediction: evaluate the future 
	- Given a policy 
- Control: optimise the future 
	- Find the best policy

### Gridworld Example: Prediction

![[Pasted image 20250116184204.png]]
- What is the value function for the uniform random policy?
### Gridworld Example: Control
![[Pasted image 20250116184412.png]]
What is the optimal value function over all possible policies? 
What is the optimal policy?

















