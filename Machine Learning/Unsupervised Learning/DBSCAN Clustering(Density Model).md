## Premise
- DBSCAN (Density-based spatial clustering of applications with noise) is a popular clustering algorithm and finds clusters as regions of high density followed by regions of low density.
- Clusters found by DBSCAN can be of any shape, as opposed to k-means which works well if the clusters are spherical in shape.
- The benefit of this method is that it looks for _continuation of shapes_ and is therefore a great method for clustering samples based on non-linear shapes.
- DBSCAN does not require one to specify the number of clusters in the data a priori, as opposed to k-means.
- 
To use DBSCAN you first need to set two parameters: `epsilon` and `min_samples`. :

- **Epsilon** is the maximum distance that **two points** can be from each other to be considered part of the same cluster,
- While **min_samples** is the minimum number of points required to form a dense region.

## Algorithm

1. Start by initializing the cluster label `C` to -1, which will be used to mark noise.
2. Loop through each point in the dataset.
3. If the point has already been processed (labeled), skip it.
4. Find all points within the `epsilon` distance of the current point (`P`'s epsilon-neighborhood).
5. If the number of points within `P`'s epsilon-neighborhood is less than `min_samples`, label `P` as noise -1.
6. Otherwise, start a new cluster:
    - Label `P` with the new cluster label `C`.
    - Create a seed set with neighbors of `P` to check for expandability.
7. For each point `Q` in the seed set:
    - If `Q` is labeled as noise, change its label to the current cluster `C` (as it is part of a cluster now).
    - If `Q` is already labeled, skip it.
    - Label `Q` with the current cluster label `C`.
    - Find all points within `Q`'s epsilon-neighborhood.
    - If there are at least `min_samples` points, add those to the seed set (making sure we check all of them for expandability).
8. Repeat this process until all points in the seed set are checked.
9. After processing all points, return the dataset with labels indicating which points belong to the same cluster.

## Implementation

```Python
from sklearn.cluster import DBSCAN
import numpy as np
X = np.array([[1, 2], [2, 2], [2, 3],
              [8, 7], [8, 8], [25, 80]])
clustering = DBSCAN(eps=3, min_samples=2).fit(X)
clustering.labels_
## array([ 0,  0,  0,  1,  1, -1])
```

## Visual Comparison

![[Pasted image 20240325231207.png]]
## Pros
1. Don’t have to specify the number of clusters
2. Mostly returns the same output for every run.
3. Resistant to outliers

## Cons
1. Highly sensitive to its two parameters `Eps` and `Min` points
2. Struggles to cluster with large differences in ‘crowdedness’

