## Premise
When we say that an ML model learns from the training data, it means that the model learns the underlying distribution of the training data with the goal of **leveraging this learned distribution to generate accurate predictions for unseen data**—data that it didn’t see during training. 

Its essential for the training data and the unseen data to come from a similar distribution. The assumption in Machine Learning Models is that the unseen data comes from a *stationary* distribution that is the *same* as training data distribution. If the unseen data comes from a different distribution, the model might not generalize well.

This assumption is incorrect in most cases for 2 reasons.

1. Underlying distribution of the real-world data is unlikely to be the same as the underlying distribution of the training data.
	1. Curating a training dataset that can accurately represent the data that a model will encounter in production turns out to be very difficult. Real-world data is multifaceted and, in many cases, virtually infinite, whereas training data is finite and **constrained by the** **time**, **compute**, **and** **human resources available** during the dataset creation and processing
2. Real world isn’t stationary. Things change. Data distributions shift.


> [!NOTE] POOR PRACTICES
> Due to the complexity of ML systems and the poor practices in deploying them, a large percentage of what might look like data shifts on monitoring dashboards are caused by internal errors, such as 
> 1. Bugs in the data pipeline
> 2. Missing values incorrectly inputted
> 3. Inconsistencies between the features extracted during training and inference
> 4. Features standardized using statistics from the wrong subset of data.
> 5. Wrong model version
> 6. Bugs in the app interface that force users to change their behaviors.

## Definition

Data distribution shift refers to the phenomenon in supervised learning when **the data a model works with changes over time**, which causes this model’s predictions to become less accurate as time passes. 

The distribution of the data the model is trained on is called the source distribution. The distribution of the data the model runs inference on is called the target distribution. When these drift apart, model predictions run into trouble.

## General Data Distribution Shifts

1. **Feature Change**
	- New features are added
	- Older features are removed
	- Set of all possible values of a feature changes
2. **Label Schema Change**
	- Set of possible values for Y change
	- Regression Example: Credit Score system changes range from 250 to 900, instead of older system of 300 to 850
	- Classification Example: {POSITIVE, NEGATIVE, NEUTRAL} → {POSITIVE, ANGRY, SAD, NEUTRAL}
	- Label schema change is especially common with high-cardinality tasks–tasks with a high number of classes–such as a product or documentation categorization.

There’s no rule that says that only one type of shift should happen at one time. A model might suffer from multiple types of drift, which makes handling them a lot more difficult.

## Detecting DDS 
DDSs are a problem only if they cause our model’s performance to degrade. So, first idea is to monitor your model’s accuracy-related metrics in production to see whether they have changed. 

> “Change” here usually means decrease, but if my model’s accuracy suddenly goes up or fluctuates significantly for no reason that I’m aware of, I’d want to investigate. 

Accuracy-related metrics work by comparing the model’s predictions to ground truth labels. During model development, you have access to labels, but in production, you don’t always have access to labels, and even if you do, labels will be delayed. 

==Having access to labels within a reasonable time window will vastly help with giving you visibility into your model’s performance==.

When ground truth tables are unavailable or too delayed to be useful, we can monitor other distributions of interest. 
1. Input distribution $P(X)$
2. Label distribution $P(Y)$
3. Conditional distributions $P(Y|X)\space \text{and} \space P(X|Y)$

While we don’t need to know the ground truth labels Y to monitor the input distribution, monitoring the label distribution and both of the conditional distributions require knowing Y.

However, in the industry, most drift detection methods ==focus on detecting changes in the input distribution==, especially the distributions of features.

### Statistical Methods

#### Simple statistics comparison
A simple method many companies use to detect whether the 2 distributions are the same is to compare their statistics: 
1. Minimum
2. Maximum
3. Mean
4. Median
5. Variance
6. Quantiles(5th, 25th, 75th and 95th quantiles)
7. Skewness
8. Kurtosis

But these metrics are far from sufficient. Mean, median, and variance are only useful with the distributions for which the mean/median/variance are useful summaries. 

If those metrics differ significantly, the inference distribution might have shifted from the training distribution. However, ==if those metrics are similar, there’s no guarantee that there’s no shift==.

#### Two Sample hypothesis test

It’s a test to determine whether the difference between two populations (two sets of data) is statistically significant. If the difference is statistically significant, then the probability that the difference is a random fluctuation due to sampling variability is very low, and, therefore, the difference is caused by the fact that these two populations come from two distinct distributions. 

> - Caveat is that just because the difference is statistically significant doesn’t mean that it is practically important. 
> 
> - However, a good heuristic is that if you are able to detect the difference from a relatively small sample, then it is probably a serious difference. 
>
> - If it takes a huge number of samples to detect, then the difference is probably not worth worrying about. 

> [!WARNING] 
> Because two-sample tests often work better on low-dimensional data than on high dimensional data, it’s highly recommended that you reduce the dimensionality of your data before performing a two-sample test on it. 

### Time Scale window methods
Shifts can also happen across two dimensions: **spatial** or **temporal**.

To detect temporal shifts, a common approach is to treat input data to ML applications as time-series data. When dealing with temporal shifts, ==the time scale window of the data we look at affects the shifts we can detect==. If your data has a weekly cycle, then a time scale of less than a week won’t detect the cycle.

![[Pasted image 20240517012927.png]]

There are 2 types of running statistics over time: 
1. Sliding statistics
2. Running statistics

Sliding statistics are computed within a single time scale window. Cumulative statistics are continually updated with more data. This means, for the beginning of each time scale window, the sliding accuracy is reset, whereas the cumulative sliding accuracy is not. Because cumulative statistics contain information from previous time windows, they might obscure what happens in a specific time window.
![[Pasted image 20240517013232.png]]
Shorter time-scale window → Faster detection of changes, but false alarms of shifts more probable.

## Addressing DDS
There are 3 main approaches when it comes to making a model work with a new distribution in production: 

#### Train models using massive datasets
The hope here is that if the training dataset is large enough, the model will be able to learn such a comprehensive distribution that whatever data points the model will encounter in production will likely come from this distribution. This is the approach that dominates research.
#### Adapt a trained model to a target Distribution without requiring new labels
Still in the works in research, not found wide adoption in Industry.

#### Retrain your model using labelled data from the target distribution
1. **Stateless retraining**: Retrain from scratch on both old and new data
2. **Stateful retraining**: Continue training existing model on new data.(Fine tuning)

Addressing data distribution shifts doesn’t have to start after the shifts have happened. It’s possible to design your system to make it more robust to shifts. A system uses multiple features, and different features shift at different rates.

1. When choosing features for your models, you might want to consider the tradeoff between the performance and stability of a feature: a feature might be really good for accuracy but deteriorate quickly, forcing you to train your model more often.
2. Design your system to make oit




