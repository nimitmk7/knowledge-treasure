Many datasets contain info. linked to locations in the physical world. In all such cases, it can be helpful to visualize the data in their proper geospatial context,i.e., to show the data on a map or alternatively as a map-like diagram.

## Types of Geographical Data

1. **Spatial** : Have a spatial boundary and existence
	- E.g. Counties, Regions, Buildings, Rivers, Lakes
	- Shape, spatial extent matters
	- Map is main object of interest
	- **Types**: Regions(e.g. Country Borders), Locations(e.g. Lat-Long), Identifiers(e.g. Zip Code)![[Pasted image 20240229235724.png]]
2. **Non-Spatial**: Individual objects that have relationship with spatial data
	- E.g. Cars, People, Weather station
	- Shape does not matter. Map is used as a reference, to track the relationship with spatial data(a.k.a locate)

## When to use a Map ?
### Limitation of Map:
- Maps use space to represent space. Therefore, space cannot be used to encode other types of information. (Remember, space-X and Y coordinates is the most effective way to represent data). 
- A bad example of unnecessarily using map to represent sales data:
![[Pasted image 20240301000653.png]]

### Relevance of Map
1. Question is inherently spatial
	- Question that correlates a phenomena to spatial location/objects.
	- Question pertaining phenomenon for which spatial proximity or extend is relevant.
2. When map helps find information needed.
Example:
![[Pasted image 20240301001111.png]]

## Projections
Unique location on Earth: (Latitude, Longitude, Altitude)
- Altitude not used in Data Visualization

- The challenge in map making is that we need to take the spherical surface of the earth and flatten it out so we can display it on a map. 
- This process, called **projection**, necessarily introduces distortions, because a curved surface cannot be projected exactly onto a flat surface. Specifically, the projection can preserve either angles or areas but not both. 
- A projection that does the former is called **conformal** and a projection that does the latter is called **equal-area**. 
- Other projections may preserve neither angles nor areas but instead preserve other quantities of interest, such as distances to some reference point or line. Finally, some projections attempt to strike a compromise between preserving angles and areas

## Layers
To visualize geospatial data in the proper context, we usually create maps consisting of multiple layers showing different types of information.

### Example
**Target Image**: 
![[geospatial_map.png]]
**Layers**: 
![[Geospatial_layers.png]]

> [!IMPORTANT] 
> Regardless of which layers you decide to keep or remove, it is generally recommended to add a scale bar and a north arrow. The scale bar helps readers understand the size of the spatial features shown in the map, while the north arrow clarifies the map’s orientation.

## Choropleth Mapping
We frequently want to show how some quantity varies across locations. We can do so by coloring individual regions in a map according to the data dimension we want to display. Such maps are called **choropleth** maps.

### Example
![[chloropleth.png.png]]

### Color Associations
- We tend to associate darker colors with higher intensities when the background color of the figure is light. However, we can also pick a color scale where high values light up on a dark background
- As long as the lighter colors fall into the red-yellow spectrum, so that they appear to be glowing, they can be perceived as representing higher intensities. 
- As a general principle, when figures are meant to be printed on white paper, light-colored background areas will typically work better. For online viewing or on a dark background, dark-colored background areas (as in Figure 15-12) may be preferable.

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

## Implementation/Hands-on:

### Geopandas tutorial


## References
1. Chapter 15: https://clauswilke.com/dataviz/
2. NYU's Data Visualization Course by Qingnan Zhou
