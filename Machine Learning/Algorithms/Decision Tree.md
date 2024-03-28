## Goal
Create a model that predicts the value of a target variable by learning simple decision rules inferred from the data features.
## Example
>[!INFO]  This example is for a classification scenario.
>

![[Pasted image 20240326155010.png]]
## Types
[[Decision Tree Regressor]]
[[Decision Tree Classifier]]

## Building the Tree
### CART algorithm
- Sci-kit learn uses the CART algorithm, which only produces binary trees.
- CART stands for Classification and Regression Trees(“growing trees”).
#### Concept
- The algorithm works by recursively partitioning the training data into smaller subsets using binary splits. The tree starts at the root node, which contains all the training data, and recursively splits the data into smaller subsets until a stopping criterion is met.

#### Algorithm
 1. At each node of the tree, the algorithm selects a feature($k$) and a threshold($t_k$) that best separates the training data into two groups, based on the values of that feature. 
	 - Cost Function: 
	 $$J(k, t_{k})= \frac {m_{left}}{m}G_{left}\space + \frac {m_{right}}{m}G_{right}  $$	
	This is done by choosing the feature and threshold that minimizes this cost function, thus producing the purest subsets possible.

2. The process continues recursively, with each node in the tree splitting the data into two smaller subsets, until a stopping criterion is met. 
3. The stopping criterion could be:
	1. maximum depth for the tree, 
	2. minimum number of instances in each leaf node
	3. maximum allowed leaf nodes
	4. minimum samples for split

> [!tip] The CART algorithm is a greedy algorithm: it greedily searches for an optimum split at the top level, then repeats the process at each level. It does NOT check whether or not the split will lead to the lowest possible impurity several levels down. A greedy algorithm often produces a reasonably good solution, but it is not guaranteed to be the optimal solution. Unfortunately, finding the optimal tree is known to be an NP-Complete problem: it requires an order of e-raised-to-the-m time, making the problem intractable even for fairly small training sets.

## Advantages
1. Simple to understand, interpret and visualize.
2. Requires little data preparation
3. Cost of Inference is $O(\log n)$ where $n$ is the number of data points.
4. Ability to handle both numerical and categorical data.
5. Ability to handle multi-output problem.
6. Uses a white-box model. The explanation for the condition is easily explained by boolean logic.
7. Possible to validate a model using statistical tests. That makes it possible to account for the reliability of the model.
8. Performs well even if its assumptions are somewhat violated by the true model from which the data were generated.

## Disadvantages
1. Decision-tree learners can create over-complex trees that do not generalize the data well. → **Overfitting**.
2. Decision trees can be unstable because small variations in the data might result in a completely different tree being generated. (**Tip**: Use **ensemble** to mitigate this issue)
3. The problem of learning an optimal decision tree is known to be NP-complete under several aspects of optimality and even for simple concepts. Consequently, practical decision-tree learning algorithms are based on heuristic algorithms such as the greedy algorithm where locally optimal decisions are made at each node. Such algorithms cannot guarantee to return the globally optimal decision tree. (**Tip**: Can be mitigated using an ensemble)
4.  Predictions of decision trees are neither smooth nor continuous, but piecewise constant approximations as seen in the above figure. Therefore, they are not good at extrapolation.
5. There are concepts that are hard to learn because decision trees do not express them easily, such as XOR, parity or multiplexer problems.
6. ==Decision tree learners create biased trees if some classes dominate. It is therefore recommended to balance the dataset prior to fitting with the decision tree==.

## Dealing with overfitting in trees
1. Pruning
2. Setting min. samples at leaf node
3. Setting max. depth of the tree

## Hyper-parameters

### Minimum samples at leaf node: 

The minimum sample leaf in a tree is the minimum number of samples required to be at a leaf node. A leaf node is a node that has no children. 

By setting a minimum number of samples required at a leaf node, we can prevent the tree from creating by creating branches with very few samples

For example, if we set the minimum sample leaf to 10, then the tree will not create a branch if there are fewer than 10 samples at that node. This can help to prevent the tree from learning too much from the training data and overfitting to it.

Lower value(e.g. 1): Tree can grow very deep and large
Higher value(e.g. 10): Limited size tree.

### Minimum Samples for split

### Minimum weight fraction of samples at leaf node

### Maximum Number of Leaf nodes

### Maximum Number of Features

## References
1. https://scikit-learn.org/stable/modules/tree.html
2. https://www.niser.ac.in/~smishra/teach/cs460/23cs460/lectures/lec9.pdf
3. 
