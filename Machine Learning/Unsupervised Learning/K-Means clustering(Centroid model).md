## Premise
- Unlike Hierarchical clustering, with K-means we have to pre-specify the quantity of clusters, K-means start with a random initialization of centroids based on the number of clusters.
- We have a clustering function and $c^{(i)}$ is the assigned cluster for data point $i$. The center of cluster $j$ is indicated as $\mu_{j}$ and the feature values are $x_{i} \in \mathbb{R}^{d}$.
## Algorithm
After randomly initializing the cluster centroids $\mu_{1}$, $\mu_{2}$, $\ldots$, $\mu_{k} \in \mathbb{R}^{n}$, the k-means algorithm repeats the following step until convergence:
$$
c^{(i)}=\arg \min _{j}\left\|x^{(i)}-\mu_{j}\right\|^{2} \text{and } \space \mu_{j}=\frac{\sum_{i=1}^{m} 1_{\left\{c^{(i)}=j\right\}} x^{(i)}}{\sum_{i=1}^{m} 1_{\left\{c^{(i)}=j\right\}}}
$$
>The basic idea behind k-means clustering is to define clusters and their centroids such that the total intra-cluster variation is minimized.

To minimize this function we have as simple jobs → assign all the datapoints to a cluster.

- Randomly assign values to $k$ cluster centroids
- Assign _every datapoint_ $i$ to its closest cluster centroid $\mu_j$.
- When all datapoint have been assigned, the cluster centroid value has to be recalculated.
- The new centroid is simply the average of all the datapoints assigned to the cluster.
- Slowly but surely the centroid would move to the _center of a dense area_ (many datapoints).

It is therefore an iterative algorithm that is slowly moving towards a stable solutions:

![[Pasted image 20240325230013.png]]
We stop the algorithm when the sum or mean of square errors between all the datapoints and their assigned cluster average (centroid) stops improving.
$$J(c, \mu)=\sum_{i=1}^{m}\left\|x^{(i)}-\mu_{c^{(i)}}\right\|^{2}$$
![[knn_graph_gif.gif]]
> [!WARNING] A known problem with k-means is that because of the random centroid initialization there are thousands if not millions of solutions, as such we are likely to obtain a local and not a global minimum.
### Step-by-Step
$\mathrm{K}$ is a hyperparameter of the algorithm and the k-means algorithm can be summarized as follows:

1. Specify the number of clusters $K$ to be created (to be specified by users).
2. Select $K$ data points randomly from the dataset as the _initial_ cluster centers or means.
3. Assign each data point to their closest centroid, based on the Euclidean distance between a data point and its centroid.
4. For each of the $K$-clusters update cluster centroid by calculating the new mean values of all the data points in the cluster.
5. Iteratively minimize the total within the sum of squares: iterate steps 3 and 4 until the cluster assignments stop changing or the maximum number of iterations is reached.

![[Pasted image 20240325230629.png | 500]]

 > [!summary] The centroids that minimize the cost function are learned through an iterative process of assigning data points to clusters and then moving the clusters.

## Hands-on implementation

```Python
from sklearn.cluster import KMeans
import numpy as np
X = np.array([[1, 2], [1, 4], [1, 0],
             [10, 2], [10, 4], [10, 0]])
kmeans = KMeans(n_clusters=2, random_state=0).fit(X)

kmeans.labels_
kmeans.predict([[0, 0], [12, 3]])
kmeans.cluster_centers_

## array([1, 0], dtype=int32)
## array([[10.,  2.],
       [ 1.,  2.]])
## array([1, 1, 1, 0, 0, 0], dtype=int32)
```

## Advantages
1. Simple to implement and understand
2. Fast and Scalable
3. Guaranteed Convergence
## Disadvantages
1. Need to know "K".
2. Returns different clusters each run due to random initialization.
3. Sensitive to outliers, so suffers when there is noise in the data.
4. Does not scale to high dimension.
5. Not ideal for datasets with non-convex shapes

## Use Case
- An efficient method to develop a small number of clusters
## Variants
1. Lloyd’s algorithm
2. K-Medians algorithm

## References
1. https://mlquant.notion.site/B-Clustering-7f2dba6285054267913459b9b4cc03de
2. 