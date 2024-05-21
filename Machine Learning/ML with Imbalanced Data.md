## Sources
1. Asymmetric Cost: Imbalance due to different cost
2. Asymmetric Data: Imbalance due to samples

- Asymmetric cost is generally applicable when getting one class wrong is more costlier than the other class (spam detection, fraud detection).

## Why is this a Problem ?
Most machine learning algorithms assume data equally distributed. So when we have a class imbalance, ==the machine learning classifier tends to be more biased towards the majority class, causing bad classification of the minority class==.


## Training Classifiers with Imbalanced Data

### 1. Change Data

#### 1.1 Random Over-sampling
**Core idea**: Randomly duplicate examples in the minority class.

Random oversampling involves randomly selecting examples from the minority class, with replacement, and adding them to the training dataset.

#### 1.2 Random Under-sampling
**Core idea**: Randomly delete examples in the majority class

Random under-sampling involves randomly selecting examples from the majority class and deleting them from the training dataset.

#### Analysis of over and under sampling
Both the above approaches can be repeated until the desired class distribution is achieved in the training dataset, such as an equal split across the classes.

They are referred to as “==_naive resampling_==” methods because they assume nothing about the data and no heuristics are used. This makes them **simple to implement and fast to execute**, which is desirable for very large and complex datasets.

Both techniques can be used for two-class (binary) classification problems and multi-class classification problems with one or more majority or minority classes.

Importantly, the change to the class distribution is **only applied to the training dataset**. The intent is to influence the fit of the models. The resampling is not applied to the test or holdout dataset used to evaluate the performance of a model.


#### 1.4 Ensemble Resampling


#### 1.5 [[SMOTE]]




## References
1. https://machinelearningmastery.com/random-oversampling-and-undersampling-for-imbalanced-classification/
2. https://towardsdatascience.com/class-imbalance-a-classification-headache-1939297ff4a4





