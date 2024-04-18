## Core Idea

> **Wisdom of the Crowd** : A large number of relatively uncorrelated models (trees) operating as a committee will outperform any of the individual constituent models.

The multiple models used, known as **weak learners**, are trained to solve the same problem, and are then combined to achieve optimal and more robust models, **strong learners**.

![[ensemble_learning.png]]

### Intuition
The low correlation between models is the key. Uncorrelated models can produce ensemble predictions that are more accurate than any of the individual predictions. 

**The reason for this wonderful effect is that the models protect each other from their individual errors** (as long as they don't constantly all err in the same direction). While some models may be wrong, many other models will be right, so as a group the trees are able to move in the correct direction. So the prerequisites for random forest to perform well are:

1. There needs to be some actual signal in our features so that models built using those features do better than random guessing.
2. The predictions (and therefore the errors) made by the individual models need to have low correlations with each other.

## Types of Ensemble Models

### Homogenous Ensemble
It is a collection of models that use the same base learning algorithm, built upon different training data. 
#### Examples
1. [[Boosting]]
2. [[Bagging]]

### Heterogenous Ensemble
It is a collection of models having different base learning algorithms, built upon same training data.

#### Examples
1. [[Stacking]]
