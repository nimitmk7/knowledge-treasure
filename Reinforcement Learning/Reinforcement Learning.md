![[Pasted image 20240501220840.png]]

**Objective**: Learning through experience/data to make good sequences of decisions under uncertainty.

Reinforcement Learning (Generally) Involves:
1. Optimization
2. Delayed consequences
3. Exploration
4. Generalization

## Optimization
- Goal is to find an optimal way to make decisions 
	- Yielding best outcomes or at least very good outcomes 
- Explicit notion of decision utility 
- Example: Finding minimum distance route between two cities given network of roads

## Delayed Consequences
- Decisions now can impact things much later... 
	- Saving for retirement
	- Finding a key in video game Montezuma’s revenge 

### Challenges
Introduces two challenges: 
1. **When planning**: decisions involve reasoning about not just immediate benefit of a decision but also its longer term ramifications 
2. **When learning**: temporal credit assignment is hard (what caused later high or low rewards?)

## Exploration
- Learning about the world by **making decisions**
	- Agent as a scientist
	- Learn to ride a bike by trying
- Censored Data
	- Only get a reward(labels) for decision made
	- Don’t know what would have happened for other decision
- Decisions impact what we learn about

## Generalization
Policy: Past experience ←→ Action


