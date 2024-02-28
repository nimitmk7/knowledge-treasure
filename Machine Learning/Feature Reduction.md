In general, there are 2 approaches to extracting a subset of features:
1. Dimensionality Reduction using **Feature Extraction**
2. Dimensionality Reduction using **Feature Selection**

## Feature Extraction
- Unsupervised
- Output reduced features are not labelled, and not in the least resemble that of the original candidate features.
- Meaning of output features not discernible, but the predictive power almost as same as of the candidate features.
- Techniques:
	- [[Principal Component Analysis]]
	- 
## Feature selection
- Supervised
- Output features conserve the original contents, meaning that it will be possible to continue to use the old labels fro these features and to use them for drawing inferences, etc.
### Strategies
#### Univariate statistics
- **Criteria for selection**: statistically significant relationship between the feature and the target
- Test is run on all features, and the features that are related to the target with the highest confidence are selected.
- **Key property**: They are univariate, meaning that they only consider each feature individually. Consequently, a feature will be discarded if it is only informative when combined with another feature
- **Advantage**: Often very fast to compute and don’t require building a model.
- Feature selection is valid regardless of the model applied afterward.
- **Examples**: ANOVA(f-statistic), Mutual Information
- **Use-case**: Helpful if there are such a large number of features that building a model on them is infeasible, or if you suspect that many features are completely uninformative.
#### Model-based 
- Uses a supervised machine learning model to judge the importance of each feature and keeps only the most important ones.
- Model-based selection considers all features at once, and so can capture interactions (if the model can capture them).
- Finally, these model-based methods select the best feature subset as part or as an extension of a learning algorithm’s training process
- **Example**: Decision tree based models, L1 regularization in linear regression

### Iterative
 - Builds a model to select features, but instead of building one model, it builds many models and uses **trial and error** to find the subset of features that produce models with the highest quality predictions.
 - In iterative feature selection, a series of models is built, with varying numbers of features.
 - There are two basic methods:
	 1. Starting with no features and Adding features one by one, until some stopping criterion is reached.[Bottom -up]
	 2. Starting with all features and Removing features one by one, until some stopping criterion is reached.[Top-down]

- Much more computationally expensive
- Yields more precise calculations.
- **Example**: RFE(Recursive Feature Elimination)

#### Univariate Methods
- The first problem we want to address in one where you have a set of numerical features and want to remove those with low variance (i.e., those features that are likely containing little information). 
- **Core idea**: Features with low variance are likely less interesting (and useful) than features with high variance.
- Steps:
	1. Calculate the variance of each feature
	2. Drop all features whose variance is less than threshold.

### Removing irrelevant features for Classification(Categorial Features)

- We used the Chi-square statistic, which examines the independence of 2 categorical vectors. 
$$
\chi^2 = \Sigma_{i=1}^n \frac{(O_i - E_i)^2}{E_i}
$$
- By calculating the chi-squared statistic between a feature and the target vector, we obtain a measurement of the independence between the two.
- If the target is independent of the feature variable, then it is irrelevant for our purposes because it contains no information we can use for classification.
- On the other hand, if the two features are highly dependent, they likely are very informative for training our model.
- To use chi-squared in feature selection, we calculate the chi-squared statistic between each feature and the target vector, then select the features with the best chi-square statistics.
- For better understanding and an example, refer:[ Towards Data Science - Chi-square test for feature selection](https://towardsdatascience.com/chi-square-test-for-feature-selection-in-machine-learning-206b1f0b8223)
- **Limitation**: Chi-Square is sensitive to small frequencies in cells of tables. Generally, when the expected value in a cell of a table is less than 5, chi-square can lead to errors in conclusions.
