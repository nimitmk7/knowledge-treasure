## GINI Index
In classification trees, the Gini attribute is used instead of MSE.

A node’s Gini attribute measures its impurity: a node is “pure” (its Gini equals 0) if all training instances it applies to belong to the same class.
### Formulation

$$G_{i} = 1 - \sum_{k=1}^{n} {p_{i,k}} ^ 2$$
where $p_{i,k}$ is the ratio of class $k$ instances among the training instances in the $i^{th}$ node.

### Example
Node: Samples 54. 
Class 1: 0, Class 2: 49, Class 3: 5

$G = 1 – (0/54)^2 – (49/54)^2 – (5/54)^2 ≈ 0.168.$

### Interpretation

The Gini Index varies between 0 and 1, where **0 represents purity of the classification and 1 denotes random distribution of elements among various classes**.

A Gini Index of 0.5 shows that there is equal distribution of elements across some classes.

### GINI Score vs Entropy
- So should you use Gini impurity or entropy?

The truth is, most of the time it does not make a big difference: they lead to similar trees.

**Gini impurity is slightly faster to compute**, so it is a good default.

However, when they differ, Gini impurity tends to isolate the most frequent class in its own branch of the tree, while **entropy tends to produce slightly more balanced trees**.


## Probability Estimation
- A Decision Tree can also estimate the probability that an instance belongs to a particular class k:
1. It traverses the tree to find the leaf node for this instance
2. It returns the ratio of training instances of class k in this node.

## Hands-on example







