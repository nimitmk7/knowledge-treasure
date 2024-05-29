Model development is an iterative process. After each iteration, you’ll want to compare your model’s performance against its performance in previous iterations and evaluate how suitable this iteration is for production. 

## Model Development and Training

### Evaluating ML Models
There are many possible solutions to any given problem. Given a task that can leverage ML in its solution, you might wonder what ML algorithm you should use for it. 

If you had unlimited time and compute power, the rational thing to do would be to try all possible solutions and see what is best for you. However, time and compute power are limited resources, and you have to be strategic about what models you select.

When selecting a model for your problem, you don’t choose from every possible model out there, but usually focus on a set of models suitable for your problem.

1. Different types of algorithms require different numbers of labels as well as different amounts of compute power(**Data and Compute requirements**). Some take longer to train than other(**training latency**), whereas some take longer to make predictions(**inference latency**). 
2. Non-neural network algorithms tend to be more explainable (e.g., what features contributed the most to an email being classified as spam) than neural networks.(**Interpretability**)

#### Tips for Model Selection
1. **Avoid the state-of-the-art trap**:
	1. If there’s a solution that can solve your problem that is much cheaper and simpler than state-of-the-art models, use the simpler solution.
2. **Start with the simplest models**:
	1. Simpler Models are easier to deploy, and deploying your model early allows you to validate that your prediction pipeline is consistent with your training pipeline.
	2. Starting with something simple and adding more complex components step- by-step makes it easier to understand your model and debug it.
	3. The Simplest model serves as a baseline to which you can compare your more complex models.
3. **Avoid human biases in selecting models:** 
	1. If an engineer is more excited about an architecture, they will likely spend a lot more time experimenting with it, which might result in better-performing models for that architecture.
	2. When comparing different architectures, it’s important to compare them under comparable setups.
4. **Evaluate good performance now versus good performance later:**
	1. The best model now does not always mean the best model two months from now.
	2. A simple way to estimate how your model’s performance might change with more data is to use learning curves. A learning curve of a model is a plot of its performance—e.g., training loss, training accuracy, validation accuracy—against the number of training samples it uses. ![[Pasted image 20240527212610.png]]
	3. While evaluating models, you might want to take into account their potential for improvements in the near future, and how easy/difficult it is to achieve those improvements.
5. **Evaluate trade-offs**
	1. Understanding what’s more important in the performance of your ML system will help you choose the most suitable model.
	2. False positive vs False negative tradeoff, compute requirement vs accuracy tradeoff, interpretability vs performance tradeoff are some of the common examples.
6. **Understand your model’s assumptions**
	1. Understanding what assumptions a model makes and whether our data satisfies those assumptions can help you evaluate which model works best for your use case.
	2. Common Assumptions:
		1. Prediction Assumption
		2. IID
		3. Smoothness
		4. Tractability
		5. Boundaries
		6. Conditional Independence
		7. Normally distributed

### Ensembles
One method that has consistently given a performance boost is to use an ensemble of multiple models instead of just an individual model to make predictions. Each model in the ensemble is called a base learner.

==Ensembling methods are less favored in production because ensembles are more complex to deploy and harder to maintain==. However, they are still common for tasks where a small performance boost can lead to a huge financial gain, such as predicting click-through rate for ads.

> [!INFO] Base learners in Ensemble
> 
> When creating an ensemble, the less correlation there is among base learners, the better the ensemble will be. Therefore, it’s common to choose very different types of models for an ensemble. 

#### Bagging
![[Pasted image 20240527213212.png]]
Bagging, shortened from bootstrap aggregating, is designed to improve both the training stability and accuracy of ML algorithms. It reduces variance and helps to avoid overfitting.

Given a dataset, instead of training one classifier on the entire dataset, you sample with replacement to create different datasets, called bootstraps, and train a classification or regression model on each of these bootstraps. Sampling with replacement ensures that each bootstrap is created independently from its peers.

Bagging generally improves unstable methods, such as neural networks, classification and regression trees, and subset selection in linear regression. 

However, it can mildly degrade the performance of stable methods such as k-nearest neighbors.

Refer to [[Bagging]] for more details.
#### Boosting
Boosting is a family of iterative ensemble algorithms that convert weak learners to strong ones. 

Each learner in this ensemble is trained on the same set of samples, but the samples are weighted differently among iterations. 

As a result, future weak learners focus more on the examples that previous weak learners misclassified.

![[Pasted image 20240527213738.png]]

#### Stacking

Stacking means that you train base learners from the training data then create a meta-learner that combines the outputs of the base learners to output final predictions. 

The meta-learner can be as simple as a heuristic: you take the majority vote (for classification tasks) or the average vote (for regression tasks) from all base learners. It can be another model, such as a logistic regression model or a linear regression model. 

![[Pasted image 20240527213916.png]]

