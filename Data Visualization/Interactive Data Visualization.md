## Why Interaction ?
- It is often impossible to visualize in a single static view all the information needed to answer all the questions at once.
	- Too much information
	- Need adaptation for multiple questions.
- As a general purpose application for data analysis and exploration.

## Visualization Pipeline

![[Pasted image 20240327201424.png]]
- We can integrate “interaction” between any of these steps.
![[Pasted image 20240327201605.png]]

## Types
### Single View

![[Pasted image 20240327201700.png|300]]
**Overview of Methods**
![[Pasted image 20240327202621.png]]
#### Selection
Any action aimed at selecting one or more elements of the visualization.
![[Pasted image 20240327204531.png]]
Examples:
1. https://syntagmatic.github.io/parallel-coordinates/
2. https://www.economist.com/graphic-detail/2022/01/15/why-north-dakota-not-new-york-may-be-the-land-of-opportunity
3. https://www.economist.com/interactive/graphic-detail/2022/01/29/what-spotify-data-show-about-the-decline-of-english
#### Navigation
- Common form of interaction used in map-based visualizations.
- Includes zooming and panning the view.
- Semantic zooming is designed to selectively display information based on the zooming level in order to avoid cluttering.
E.g. Google Maps

#### Spatial Arrangement
- Allows users to rearrange the order data entries are visualized.
- Commonly used in explorative data visualization to discover patterns within the dataset.
![[Pasted image 20240327215109.png]]

#### Change Mapping
- User can interactively change the way attributes are encoded in the visual channel.
- May lead to use of a completely different graph or change in properties of a given graph.

#### Aggregation
- Change the level of a granularity of a given data set(related to hierarchical attributes)
- Space and time are hierarchical and often require observing patterns at different resolutions.
- Different pattern may emerge at different resolution.

##### Example
![[Pasted image 20240327235343.png]]

#### Filtering
- Interactive filtering of the data allows readers to explore different facets of the datasets.

##### Example
![[Pasted image 20240327235938.png]]
### Multiple Linked View

![[Pasted image 20240327201741.png |300]]
#### Why ?
- Show different properties of the same data simulataneously.
	- Different Information(Subset, Attributes, Granularity, Transformation)
	- Different representation
- Use one view to navigate, select, filter information in the other view.

#### Examples
1. Crossfilter: https://square.github.io/crossfilter/
2. Google Street View ![[Pasted image 20240328001028.png]]
3. https://www.economist.com/big-mac-index




