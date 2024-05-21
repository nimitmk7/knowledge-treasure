## Core Idea
 Boosting is an ensemble learning method whose goal is to **reduce the bias or training error** within a model.
 
Boosting also uses homogeneous weak learners, and each weak learner trains using a random sample of data from the dataset. The training of each weak learner is done **sequentially**, with the goal that each weak learner will **learn from, adapt, and try to compensate for its predecessors**.

In doing so, the weak learners are combined in a deterministic way, with the
weak rules from each individual learner ultimately combining to form one predictive ”strong” model with each iteration.

> [!tip] Boosting is prone to overfitting the training data and potentially have High Variance, hence the use of high bias, low variance weak learners is preferred for boosting.

![[Pasted image 20240416164129.png]]
## Assumptions
- Independence of observations
- While boosting can increase the accuracy of a base learner, it might come at the cost of intelligibility and interpretability
- Gradient tree boosting implementations often limit the minimum number of observations in trees’ terminal nodes. It is used in the tree-building process by ignoring any splits that lead to nodes containing fewer than this number of training set instances.
## Implementations
1. [[Gradient Boosting]]
2. [[AdaBoost]]


