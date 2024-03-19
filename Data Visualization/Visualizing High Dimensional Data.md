## Why is it hard ?
- There are patterns that cannot be easily identified by visual observation.
- The visualization space is inherently low dimensional.

### Curse of Dimensionality
- Large observational space often requires large number of observations for patterns to emerge.
- **Silver Lining**: Often Data are structured so that they are located on an unknown lower dimensional manifold.

## Methods

### Panel Matrices
![[Pasted image 20240318145414.png]]
#### Example
![[Pasted image 20240318145753.png]]
- **Diagonal**: Histogram
- **Lower Half**: Scatter plot + Fitted Line
- **Upper Half**: Correlation Chart
#### Pros
1. Compact
2. Easy Comparison
3. Flexible
#### Cons
1. Complex for a large number of variables.
2. Limited order into high-order relationship.
3. May lead to information overload.
### Parallel Co-ordinates
- Vertical Co-ordinate axis for every single variable.
- Every observation will be a poly-line.
- Can have as many co-ordinate as we want.
- Common Mathematical relationships show very nice, clear patterns.
![[Pasted image 20240318150924.png]]
#### Interactive Examples
https://syntagmatic.github.io/parallel-coordinates/ 

#### Pros
1. Represent high dimensional data in 2D
2. Mathematical relationship leads to the pattern
3. Suitable for interactive exploration

#### Cons
1. Only relationship between adjacent dimensions are revealed.
2. May suffer from over-plotting.

### Radar Chart
- Instead of putting all co-ordinates in the parallel fashion, we re-arrange them in the circular fashion.
#### Example
![[Pasted image 20240318151624.png]]
![[Pasted image 20240318151655.png]]
#### Pros
- Simple to Understand
- Useful for Comparisons
#### Cons
- Cannot scale to many variables
- Be careful about scales.
### Dimension Reduction and Clustering
#### Clustering
**Core Idea**: Group observations by some kind of similarity measure.

##### Example:
Clustering of MNIST dataset using t-SNE algorithm

![[Pasted image 20240318174035.png]]
##### [[K-Means algorithm]]

But in order to use clustering algorithms, we need to reduce the data to a lower dimensional space using dimension reduction.
### Dimension Reduction
**Core Idea**: Embed high dimensional data into lower dimension such that some kind of structure is preserved.  Similar data in high-dimensional space should stay close to each other in low dimensional space.
##### [[Principal Component Analysis]]
![[Pasted image 20240318173037.png]]

##### Multi-Dimensional Scaling
**Objective**: Reduce the dimension of the data
- Define "distance" as how similar 2 observations are
- Such "distance" should be preserved in low dimensional embedding
**How**:
- Directly find low dimensional embedding via numerical optimization
###### Example
![[Pasted image 20240318173505.png]]
###### Pros
1. Support non-linear embedding
###### Cons
1. More complicated to implement
2. Hard to interpret semantics of axis

##### Stochastic Neighbor Embedding
- Objective: Reduce the dimension of the data. 
	- Define similar observations as “neighbors”.  
	- “Neighborhood” should be preserved in low dimensional embedding. 
- How? 
	- Directly find low dimensional embedding via numerical optimization.

![[Pasted image 20240318173843.png]]
###### Pros
1. General method for discovering hidden structure within high dimensional data.
2. Implementation available in a lot of packages.
### Icon Based
- Icon = Observation Case
- Variable visualized as properties of the icon
- Independent coding
- Often used in Maps

#### Example
1. 
![[Pasted image 20240318183516.png]]
2. Film Flowers ![[Pasted image 20240318183711.png]]
#### Pros
1. Creative and Engaging
#### Cons
1. Hard to interpret
2. May not be supported by standard packages
3. Cannot scale to very high dimensional data
### Pixel Based
- Each data is a pixel.
- Size(number of pixels): No. of observations * No. of variables
- Provide an overview of the entire dataset.
#### Arrangement
- Need to find a linear ordering of all pixels in our dataset, typically done by sorting through one or more columns in our dataset.
- Then, we need to decide how to lay them out in the 2-D space, and there are multiple ways to do it.
![[Pasted image 20240318201102.png]]

#### Example
![[Pasted image 20240318201216.png]]
#### Pros
1. Provides an instant overview of the entire dataset
#### Cons
1. Not good for reading details
2. Depends on ordering of the observations
3. Cannot scale to very high dimension
Read More: [Pixel-Oriented Visualization Techniques for Exploring Very Large Data Bases](https://www.jstor.org/stable/1390753)

### Dimensional Stacking
Dimensional stacking is ==a technique that projects high-dimensional data by embedding dimensions within other dimensions==. It allows users to visualize trends in a high-dimensional space.
![[Pasted image 20240318202005.png]]

#### Pros
1. Interactively zoom into one of the subplots in a heirarchial manner.
2. Scales well with large no of observations
3. Provides an overview of the dataset.
#### Cons
1. Cannot scale to very high dimensions.









