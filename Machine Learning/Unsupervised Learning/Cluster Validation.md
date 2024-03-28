We want to have a method to measure the quality of the cluster model we developed, given the limitations:

- True labels (ground truth) are unknown
- We can’t use standard error metrics!

So how can we?

1. Select the **best clustering** **method**.
2. Select the optimal **number of** **clusters** (and other hyper-parameters).

## Validation Methods

### External Validation
- Use expert opinion and ground truth data.
#### Rand Index
"External validation" can mean various things:

**Known Classifications**: It could mean that you know what the “true” classification should be, let’s imagine a credit classification problem:

> With this information you can investigate the overlap of company credit clusters with their credit score, AAA, AA, A, BBB, CCC, etc.

A formula to measure the overlap between the true and new clusters are called the Rand index (RI):

- RI = (number of agreeing pairs) / (number of pairs)
- Similarity score between 0.0 and 1.0, inclusive, 1.0 stands for perfect match.

##### Implementation
```Python
from sklearn.metrics.cluster import rand_score 
## rand_score(true, pred) 
rand_score([1, 1, 1, 2, 2], [1, 1, 2, 2, 3])
## 0.6
```

- RI = (TP+TN)/(TP+TN+FP+FN) = (2+1)/(2+1+1+1+1) = 0.6

|Predict/True|1|2|
|---|---|---|
|1|2|1|
|2|1|1|
|3||1|

So it is essentially a type of accuracy measure for categorical predictions. _However this assumes you have access to the labels_.

#### Prediction Method
If you don’t have labels, one external test is to use the clusters to predict external variables of interest:

> Imagine you expect five different clusters of financial difficulty, can you use these to predict company default?

For example, you can test multiple model (KMeans, DBSCAN) and hyper-parameter choices to identify which one performs the best on an external prediction problem (beware of data leakage)

##### Implementation

```python
from sklearn import tree
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

X_train, X_test, default_train, default_test = train_test_split(X, default)
km = KMeans(n_clusters=2, random_state=0) 
cluster_label_train = km.fit_transform(X_train)
cluster_label_test  = km.transform(X_test)

clf = tree.DecisionTreeClassifier()
clf = clf.fit(cluster_label_train, default_train)

default_pred = clf.predict(cluster_label_test)
accuracy_score(default_test, default_pred)
```

<aside> 📉 This is therefore a downstream task specific method to validate the success of the clusters. Can you for example use the new clusters to predict bankruptcies?
</aside>

### Internal Validation
- Use the internal structure of the clusters.

#### Visual Method

We normally don’t know how many clusters to form unless you know the quantity of the labels in your datasets.

> For example, if you know you are looking at reports but don’t know which company they belong to, but you know they come out of a **S&P 500 dataset**, then you can create **500 clusters**.

If you don’t know the number of labels that you expect, then you can test multiple $K$ values to minimize the variance (error).
![[Pasted image 20240326095853.png | 500]]
- **The problem is that this would always be optimized when $K=N$ (e.g., distance=0).**
- Even using validation data is of no use because it will also be optimized with the highest possible value of $K$.

**Visually:** As such you need to **balance** the variance (error) and the number of $K$, and we can typically do it visually as we have with the **elbow method**.

#### Empirical Method

We can run multiple model and hyper-parameter combinations and make use of _internal validation measures_ like the **Silhouette** and **Calinski-Harabaz** score to empirically select optimal K.

> [!important] All the internal validation measures indirectly penalizes the value of K so that the optimal $K \neq N$.

![[Pasted image 20240326100230.png]]

##### Silhouette Score
By defining 
1. **a** = the mean distance between one sample and all the other datapoints in the same cluster.
2. **b** =  the mean distance between the same sample and all other points in the next nearest cluster, the silhouette coefficient s for a single sample is defined as follows:
The silhouette coefficient $s$ for a single sample is defined as follows:
$$s_i=\frac{b_i-a_i}{\max (a_i, b_i)}$$
**Every point** has a silhouette coefficient, if for datapoint $i$ the average in-cluster distance $a$ was **3** and the average to neighbor distance $b$ was **15**, then the coefficient would be $0.8$. If the in-cluster mean distance $a$ was $1$, then the coefficient would be closer to $0.9$ (near optimal, ↑ better).

![[Pasted image 20240326100420.png]]

We can calculate **$s_i$ for all of our datapoints**, this could help with outlier detection, and then we can also calculate the **silhouette score**, which averages across all points to help select the best $K$.

>[!tldr] In summary, the silhouette value is a measure of how similar an object is to its own cluster (cohesion) compared to its closest cluster (separation).

**The higher the average Silhouette score the better**

If the Silhouette score **is high** (best value 1; worst value -1), the object is well-matched to its own cluster and poorly matched to neighboring clusters.

###### Example

![[Pasted image 20240326100904.png]]
Although the MSE will always improve when the number of clusters grow, the Silhouette score would start to deteriorate when the clusters overfit, which is the benefit of using this score in assessing the quantity of clusters.

> [!tip] According to the average Scores above, we will select K = 3 instead of K = 6.

##### Calinski Harabaz index
By noting k the number of clusters, $B_{k}$ and $W_{k}$ the between and within-clustering dispersion matrices respectively defined as:

  $$ B_{k}=\sum_{j=1}^{k} n_{c^{(i)}}\left(\mu_{c^{(i)}}-\mu\right)\left(\mu_{c^{(i)}}-\mu\right)^{T}, \quad W_{k}=\sum_{i=1}^{m}\left(x^{(i)}-\mu_{c^{(i)}}\right)\left(x^{(i)}-\mu_{c^{(i)}}\right)^{T}$$

In other words, $B_{k}$ is the sum of squares **between cluster** means (High Good), and $W_{k}$ the total sum of squares **within clusters** (Low Good)**,** and $n$ is the number of datapoints.

The Calinski-Harabaz index $s(k)$ indicates how well a clustering model defines its clusters, such that the higher the score, the more dense and well separated the clusters are. It is defined as follows:

$$ s(k)=\frac{\operatorname{Tr}\left(B_{k}\right)}{\operatorname{Tr}\left(W_{k}\right)} \times \frac{N-k}{k-1} $$
1. $Tr(B_k)$: Between-cluster separation: In clustering, $B_k$ measures the spread of cluster centroids from the overall data centroid. A higher value of $Tr(Bk)$ indicates better separation between the clusters.
2. $Tr(W_k)$: Within-cluster spread: measures the spread of data points within each cluster. A lower value of $Tr(W_k)$ indicates that the data points within each cluster are more tightly grouped.
3. $N$: This is the total number of data points in the dataset.
4. $k$: This is the number of clusters being considered.

The formula calculates the ratio of the between-cluster scatter to the within-cluster scatter, which is then multiplied by a scaling factor $((N-k) / (k-1))$.

- The purpose of the scaling factor is to account for the fact that as the number of clusters (k) increases, the within-cluster scatter ($Tr(W_k)$) generally decreases.
- Therefore, the scaling factor helps balance the trade-off between having too few or too many clusters.

> [!INFO] In linear algebra, the trace of a square matrix A, denoted tr, is defined to be the sum of elements on the main diagonal of A, which is equal to the sum of squared distances if A is a variance-covariance matrix.

So in practice, you can just run the internal measure that you trust and perhaps even a few others:

![[Pasted image 20240326102632.png]]
There are many more internal validation methods, even you can come up with your own definition!

All internal validation methods loosely follow the process below:

- Step 1: Initialize a list of clustering algorithms which will be applied to the data set.
- Step 2: For each clustering algorithm, use different combinations of parameters to get different clustering results.
- Step 3: Compute the corresponding internal validation index of each combination in step 2.
- Step 4: Choose the best partition and the optimal cluster number according to the criteria.

![[Pasted image 20240326102704.png]]

## Robustness

You could also measure the stability of your clustering model, generally the process involves:

- Generate several new datasets out of the original one (by sampling).
- Cluster all these new datasets.
- Define statistic to formalize how similar new clusterings are to the original one.
- If they are very similar, it's stable.
- Many methods have been proposed like Fang and Wang (2012) bootstrap stability.

> "The validation of clustering structures is the most difficult and frustrating part of cluster analysis. Without a strong effort in this direction, cluster analysis will remain a black art accessible only to those true believers who have experience and great courage."

> Algorithms for Clustering Data, Jain and Dubes (1998)

