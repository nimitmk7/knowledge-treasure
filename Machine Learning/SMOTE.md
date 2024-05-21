SMOTE stands for **S**ynthetic **M**inority **O**versampling **Te**chnique. It is an advanced oversampling technique, which uses data augmentation. 

**Advantage**: Not generating duplicates, but creating synthetic data points that are slightly different from the original data points. 

### Algorithm

1. Draw a random sample from the minority class.
2. For observations in this random sample, you will identify the $k$ nearest neighbors.
3. Take one of those neighbors and identify the vector between the current data point and the selected neighbor.
4. Multiply the vector by a random number between 0 and 1.
5. Add this vector to the current data point, to obtain the synthetic data point.

**Core Idea**: Slightly moving the data point in the direction of its neighbor.

## Influence on Results

- Using SMOTE we can tweak the model to ==reduce false negatives, at the cost of increasing false positives==.
- Thus, SMOTE ==increases the recall at the cost of lower precision.== 
