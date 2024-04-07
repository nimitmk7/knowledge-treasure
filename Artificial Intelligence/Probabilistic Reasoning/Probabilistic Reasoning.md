


## Bayesian Network

A data structure used to represent dependencies among variables. It can represent any fully joint probability distribution concisely. 

A directed graph in which each node is annotated with quantitative probability information. 

1. Each node: A random variable(Discrete/Continuous)
2. Directed Links/Arrows connect pairs of nodes. 
3. Each Node $X_i$ has associated probability information $\theta(X_i | Parents(X_i))$ that quantifies the effect of parents on the node using a finite number of parameters.

![[Pasted image 20240323212538.png]]
$X$ → $Y$ : $X$ has a direct influence on $Y$. 
Therefore the causes are parents of the effects.

Once the topology of the Bayes net is laid out, we need only specify the local probability information for each variable, in the form of a conditional distribution given its parents. The full joint distribution for all the variables is defined by the topology and the local probability information.

### Example

 You have a new burglar alarm installed at home. It is fairly reliable at detecting a burglary, but is occasionally set off by minor earthquakes. (This example is due to Judea Pearl, a resident of earthquake- prone Los Angeles.) You also have two neighbors, John and Mary, who have promised to call you at work when they hear the alarm. John nearly always calls when he hears the alarm, but sometimes confuses the telephone ringing with the alarm and calls then, too. Mary, on the other hand, likes rather loud music and often misses the alarm altogether. Given the evidence of who has or has not called, we would like to estimate the probability of a burglary.

![[Pasted image 20240323213152.png]]
Each row in a CPT contains the conditional probability of each node value for a conditioning case. A conditioning case is just a possible combination of values for the parent nodes—a miniature possible world, if you like. 

Each row must sum to 1, because the entries represent an exhaustive set of cases for the variable. For Boolean variables, once you know that the probability of a true value is $p$ , the probability of false must be $1 − p$, so we often omit the second number.





## References
1. Artificial Intelligence: A Modern Approach, 4th edition.