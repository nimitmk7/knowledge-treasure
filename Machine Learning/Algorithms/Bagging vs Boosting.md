- Bagging and Boosting are both ensemble learning techniques used to improve the performance of machine learning models. However, they differ in their approach and objectives. Here are the key differences between Bagging and Boosting:
- **Data Sampling:**
    - Bagging: In Bagging (short for Bootstrap Aggregating), multiple training datasets are created by randomly sampling from the original dataset with replacement. Each dataset is of the same size as the original dataset.
    - Boosting: In Boosting, the training datasets are also created by random sampling with replacement. However, each new dataset gives more weight to the instances that were misclassified by previous models. This allows subsequent models to focus more on difficult cases.
- **Model Independence:**
    - Bagging: In Bagging, each model is built independently of the others. They are trained on different subsets of the data and can be constructed in parallel.
    - Boosting: In Boosting, models are built sequentially. Each new model is influenced by the performance of previously built models. Misclassified instances are given higher weights, and subsequent models try to correct those errors.
- **Weighting of Models:**
    - Bagging: In Bagging, all models have equal weight when making predictions. The final prediction is often obtained by averaging the predictions of all models or using majority voting.
    - Boosting: In Boosting, models are weighted based on their performance. Models with better classification results are given higher weights. The final prediction is obtained by combining the weighted predictions of all models.
- **Objective:**
    - Bagging: Bagging aims to reduce the variance of a single model. It helps to improve stability and reduce overfitting by combining multiple models trained on different subsets of the data.
    - Boosting: Boosting aims to reduce the bias of a single model. It focuses on difficult instances and tries to correct the model’s mistakes by giving more weight to misclassified instances. Boosting can improve the overall accuracy of the model but may be more prone to overfitting.
- **Examples:**
    - Bagging: Random Forest is an extension of Bagging that uses decision trees as base models and combines their predictions to make final predictions.
    - Boosting: Gradient Boosting is a popular Boosting algorithm that sequentially adds decision trees to the model, with each new tree correcting the mistakes of the previous ones.
## References
1. https://vinija.ai/concepts/ml-comp/#model-ensembles
2. 