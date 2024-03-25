## Motivation
In machine learning, a model's performance relies heavily on its hyper-parameters. These are settings that control the training process itself. So finding the correct hyper-parameters is crucial for the learning process. 

## Benefits
1. Optimize Model Performance
2. Generalization(Over-fitting vs Under-fitting)
3. Model Stability
4. Resource Efficiency
5. Domain-Specific Considerations

## Methods

>[!INFO] What does CV stand for in these method names ?
> CV stands for Cross Validation, which is the approach used by these methods to determine the score of a model for a given set of hyper-parameters.

### GridSearchCV

It runs through all the different parameters that is fed into the parameter grid by the user and produces the best combination of parameters, based on a scoring metric of given choice (accuracy, f1, etc).

Refer documentation: [GridSearchCV - Sci-kit Learn](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)
#### Implementation
```Python
from sklearn.model_selection import GridSearchCV  
from sklearn.neighbors import KNeighborsClassifier  
from sklearn.pipeline import Pipeline  
from sklearn.preprocessing import MinMaxScaler

knn_pipe = Pipeline([('mms', MinMaxScaler()),  
                     ('knn', KNeighborsClassifier())])
                     
params = [{'knn__n_neighbors': [3, 5, 7, 9],  
         'knn__weights': ['uniform', 'distance'],  
         'knn__leaf_size': [15, 20]}]

gs_knn = GridSearchCV(knn_pipe,  
                      param_grid=params,  
                      scoring='accuracy',  
                      cv=5)  
                      
gs_knn.fit(X_train, y_train)  
```

```Python
gs_knn.best_params_

Output:  
{'knn__leaf_size': 15, 'knn__n_neighbors': 5, 'knn__weights': 'distance'}

# find best model score  
gs_knn.score(X_train, y_train)

Output:  
0.84035137
```

The exhaustive search identified the best parameters for our K-Neighbors Classifier to be `leaf_size=15`, `n_neighbors=5`, and `weights='distance'`. This combination of parameters produced an accuracy score of 0.84. Before improving this result, let’s break down what `GridSearchCV` did in the block above.

1. **_estimator_**: estimator object being used.
2. **_param_grid_**: dictionary that contains all of the parameters to try
3. **_scoring_**: evaluation metric to use when ranking results 
4. **_cv_**: cross-validation, the number of cv folds for each combination of parameters

The estimator object, in this case `knn_pipe`, must be scaled accordingly, based on the distribution of the dataset as well as the type of classifier being used. The scoring metric can be any metric of your choice. However, just like the estimator object, the scoring metric should be chosen based on what type of problem the project is trying to solve. The other two parameters in the grid search is where the limitations come in to play.
#### Pros
1. Easy implementation
#### Cons
1. Limited by the parameter choices provided by the user provides
2. Time-consuming in nature
3. Not flexible enough to test one more extra choice, need to run the full function again.

### RandomizedSearchCV

In randomized search, instead of specifying a grid of values, you can define probability distributions or ranges for each hyperparameter. Thus we have a much larger hyper-parameter search space to explore.

Randomized search then _randomly samples_ a fixed number of combinations of hyper-parameters from these distributions. This allows randomized search to explore a diverse set of hyper-parameter combinations efficiently.

Refer documentation: [RandomizedSearchCV - Sci-kit learn](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html)


![[Pasted image 20240325083858.png]]
#### Implementation
Let's tune the parameters of the baseline SVM classifier using randomized search.

We import the `RandomizedSearchCV` class and define `param_dist`, a much larger hyperparameter search space.
```python
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import uniform

param_dist = {
    'C': uniform(0.1, 10), # Uniform distribution between 0.1 and 10
    'kernel': ['linear', 'rbf', 'poly'],
    'gamma': ['scale', 'auto'] + list(np.logspace(-3, 3, 50))
}

```
Similar to grid search, we instantiate the randomized search model to search for the best hyper-parameters. Here, **we set `n_iter` to 20; so 20 random hyper-parameter combinations will be sampled**.

```Python
# Create the RandomizedSearchCV object
randomized_search = RandomizedSearchCV(estimator=baseline_svm, param_distributions=param_dist, n_iter=20, cv=5)

randomized_search.fit(X_train, y_train)
```




## References
1. [GridSearchCV for Beginners - Towards Data Science](https://towardsdatascience.com/gridsearchcv-for-beginners-db48a90114ee)
2. [Hyperparameter Tuning: GridSearchCV and RandomizedSearchCV, Explained](https://www.kdnuggets.com/hyperparameter-tuning-gridsearchcv-and-randomizedsearchcv-explained)
