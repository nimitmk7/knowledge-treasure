# Spatial Visualization Techniques
## Categorization of common techniques

| Objects                               | Densities                                                   | Values                                                    |
| ------------------------------------- | ----------------------------------------------------------- | --------------------------------------------------------- |
| **Dot Maps**(Distribution of Objects) | **Heat Maps**(Distribution of values, continuous)           | **Choropleth Maps**(Distribution of values, discrete/geo) |
|                                       | **Binned Maps**(Distribution of values, discrete/arbitrary) | **Symbol Maps**(Distribution Maps, discrete/geo)          |
## Dot Map
- Each item is mapped to its location.
- Useful to depict distribution of events and geo-located objects.
- Dot color and size can encode additional information

### Example
![[Data Visualization/Images/Spatial_example.png]]
## Heat Map
- Used to visualize the continuous distribution of a quantity.
- Typically use a "density estimation" method, which estimates a continuous density model from discrete data.

### Color Map
- Color really matters.
- Most common color map are Rainbow and Jet, but they are not very suitable. Converting them to B&W, we see that brightness does not decrease/increase monotonically, when we go from Left to Right.
- Not very helpful as when we look at heat map, we tend to pick out light or dark regions rightaway.

> [!WARNING] DO NOT USE RAINBOW/JET COLOR SCHEME
> Most common color map are Rainbow and Jet, but they are not very suitable. Converting them to B&W, we see that brightness does not decrease/increase monotonically, when we go from Left to Right. It is bot very helpful, because when we look at heat map, we tend to pick out light or dark regions right away.
> ![[Data Visualization/Images/Color_schemes.png]]
>  

#### Recommended ColorMaps
![[Color_schemes 1.png]]
- **Viridis** and **Heat**: Light to Dark
- **Cool Warm**: Dark to light to Dark (Bidirectional, so very useful to represent diverging data).
- 
### Example
![[Pasted image 20240301212454.png]]
## Binned Map
- Visualize discrete distribution of values.
- Binned map partition space into discrete bins. 
- Color encodes aggregated quantity over each bin.
### Example
![[spatial_example_1.png]]

## Choropleth Map
We frequently want to show how some quantity varies across locations. We can do so by coloring individual regions in a map according to the data dimension we want to display. Such maps are called **choropleth** maps.
### Example

1. Distribution of information
![[chloropleth.png.png]]
2. Categorical information

![[spat_example.png]]
3. Focus on the foreground
![[spatial_eg.png]]

## Symbol Map
- Uses symbol in particular size to associate quantity associated with the region.
- Does not depend on region's size/shape.
- Easier to use multiple channels(color, shape)
- Does not solve the spatial density problem.

### Example
![[spatial_eg1.png]]

## Cartograms
Not every map-like visualization has to be geographically accurate to be useful. 

A cartogram is a thematic map of a set of features, in which their geographic size is altered to be directly proportional to a selected variable, such as travel time, population, or Gross National Product. This helps convey information of that variable.

### Example
- Original Chloropleth map
![[chloropleth_bad.png]]
- Cartogram with shape corrected for population in each state.
![[cartogram.png]]
### Cartogram Heatmap
- Each region is represented using an equally sized square.
![[cartogram_heatmap.png]]
### Other complex cartograms
![[complex_cartogram.png.png]]

