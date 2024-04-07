Humans, it seems know things; and what they know helps them do things. In AI, **knowledge-based agents** use a process of **reasoning** over an internal representation of knowledge to decide what actions to take.

Knowledge-based agents can accept new tasks in the form of explicitly described goals; they can achieve competence quickly by being told or learning new knowledge about the environment; and they can adapt to changes in the environment by updating the relevant knowledge..

## Knowledge-based agents

The central component of a knowledge-based agent is its **knowledge base(KB).** A knowledge base is a set of sentences. (Here “sentence” is used as a technical term. It is related but not identical to the sentences of English and other natural languages.) 

Each sentence is expressed in a language called a **knowledge representation** language and represents some assertion about the world. When the sentence is taken as being given without being derived from other sentences, we call it an **axiom**.

The standard way to add/update sentences to the knowledge base and to query them are through operations **TELL** and **ASK** respectively. Both operations may involve **inference**-deriving new sentences from old.

### Outline

#### Pseudocode
**function** KB-AGENT(*percept*) **returns** an action
	**persistent**: *KB*, a knowledge base
			*t*, a counter, initially 0 indicating time
	 TELL(*KB*, MAKE-PERCEPT-SENTENCE(*percept, t*))
	 action ← ASK(*KB*, *MAKE-ACTION-QUERY*(*t*))
	 TELL(*KB*, *MAKE-ACTION-SENTENCE*(*action*, *t*))
	 t ← t + 1
	 **return** *action*

#### Intuition
1. Given a percept, the agent adds the percept to its knowledge base.(**TELL**)
2. Agent asks the knowledge base for the best action.(**ASK**)
3. It tells the knowledge base that it has in fact taken action. (**TELL**)

We only need to specify what the agent knows and what its goals are, to determine its behavior. The KB-agent can simply be built by TELLing it what it needs to know. 

Starting with an empty knowledge base, the agent designer can TELL sentences one by one until the agent knows how to operate in its environment. This is called the **declarative approach** to system building. In contrast, the **procedural approach** encodes desired behaviors directly as program code. 

We can also provide a KB-agent with mechanisms that allow it to learn for itself. These mechanisms, create general knowledge about its environment from a series of percepts.

## The Wumpus World
The Wumpus world is a simple world example to illustrate the worth of a knowledge-based agent and to represent knowledge representation. It was inspired by a video game **Hunt the Wumpus** by Gregory Yob in 1973.

The wumpus world is a cave consisting of rooms connected by passageways. Lurking somewhere in the cave is the terrible wumpus, a beast that eats anyone who enters its room. The wumpus can be shot by an agent, but the agent has only one arrow. Some rooms contain bottomless pits that will trap anyone who wanders into these rooms (except for the wumpus, which is too big to fall in). The only redeeming feature of this bleak environment is the possibility of finding a heap of gold.

![[Pasted image 20240406105655.png]]
- **Performance measure**: +1000 for climbing out of the cave with the gold,       –1000 for falling into a pit or being eaten by the wumpus, –1 for each action taken, and –10 for using up the arrow. The game ends either when the agent dies or when the agent climbs out of the cave.

- **Environment**: A $4 \times 4$ grid of rooms, with walls surrounding the grid. The agent al- ways starts in the square labeled $[1,1]$, facing to the east. The locations of the gold and the wumpus are chosen randomly, with a uniform distribution, from the squares other than the start square. In addition, each square other than the start can be a pit, with probability 0.2.
  
 - **Actuators**: The agent can move **Forward**, TurnLeft by $90^◦$, or TurnRight by $90^◦$. The agent dies a miserable death if it enters a square containing a pit or a live wumpus. (It is safe, albeit smelly, to enter a square with a dead wumpus.) If an agent tries to move forward and bumps into a wall, then the agent does not move. The action **Grab** can be used to pick up the gold if it is in the same square as the agent. The action **Shoot** can be used to fire an arrow in a straight line in the direction the agent is facing. The arrow continues until it either hits (and hence kills) the wumpus or hits a wall. The agent has only one arrow, so only the first Shoot action has any effect. Finally, the action **Climb** can be used to climb out of the cave, but only from square $[1,1]$.

- **Sensors**: The agent has 5 sensors, each of which gives a single bit of information: 
	- In the squares directly (not diagonally) adjacent to the wumpus, the agent will perceive a **Stench**.
	-  In the squares directly adjacent to a pit, the agent will perceive a **Breeze**.
	- In the square where the gold is, the agent will perceive a **Glitter.**
	- When an agent walks into a wall, it will perceive a **Bump**.
	- When the wumpus is killed, it emits a woeful **Scream** that can be perceived anywhere in the cave.

The percepts will be given to the agent program in the form of a list of five symbols; for example, if there is a stench and a breeze, but no glitter, bump, or scream, the agent program will get $[Stench,Breeze,None,None,None]$.

> [!INFO] The Wumpus environment is deterministic, discrete, static, sequential, partially observable and single-agent. 
>  *Static*: Wumpus and pits are not moving
>  *Discrete*: The states are discrete
>  *Partially observable*: The agent can only perceive the close environment such as an adjacent room.
>  *Sequential*: The order is important, and rewards may come only after many actions are taken



The transition model itself is unknown, because the agent does not know which *Forward* actions are fatal – in which case, discovering the locations of pits and wumpus completes the agent’s knowledge of the transition model.

For an agent in the environment, the main challenge is its initial ignorance of the configuration of the environment; overcoming this ignorance seems to require logical reasoning. 

In most instances of the wumpus world, it is possible for the agent to retrieve the gold safely. Occasionally, the agent must choose between going home empty-handed and risking death to find the gold. About 21% of the environments are utterly unfair, because the gold is in a pit or surrounded by pits.

### Example

The agent’s initial knowledge base contains the rules of the environment, as described previously; in particular, it knows that it is in $[1,1]$ and that $[1,1]$ is a safe square; we denote that with an “A” and “OK,” respectively, in square $[1,1]$.


![[Pasted image 20240406143806.png]]

| Environment State | Description                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (a)               | Agent A starts at [1, 1] facing right. The background knowledge β assures agent A that he is at [1, 1] and that it is OK = certainly not deadly.                                                                                                                                                                                                                                                                             |
|                   | Agent A gets the percept [Stench, Breeze, Glitter, Bump, Scream]=[None, None, None, None, None].                                                                                                                                                                                                                                                                                                                             |
|                   | Agent A infers from this percept and β that its both neighboring squares [1, 2] and [2, 1] are also OK: “If there was a Pit (Wumpus), then here would be Breeze (Smell) — but isn’t, so. . . ”.  The KB enables agent A to discover certainties about parts of its environment — even without visiting those parts.                                                                                                          |
| (b)               | Agent A is cautious, and will only move to OK squares. Agent A walks into [2, 1], because it is OK, and in the direction where agent A is facing, so it is cheaper than the other choice [1, 2]. Agent A also marks [1, 1] Visited.                                                                                                                                                                                          |
|                   | Agent A perceives [Stench, Breeze, Glitter, Bump, Scream]=[None, Breeze, None, None, None]. Agent A infers: “At least one of the adjacent squares [1, 1], [2, 2] and [3, 1] must contain a Pit. There is no Pit in [1, 1] by my background knowledge β. Hence [2, 2] or [3, 1] or both must contain a Pit.” Hence agent A cannot be certain of either [2, 2] or [3, 1], so [2, 1] is a dead-end for a cautious agent like A. |
![[Pasted image 20240406144326.png]]

*Agent moving in the environment inferring the contents of adjacent cells.  

(a) After the 3rd move and percept $[Stench, Breeze, Glitter, Bump, Scream]=[Stench, None, None, None, None]$. (b) After the 5th move with percept $[Stench, Breeze, Glitter, Bump, Scream]=[Stench, Breeze, Glitter, None, None]$

| Environment State | Description                                                                                                                                                                                                     |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| (a)               | Agent A has turned back from the dead end [2, 1] and walked to examine the other OK choice [1, 2] instead.                                                                                                      |
|                   | Agent A perceives [Stench, Breeze, Glitter, Bump, Scream]=[Stench, None, None, None, None].                                                                                                                     |
|                   | Agent A infers using also earlier percepts: “The Wumpus is in an adjacent square. It is not in [1, 1]. It is not in [2, 2] either, because then I would have sensed a Stench in [2, 1]. Hence it is in [1, 3].” |
|                   | Agent A infers using also earlier inferences: “There is no Breeze here, so there is no Pit in any adjacent square. In particular, there is no Pit in [2, 2] after all. Hence there is a Pit in [3, 1].”         |
|                   | Agent A finally infers: “[2, 2] is OK after all — now it is certain that it has neither a Pit nor the Wumpus.”                                                                                                  |
| (b)               | Agent A walks to the only unvisited OK choice [2, 2]. There is no Breeze here, and since the square of the Wumpus is now known too, [2, 3] and [3, 2] are OK too.                                               |
|                   | Agent A walks into [2, 3] and senses the Glitter there, so it grabs the gold and succeeds.                                                                                                                      |

Note that in each case for which the agent draws a conclusion from the available information, that conclusion is guaranteed to be correct if the available information is correct. This is a fundamental property of logical reasoning.
## Logic
**Syntax**: The rules governing the composition of well-formed expressions in a programming language. Example: “x + y = 4” is a valid sentence, “x4y+=” is not.

**Semantics**: The meaning of sentences. They define the truth of each sentence w.r.t to each possible world. Example: “x + y = 4” is true in a world where x is 2 and y is 2, but false in a world where x = 1 and y = 1.

### Model
Now, we use the term **model** in place of “possible world”.  Whereas possible worlds might be thought of as (potentially) real environments that the agent might or might not be in, models are mathematical abstractions, each of which has a fixed truth value (true or false) for every relevant sentence.

If a sentence $\alpha$ is true in model $m$ , we say that $m$ satisfies $\alpha$ or $m$ is a model of $\alpha.$ We use the notation $M(\alpha)$ to mean the set of all models of $α$.

### Knowledge Base Interaction

![[KB_Interact.png| 400]]

#### 1. Entailment
$\alpha$ entails $\beta$ if and only if, in every model $\alpha$ is true, $\beta$ is also true. 

Mathematically, $\alpha \vDash \beta$ if and only if $M(\alpha) \subseteq M(\beta)$. So $\alpha$ is a stronger assertion than $\beta$. Why ? It rules out *more* possible worlds.

 Entailment leaves the KB unchanged. As $\beta$ is true in all models where $\alpha$ is true. 
 So, if $M(KB) \cap M(\alpha_{3}) = M(KB)$ , we can say $KB \vDash \alpha_{3}$.
![[Pasted image 20240406165452.png]]
##### Example
$x = 0$ entails the sentence $xy = 0$. So if a KB has the knowledge $x=0$, when it is told $xy=0$, it does not get updated. As $xy=0$ is implied naturally for all cases where $x=0$.
#### 2. Contradiction

![[Pasted image 20240406170001.png | 300]]
The new statement contradicts what we know as part of KB. 
#### 3. Contingency

![[Pasted image 20240406170405.png]]
$M(KB) \cap M(\alpha_3) \subset M(KB)$ 
Thus, $\alpha_3$ adds non-trivial information to the KB and reduces the set of models that satisfy the KB.

##### Example
KB: $\lnot B_{11}$ 
$\alpha_3: B_{21}$

So, now KB: $\lnot B_{11} \cap B_{21}$

#### Ask Operation Responses
![[Pasted image 20240406194117.png]]
## Propositional Logic

### Syntax
![[Pasted image 20240406194335.png]]
### Semantics

![[Pasted image 20240406194353.png]]

## Model Checking Algorithm

The reasoning algorithm regarding the possible state of the environment in surrounding cells that the agent performed informally above, is called _model checking_ because it enumerates _all possible_ models to check that a sentence $a$ is supported by the KB i.e. $M(KB) ⊆ M(\alpha)$. 

![[Pasted image 20240406201524.png]]
*Possible Models in the presence of pits in cells $[1,2],[2,2]$ and $[3,1]$. 

There are $2^3=8$ possible models. The KB when the percepts indicate nothing in cell $[1,1]$ and a breeze in $[2,1]$ is shown as a solid line. With dotted line we show all models of $a_1=\text{"not have a pit in [1,2]"}$ sentence*.

![[Pasted image 20240406201556.png]]

### Inference Example

Using the operators and their semantics we can now construct an KB as an example for the wumpus world. We will use the following symbols to describe atomic and complex sentences in this world.

| Symbols   | Description                                |
| --------- | ------------------------------------------ |
| $P_{x,y}$ | Pit in cell [x,y]                          |
| $W_{x,y}$ | Wumpus (dead or alive) in cell [x,y]       |
| $B_{x,y}$ | Perception of a breeze while in cell [x,y] |
| $S_{x,y}$ | Perception of a stench while in cel l[x,y] |

Using these symbols we can convert the natural language assertions into logical sentences and populate the KB. The sentences $R_1$ and $R_2/R_3$ are general rules of the wumpus world. $R_4$ and $R_5$ are specific to the specific world instance and moves of the agent. 

| Sentence | Description                                                                                                  | KB                                              |
| -------- | ------------------------------------------------------------------------------------------------------------ | ----------------------------------------------- |
| $R_1$    | There is no pit in cel [1,1]                                                                                 | $\neg P_{1,1}$                                  |
| $R_2$    | The cell [1,1] is breezy if and only if there is a pit in the neighboring cell.                              | $B_{1,1} ⇔ (P_{1,2} \lor P_{2,1})$              |
| $R_3$    | The cell [2,1] is breezy if and only if there is a pit in the neighboring cell.                              | $B_{2,1} ⇔ (P_{1,1} \lor P_{2,2} \lor P_{3,1})$ |
| $R_4$    | Agent while in cell [1,1] perceives [Stench, Breeze, Glitter, Bump, Scream]=[None, None, None, None, None]   | $\neg B_{1,1}$                                  |
| $R_5$    | Agent while in cell [2,1] perceives [Stench, Breeze, Glitter, Bump, Scream]=[None, Breeze, None, None, None] | $B_{2,1}$                                       |

As the agent moves, it **uses the KB to decide whether a sentence is entailed by the the KB or not**. For example can we infer that there is no pit at cell $[1,2]$? The sentence of interest is $\alpha = \neg P_{1,2}$ and we need to prove that:
$$ KB \models \alpha $$
To answer such question we will use the model checking algorithm outlined in this chapter: ==enumerate all models and check that $\alpha$ is true for in every model where the KB is true==.

![[Pasted image 20240406201403.png]]

As described in the figure caption, 3 models out of the $2^7=128$ models make the KB true and in these rows the $P_{1,2}$ is false. 

Although the model checking approach was instructive, there is an issue with its complexity. Notice that if there are $n$ symbols in the KB there will be $2^n$ models, the time complexity is $O(2^n)$.

The symbolic representation together with the explosive increase in the number of sentences in the KB as time goes by, can’t scale.

## Propositional Theorem Proving
### Logical Equivalence
Two sentences $\alpha$ and $\beta$ are logically equivalent if they are true in the same set of models. Its denoted as $\alpha \equiv \beta$.
**Example**: $P \land Q \equiv Q \land P$

**Alternative definition**: $\alpha \equiv \beta$ if and only if $\alpha \models \beta$ and $\alpha \models \beta$ 
![[Pasted image 20240406202850.png]]
### Validity
A sentence is valid if it is true in all models. They are also known as **tautologies**- they are necessarily true. 
**Example**: $P \lor \lnot P$ 

From our definition of entailment, we can derive the **deduction theorem**, which was known to the ancient Greeks:

*For any sentences $α$ and $β$ ,$\alpha \models β$ if and only if the sentence $(α⇒β)$ is valid.*

### Satisfiability
A sentence is satisfiable if it is true in, or satisfied by **some** model. Satisfiability can be checked by enumerating the possible models until one is found that satisfies the sentence.

Validity and satisfiability are of course connected: $α$ is valid iff $¬α$ is unsatisfiable; contrapositively, $α$ is satisfiable iff $¬α$ is not valid. We also have the following useful result:

$α \models β$  if and only if the sentence $(α ∧ ¬β)$ is unsatisfiable.

### Inference Rules
These can be applied to derive a proof-a chain of conclusions that leads to the desired goal.
#### Modus Ponens
$$\frac{\alpha \Rightarrow \beta, \alpha }{\beta} $$

#### And-elimination
$$\frac{\alpha \space \land \space \beta}{\alpha}$$
### Enhancing the KB
Let us see how these inference rules and equivalences can be used in the wumpus world. We start with the knowledge base containing $R_{1}$ through $R_5$ and show how to prove ¬$P_{1,2}$, that is, there is no pit in $[1,2]$:

1. Apply **bi-conditional elimination** to R2 to obtain
$$R_6 : (B_{1,1} ⇒ (P_{1,2} ∨P_{2,1})) ∧ ((P_{1,2} ∨P_{2,1}) ⇒ B_{1,1}).$$
2. Apply And-Elimination to $R_6$ to obtain $$R_7 : ((P_{1,2} ∨P_{2,1}) ⇒ B_{1,1}).$$
3. Logical equivalence for contrapositives gives $$R_8 : (¬B_{1,1} ⇒ ¬(P_{1,2} ∨P_{2,1}))$$
4. Apply Modus Ponens with $R_{8}$ and the percept $R_{4}$ (i.e., $¬B_{1,1}$), to obtain $$R_9: ¬(P_{1,2}∨P_{2,1})$$
5. Apply De Morgan’s rule, giving the conclusion
$$R10: ¬P_{1,2}∧¬P_{2,1} $$
That is, neither $[1,2]$ nor $[2,1]$ contains a pit.

> Searching for proofs is an alternative to enumerating models. In many practical cases, finding a proof can be more efficient because the proof can ignore irrelevant propositions, no matter how many of them there are.


## References
1. AIMA Chapter 7
2. https://pantelis.github.io/artificial-intelligence/aiml-common/lectures/logical-reasoning/propositional-logic/_index.html
3. 