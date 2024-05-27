Once you have a workable model, having the right features tends to give them the biggest performance boost compared to clever algorithmic techniques such as hyper-parameter tuning.

## Learned Features vs Engineered Features

Why do we have to worry about feature engineering? Doesn’t deep learning promise us that we no longer have to engineer features?

 The promise of deep learning is that we won’t have to handcraft features. For this reason, deep learning is sometimes called feature learning.
 
 1. Many features can be automatically learned and extracted by algorithms. However, we’re still far from the point where all features can be automated.
 2. The majority of ML applications in production aren’t deep learning.

Feature engineering requires knowledge of domain-specific techniques. But much of the pain has been alleviate since the rise of deep learning.

==The process of choosing what information to use and how to extract this information into a format usable by your ML models is **feature engineering.**==

## Common Feature Engineering Operations

### Handling Missing Values
Not all types of missing values are equal. There are 3 types of missing values: 

1. Missing not at random(**MNAR**): The reason a value is missing is because of the true value itself. 
	 Example: Very high income individuals not disclosing their income
2. Missing at random(**MAR**): A value is missing not due to the value itself, but due to another observed variable. 
	Example: Age values missing for a particular gender 
3. Missing Completely at random(**MCAR**): There’s no pattern in when the value is missing. 
	Example: People just forgetting to fill their job role in an application

There are 2 ways to deal with missing values: Imputation and Deletion. 

But still remember, there is no perfect way to handle missing values.

- With deletion, you risk losing important information or accentuating biases.
- With imputation, you risk injecting your own bias into and adding noise to your data, or worse, data leakage.
#### Deletion

##### Column Deletion
If a variable has too many missing values, just remove that variable. 

**Drawback**: You might remove important information and reduce the accuracy of your model.
##### Row Deletion
If a sample has missing value(s), just remove that sample. This method can work when the missing values are completely at random (MCAR) and the number of examples with missing values is small, such as less than 0.1%. 

You don’t want to do row deletion if that means 10% of your data samples are removed.

**Drawback**: 
1. Removing rows of data can also remove important information that your model needs to make predictions, especially if the missing values are not at random (MNAR). 
2. Removing rows of data can create biases in your model, especially if the missing values are at random (MAR). 

#### Imputation
“Fill the missing data with certain values”
1. Filling with defaults
2. Filling with mean, median or mode

### Scaling
Before inputting features into models, it’s important to scale them to be similar ranges. This process is called feature scaling. 

This is one of the simplest things you can do that often results in a performance boost for your model. 

Neglecting to do so can cause your model to make gibberish predictions, especially with classical algorithms like gradient-boosted trees and logistic regression.

### Discretization
Discretization is the process of turning a continuous feature into a discrete feature. This process is also known as quantization or binning. This is done by creating buckets for the given values. 

Even though, by definition, discretization is meant for continuous features, it can be used for discrete features too. 

The downside is that this categorization introduces discontinuities at the category boundaries—$34,999 is now treated as completely different from $35,000, which is treated the same as $100,000. 

Choosing the boundaries of categories might not be all that easy. You can try to plot the histograms of the values and choose the boundaries that make sense. 

In general, common sense, basic quantiles, and sometimes subject matter expertise can help.

### Encoding Categorical Features

### Feature Crossing
Feature crossing is the technique ==to combine two or more features to generate new features==. This technique is useful to model the nonlinear relationships between features. 

Because feature crossing helps model nonlinear relationships between variables, it’s ==essential for models that can’t learn or are bad at learning nonlinear relationships, such as linear regression, logistic regression, and tree-based models==. 

It’s less important in neural networks, but it can still be useful because explicit feature crossing occasionally helps neural networks learn nonlinear relationships faster. 

A caveat of feature crossing is that it can make your feature space blow up. Imagine feature A has 100 possible values and feature B has 100 possible features; crossing these two features will result in a feature with 100 × 100 = 10,000 possible values. You will need a lot more data for models to learn all these possible values. ==Another caveat is that because feature crossing increases the number of features models use, it can make models overfit to the training data==.

### Discrete and Continuous Positional Embeddings

## Data Leakage

Data leakage refers to the phenomenon when a form of the label “leaks” into the set of features used for making predictions, and this same information is not available during inference.

Data leakage is challenging because often the leakage is nonobvious. It’s dangerous because it can cause your models to fail in an unexpected and spectacular way, even after extensive evaluation and testing.

### Common Causes

#### Splitting time-correlated data randomly instead of by time

In college, we are taught to randomly split my data into train, validation, and test splits. This is also how data is often reportedly split in ML research papers. 

However, this is also one common cause for data leakage. In many cases, ==data is time-correlated, which means that the time the data is generated affects its label distribution==.

When building models to predict the future stock prices, you want to split your training data by time, such as training your model on data from the first six days and evaluating it on data from the seventh day. If you randomly split your data, prices from the seventh day will be included in your train split and leak into your model the condition of the market on that day. We say that the future is leaked into the training process. 

However, in many cases, the correlation is nonobvious. Consider the task of predicting whether someone will click on a song recommendation. Whether someone will listen to a song depends not only on their music taste but also on the general music trend that day. If an artist passes away one day, people will be much more likely to listen to that artist. By including samples from a certain day in the train split, information about the music trend that day will be passed into your model, making it easier for it to make predictions on other samples on that same day.

> To prevent future information from leaking into the training process and allowing models to cheat during evaluation, split your data by time, instead of splitting randomly, whenever possible. 

![[Pasted image 20240527162117.png]]
#### Scaling before splitting
Scaling requires global statistics—e.g., mean, variance—of your data. One common mistake is to ==use the entire training data to generate global statistics before splitting it into different splits, leaking the mean and variance of the test samples into the training process==, allowing a model to adjust its predictions for the test samples. This information isn’t available in production, so the model’s performance will likely degrade.

To avoid this type of leakage, always split your data first before scaling, then use the statistics from the train split to scale all the splits. 

#### Filling in missing data with statistics from the test split
One common way to handle the missing values of a feature is to fill (input) them with the mean or median of all values present. Leakage might occur if the mean or median is calculated using entire data instead of just the train split. 

This type of leakage is similar to the type of leakage caused by scaling, and it can be prevented by using only statistics from the train split to fill in missing values in all the splits.

#### Poor handling of data duplication before splitting
If you have duplicates or near-duplicates in your data, failing to remove them before splitting your data might cause the same samples to appear in both train and validation/test splits.

Data duplication can also happen because of data processing— for example, oversampling might result in duplicating certain examples.

To avoid this, always check for duplicates before splitting and also after splitting just to make sure. If you oversample your data, do it after splitting.

#### Group Leakage
A group of examples have strongly correlated labels but are divided into different splits.

#### Leakage from Data Generation Process
Normalize your data so that data from different sources can have the same means and variances. And don’t forget to incorporate subject matter experts, who might have more contexts on how data is collected and used, into the ML design process!

### Detecting Data Leakage

Measure the predictive power of each feature or a set of features with respect to the target variable (label). If a feature has unusually high correlation, investigate how this feature is generated and whether the correlation makes sense. 

It’s possible that two features independently don’t contain leakage, but two features together can contain leakage. For example, when building a model to predict how long an employee will stay at a company, the starting date and the end date separately doesn’t tell us much about their tenure, but both together can give us that information.

Do ablation studies to measure how important a feature or a set of features is to your model. If removing a feature causes the model’s performance to deteriorate significantly, investigate why that feature is so important. 

If you have a massive amount of features, say a thousand features, it might be infeasible to do ablation studies on every possible combination of them, but it can still be useful to occasionally do ablation studies with a subset of features that you suspect the most. 

This is another example of how subject matter expertise can come in handy in feature engineering. Ablation studies can be run offline at your own schedule, so you can leverage your machines during downtime for this purpose.

Keep an eye out for new features added to your model. If adding a new feature significantly improves your model’s performance, either that feature is really good or that feature just contains leaked information about labels.

Be very careful every time you look at the test split. If you use the test split in any way other than to report a model’s final performance, whether to come up with ideas for new features or to tune hyperparameters, you risk leaking information from the future into your training process.

## Engineering Good Features

Having too many features can be bad both during training and serving your model for the following reasons:

1. More features you have, more opportunities for data leakage.
2. Too many features can cause overfitting.
3. Too many features can increase memory required to serve a model. 
4. Too many features can increase inference latency when doing online prediction, especially if you need to extract these features from raw data for predictions online. 
5. Useless features become technical debts. Whenever your data pipeline changes, all the affected features need to be adjusted accordingly. 

### Feature Importance
 If you use a classical ML algorithm like boosted gradient trees, the easiest way to measure the importance of your features is to use built-in feature importance functions implemented by XGBoost. For more model-agnostic methods, you might want to look into SHAP (SHapley Additive exPlanations).

The exact algorithm for feature importance measurement is complex, but intuitively, a feature’s importance to a model is measured by how much that model’s performance deteriorates if that feature or a set of features containing that feature is removed from the model. SHAP is great because it not only measures a feature’s importance to an entire model, it also measures each feature’s contribution to a model’s specific prediction. 

![[Pasted image 20240527163409.png]]
![[Pasted image 20240527163427.png]]
> Often, a small number of features accounts for a large portion of your model’s feature importance.

Not only good for choosing the right features, feature importance techniques are also great for interpretability as they help you understand how your models work under the hood.

### Feature Generalization

Since the goal of an ML model is to make correct predictions on unseen data, features used for the model should generalize to unseen data. Not all features generalize equally. 

Measuring feature generalization is a lot less scientific than measuring feature importance, and it requires both intuition and subject matter expertise on top of statistical knowledge. 

Overall, there are two aspects you might want to consider with regards to generalization: 
1. Feature coverage
2. Distribution of Feature Values.

Coverage is the percentage of the samples that has values for this feature in the data— so the fewer values that are missing, the higher the coverage.

For the feature values that are present, you might want to look into their distribution. If the set of values that appears in the seen data (such as the train split) has no overlap with the set of values that appears in the unseen data (such as the test split), this feature might even hurt your model’s performance.

Also, when considering a feature’s generalization, there’s a trade-off between generalization and specificity. 


