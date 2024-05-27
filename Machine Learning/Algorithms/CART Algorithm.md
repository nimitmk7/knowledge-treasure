 CART stands for Classification and Regression Trees(“growing trees”). Python package Sci-kit learn uses this algorithm, which only produces binary trees.
#### Concept
- The algorithm works by recursively partitioning the training data into smaller subsets using binary splits. The tree starts at the root node, which contains all the training data, and recursively splits the data into smaller subsets until a stopping criterion is met.

## Gini Index
$$
G = 1 - \sum_{i=1}^C(p_{i})^2
$$

where $p_i$ is the probability that a tuple in dataset belongs to class $i$. The sum is computed over all C classes.

It favors larger partitions, whereas information gain favors smaller partitions with distinct values. 

#### Algorithm
 1. At each node of the tree, the algorithm selects a feature($k$) and a threshold($t_k$) that best separates the training data into two groups, based on the values of that feature. 
	 - Cost Function: 
	 $$J(k, t_{k})= \frac {m_{left}}{m}G_{left}\space + \frac {m_{right}}{m}G_{right}  $$
	
	This is done by choosing the feature and threshold that minimizes this cost function, thus producing the purest subsets possible.
	Here $G$ stands for Gini Index.

2. The process continues recursively, with each node in the tree splitting the data into two smaller subsets, until a stopping criterion is met. 
3. The stopping criterion could be:
	1. maximum depth for the tree, 
	2. minimum number of instances in each leaf node
	3. maximum allowed leaf nodes
	4. minimum samples for split


 The CART algorithm is a greedy algorithm: it greedily searches for an optimum split at the top level, then repeats the process at each level. It does NOT check whether or not the split will lead to the lowest possible impurity several levels down. 
 
 A greedy algorithm often produces a reasonably good solution, but it is not guaranteed to be the optimal solution. Unfortunately, finding the optimal tree is known to be an ==NP-Complete problem==: it requires an order of $e^m$ time, making the problem intractable even for fairly small training sets.

