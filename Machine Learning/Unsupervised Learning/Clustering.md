Clustering is the **process of grouping similar entities together**. Each similar group is called a **cluster** and contains statistically similar data points.

## Types

![[Pasted image 20240325181601.png]]

### Hard
- Each data point is assigned to exactly one cluster.
### Soft
- Each data point is assigned a probability or likelihood of being in a cluster.

## Distance measures

All of clustering is based on the _distance_ and _similarity_ between datapoints and/or clusters.

> Clustering searches for **similar data** points and that requires a metric or measure to compute a degree of similarity or dissimilarity of the samples.

The **choice of distance measure** is crucial in clustering. It defines how the similarity of two elements $(x, y)$ is calculated and influences the shape of the clusters; in reality there are hundreds of distance measures.

![[Pasted image 20240325181816.png]]

### Commonly used measures
The classical distance measures are **Euclidean** and **Manhattan distances**. For the most common clustering algorithms, the default distance measure is **Euclidean**.

- **Euclidean distance** is the shortest distance between two points in a straight line or "as the crow flies". It's like measuring the distance between two toys by drawing a straight line between them.
- **Manhattan distance**, on the other hand, measures the distance between two points as if you were walking along the streets of a city, where you can only move along the grid of streets.

| Distance Measure  | Formula                                                     |
| ----------------- | ----------------------------------------------------------- |
| Euclidean         | $d(x, y)=\sqrt{\sum_{i=1}^{m}\left(x_{i}-y_{i}\right)^{2}}$ |
| Manhattan         | $d(x, y)=\sum_{i}=1^{m}\left\|x_{i}-y_{i}\right\|$          |
| Chebyshev         | $d(x, y)=\max \left(\left\|x{i}-y{i} \right\|\right)$       |
| Squared Euclidean | $d(x, y)=\sum_{i=1}^{m}\left(x_{i}-y_{i}\right)^{2}$        |
| Canberra          | $d(x, y)=\sum \frac{\|x_i-y_i \mid}{\|x_i+y_i\|}$           |
## Clustering Algorithms
### Connectivity Models
Groups data into clusters and then merges the clusters based on distances to each other.

#### Algorithms
[[Agglomerative Hierarchical Clustering(AHC)]]
### Centroid Models
Groups data into clusters based on how close they are to the center point of each cluster.

#### Algorithms
[[K-Means clustering(Centroid model)]]

### Density Models
Groups data by finding **dense regions** of data points in one or more dimensions. 
#### Algorithms
[[DBSCAN Clustering(Density Model)]]
## [[Cluster Validation]]


## References
1. https://mlquant.notion.site/B-Clustering-7f2dba6285054267913459b9b4cc03de
2. 

