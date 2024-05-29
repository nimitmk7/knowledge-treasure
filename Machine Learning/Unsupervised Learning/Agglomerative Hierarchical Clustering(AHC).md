## Introduction
Agglomerative Hierarchical Clustering (AHC) is ==an iterative classification method that groups objects into clusters based on their similarity==. 

1. Clustering method creates a hierarchy of clusters.
2. It starts with all the data points assigned to clusters of their own.
3. Then, the two nearest clusters are merged into the same cluster.
4. In the end, the algorithm terminates when there is only one cluster left.

## Formal Steps
1. In the beginning, every data point is its own cluster which means that we have `N` clusters at the beginning of the algorithm for a dataset of size `N`.
2. The distance between all clusters is calculated and two closest clusters are merged together to form a new cluster.
3. A new distance matrix is formed and the point which is closest to the cluster formed in step 2, will be merged again.
4. Steps 2 and 3 are repeated until one large cluster is created.
![[Pasted image 20240325195831.png]]
- In the Dendrogram, every merge of one cluster with another is represented by a horizontal distance line.
- The horizontal line (y-axis) shows the distance between the clusters as soon as they are merged, e.g., when cluster $6$ and $11$ merge, the [euclidean distance](https://online.stat.psu.edu/stat505/lesson/14/14.4) between them is $22.$
- The fewer clusters you have the larger the distance between datapoints in the cluster.
- Intuitively, a cluster of the two closest datapoints are closer to each other than the ten closest datapoints.

> [!tip] 
> When there are many clusters (at the bottom) we expect the average distance between points to be small, whereas with a small number of clusters (near the end) we expect the distance to increase.
> 
## Visual Example
![[6-what-is-clustering.gif]]

## Determining Cluster quality
**How many clusters should we select?**

A number of criteria can be used to determine the cutting point, like the **largest jump distance :**


![[Pasted image 20240325200252.png|300]]

- It selects the number of clusters where the vertical jump in the Dendrogram covers the largest distance, with our previous example that would be **4 clusters**.
- You identify the largest jump and draw a line through it, and you select the four clusters **below** the line.

![[Pasted image 20240325200349.png | 500]]
- **Process**: Cut the dendrogram where the gap between two successive combination similarities is largest. Such large gaps arguably indicate "natural" clustering.
- **Rationale**: Adding one more cluster decreases the quality of the clustering significantly, so cutting before this steep decrease occurs is desirable.
- **Similar**: This strategy is analogous to looking for the elbow in the $K$-means graph.

- _Like everything we have seen before, there are not just one method, but **multiple methods** to select the number of clusters_
    
    (1) **Pre-specified Similarity/Distance**
    
    Cut at a prespecified level of similarity. For example, we cut the dendrogram at $0.4$ if we want clusters with a minimum combination similarity of $0.4$.
    
    Cutting the diagram at $y=0.4$ yields 4 clusters (grouping only documents with high similarity together).
    
    (2) **Pre-specify Quantity of Clusters**
    
    We can also, if we want to preselect the number of clusters $K$ and have the cutting point where $K$ clusters are produced.

## Horizontal Lines
- There are **different types** of hierarchical clustering algorithms that aims at optimizing different objective functions.
- These methods define different **between-cluster distances** (horizontal line); it is essentially _cluster distance metrics_.

### Methods
![[Pasted image 20240325200758.png|500]]
**Average Linkage**: Average of the distances from points in one cluster to the other.
**Single Linkage**: Distance between the closest points in each cluster.
**Complete Linkage**: Distance between the furthest points in each cluster.

> [!NOTE] NOTE
> This doesn’t matter at the bottom of the clustering operation, as we start off with only one datapoint.

Four methods are summed up in the table below:

| Single Linkage                                                                     | Average Linkage                                                                                                       | Complete Linkage                                                                        | Ward Linkage                                                                                                                                          |
| ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nearest neighbor                                                                   | Between-group average linkage                                                                                         | Furthest neighbor                                                                       | Increased sum of squares                                                                                                                              |
| Proximity between two clusters is the proximity between their two closest objects. | Proximity between two clusters is the arithmetic mean of all the proximities between the objects of the two clusters. | Proximity between two clusters is the proximity between their two most distant objects. | Proximity is the difference between the joint and seperated sum of square from centroid in these two clusters: $S_{12}-\left(S S_{1}+S S_{2}\right)$. |
| Minimize minimum distance of between cluster pairs.                                | Minimize average distance between cluster pairs.                                                                      | Minimize maximum distance of between cluster pairs.                                     | Minimize within cluster distance of cluster pair                                                                                                      |

#### Observations:
1. Single linkage is fast, and can perform well on non-circular data, but it performs poorly in presence of noise.
2. Average and complete linkage perform well on cleanly separated circular clusters, but have mixed results otherwise.
3. Ward is the most effective method for noisy data.

#### **AHC + Difference Cluster Distance Metrics**
Reference: [Comparing different hierarchical linkage methods on toy dataset](https://scikit-learn.org/stable/auto_examples/cluster/plot_linkage_comparison.html)
![[Pasted image 20240325210851.png]]

## Pros
1. Always returns the same clusters with the same assignment.
2. Can handle any form of distance and similarity measure.
3. Ward’s algorithm normally generates equally sized clusters
## Cons
1. Computationally Expensive
2. Can’t handle outliers
