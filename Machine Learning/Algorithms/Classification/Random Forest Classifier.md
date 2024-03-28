## Motivation
Decision Trees work great with the data used to create them, but they are not flexible enough when it comes to classifying new samples.
## Core idea

> Wisdom of the Crowd : **A large number of relatively uncorrelated models (trees) operating as a committee will outperform any of the individual constituent models.**

### Reasoning
The low correlation between models is the key. Uncorrelated models can produce ensemble predictions that are more accurate than any of the individual predictions. 

**The reason for this wonderful effect is that the trees protect each other from their individual errors** (as long as they don't constantly all err in the same direction). While some trees may be wrong, many other trees will be right, so as a group the trees are able to move in the correct direction. So the prerequisites for random forest to perform well are:

1. There needs to be some actual signal in our features so that models built using those features do better than random guessing.
2. The predictions (and therefore the errors) made by the individual trees need to have low correlations with each other.

Watch: 
<iframe width="560" height="315" src="https://www.youtube.com/embed/J4Wdy0Wc_xQ?si=ebewJJZHGK3X_Qey" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/sQ870aTKqiM?si=dkDnMjA9DpfFuFpV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Procedure
1. Create a bootstrapped dataset.
	- Randomly select samples from the original dataset.
	- We're allowed to pick the same sample more than once.
2. Create a decision trees using the bootstrapped dataset, but only use a random subset of variables at each step.
3. Repeat steps 1,2 till you get $n$ trees.
## Inference
For each new data point, We pass it through all n decision trees, and then note their result(a.k.a **vote**). Final result is the class with maximum votes.

## Bagging
**B**ootstrapping the Data + **Agg**regating the Result = **Bagging**

## Model Evaluation 
- Use the out-of-bag data from the dataset. 
- Out of bag error = % OOB samples incorrectly classified.

## Missing Data and Clustering


## Pros
1. Less prone to overfitting
## Cons
1. Low interpretability

## Use Case
1. When the model is overfitting and you want better generalization.
## References
1. [Understanding Random Forest - Towards Data Science](https://towardsdatascience.com/understanding-random-forest-58381e0602d2)
2. https://vinija.ai/concepts/ml-comp/#model-ensembles 
3. 