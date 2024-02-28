> [!INFO] This note is created from excerpts of Chapter 2 of the book Artificial Intelligence: A Modern Approach

## Agents and Environments
### Agent
An agent is anything that can be viewed as perceiving its environment through sensors(e.g. camera) and acting upon that environment through actuators(e.g. motors of a robotic hand)

### Environment
In practice, it is just that part of the universe whose state we care about when designing this agent—the part that affects what the agent perceives and that is affected by the agent’s actions.

### Percept Sequence
We use the term percept to refer to the content an agent’s sensors are perceiving. An agent’s percept sequence is the complete history of everything the agent has ever perceived. 

In general, an agent's choice of action at any given instant can depend on its built-in knowledge and on the entire percept sequence observed to date, but not on anything it hasn't perceived.

Mathematically speaking, an agent's behavior can be described by the agent function that maps any given percept sequence to the action. Internally, the **agent function** will be implemented by an **agent program**.

![[Pasted image 20240202131645.png]]
## Good Behavior: The Concept of Rationality

A rational agent is one that does the right thing. Obviously, doing the right thing is better than doing the wrong thing, but ==what does it mean to do the right thing?==

### Performance Measures

AI has generally stuck to one notion called **consequentialism**: we evaluate an agent's behavior by it's consequences. If the sequence of states the environment undergoes due to the agent's actions are desirable, then the agent has performed well.

This notion of desirability is captured by a **performance measure** that evaluates any sequence of environment states. 

As desirability can be a very contextual thing to define, As a general rule it is better to design performance measures according to what one actually wants to be achieved in the environment, rather than according to how one thinks the agent should behave.

### Rationality

What is rational at any time depends on 4 things:
1. The performance measure that defines the criterion of success. 
2. •The agent’s prior knowledge of the environment.
3. The actions that the agent can perform.
4. The agent’s percept sequence to date.

This leads to a definition of a rational agent:

> For each possible percept sequence, a rational agent should select an action that is expected to maximize its performance measure, given the evidence provided by the percept sequence and whatever built-in knowledge the agent has.

### Learning and Autonomy

**Rationality** is not the same as perfection. Rationality maximizes *expected* performance while **perfection** maximizes *actual* performance.

Doing actions in order to modify future percepts—sometimes called information gathering—is an important part of rationality. An example of information gathering is provided by the exploration that must be undertaken by a vacuum-cleaning agent in an initially unknown environment.

Our definition requires a rational agent not only to gather information but also to learn as much as possible from what is percieves. To the extent that an agent relies on the prior knowledge of its designer rather than on its own percepts and learning processes, we say that the agent lacks **autonomy**.

## Nature of Environments

### Specifying the Task Environment 
In designing an agent, the first step must always be to specify the task environment as fully as possible.

P - Performance
E - Environment
A - Actuators
S - Sensors

### Properties of Task Environments

1. **Fully observable** vs **Partially observable**
    If an agent’s sensors give it access to the complete state of the environment at each point in time, then we say that the task environment is **fully observable**. A task environment is effectively fully observable if the sensors detect all aspects that are relevant to the choice of action; relevance in turn depends on the performance measure.
    
    An environment might be partially observable due to noisy and inaccurate sensors or because parts of the state are simply missing from sensor data. If the agent has no sensors at all then the environment is unobservable. 
2. **Single-agent** vs **Multi-agent** 
3. **Deterministic** vs **Non-deterministic**
    If the next state of the environment is completely determined by the current state and the action executed by the agent(s), then we say the environment is deterministic; otherwise, it is nondeterministic.
4. **Episodic** vs **Sequential**
5. **Static** vs **Dynamic**
    If the environment can change while an agent is deliberating, then we say the environment is dynamic for that agent; otherwise, it is static. If the environment itself does not change with the passage of time but the agent’s performance score does, then we say the environment is **semidynamic**.
6. **Discrete** vs **Continuous**
     The discrete/continuous distinction applies to the state of the environment, to the way time is handled, and to the percepts and actions of the agent.
7. **Known** vs **Unknown**
     In a known environment, the outcomes (or outcome probabilities if the environment is nondeterministic) for all actions are given. Obviously, if the environment is unknown, the agent will have to learn how it works in order to make good decisions.

Examples: 

![[Pasted image 20240202150813.png]]

The hardest case is partially observable, multiagent, nondeterministic, sequential, dy- namic, continuous, and unknown. Taxi driving is hard in all these senses, except that the driver’s environment is mostly known.

## The Structure of Agents

The job of AI is to design an **agent program** that ==implements the agent function==--the mapping from percepts to actions. We assume this program will run on some sort of computing device with physical sensors and actuators--the **agent architecture**.

AGENT = ARCHITECTURE + PROGRAM

### Agent Programs

They take the current percept as input from the sensors and return an action to the actuators.

It is instructive to consider why the table-driven approach to agent construction is doomed to failure. Let P be the set of possible percepts and let T be the lifetime of the agent (the total number of percepts it will receive). The lookup table will contain $∑^T_{t=1}|P|^t$ entries.

Therefore, ==the key challenge for AI is to find out how to write programs that, to the extent possible, produce rational behavior from a smallish program rather than from a vast table==.

### Simple Reflex agents






 
 
