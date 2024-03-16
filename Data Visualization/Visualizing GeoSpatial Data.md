Many datasets contain info. linked to locations in the physical world. In all such cases, it can be helpful to visualize the data in their proper geospatial context,i.e., to show the data on a map or alternatively as a map-like diagram.

## Types of Geographical Data

1. **Spatial** : Have a spatial boundary and existence
	- E.g. Counties, Regions, Buildings, Rivers, Lakes
	- Shape, spatial extent matters
	- Map is main object of interest
	- **Types**: Regions(e.g. Country Borders), Locations(e.g. Lat-Long), Identifiers(e.g. Zip Code)![[Geocoding_1.png]]
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
![[Geo_example.png]]

## Layers
To visualize geospatial data in the proper context, we usually create maps consisting of multiple layers showing different types of information.

### Example
**Target Image**: 
![[geospatial_map.png]]
**Layers**: 
![[Geospatial_layers.png]]

> [!IMPORTANT] 
> Regardless of which layers you decide to keep or remove, it is generally recommended to add a scale bar and a north arrow. The scale bar helps readers understand the size of the spatial features shown in the map, while the north arrow clarifies the map’s orientation.
## [[Spatial Visualization Techniques]]
## Color Associations
- We tend to associate darker colors with higher intensities when the background color of the figure is light. However, we can also pick a color scale where high values light up on a dark background
- As long as the lighter colors fall into the red-yellow spectrum, so that they appear to be glowing, they can be perceived as representing higher intensities. 
- As a general principle, when figures are meant to be printed on white paper, light-colored background areas will typically work better. For online viewing or on a dark background, dark-colored background areas (as in Figure 15-12) may be preferable.

## Common Issues with Maps

### Base Rate Bias
- Base rate fallacy refers to **the tendency to ignore relevant statistical information in favor of case-specific information**. Instead of taking into account the base rate or ==prior probability== of an event, people are often distracted by less relevant information.
- Measurement should be normalized by the base rate to avoid bias.

### Example:
![[spa_vs_eg.png]]
### Insensitivity to Sample Size
- Small sample size may lead to extreme results.
- Insensitivity to sample size is a cognitive bias that occurs when people judge the probability of obtaining a sample statistic without respect to the sample size.

### Perception Bias
1. Color perception is highly affected by size.
2. Color of surrounding regions also affects the way we perceive color.
3. Size perception is affected by the size of the neighboring symbols.

### Projections
Unique location on Earth: (Latitude, Longitude, Altitude)
- Altitude not used in Data Visualization
- The challenge in map making is that we need to take the spherical surface of the earth and flatten it out so we can display it on a map. 
- This process, called **projection**, necessarily introduces distortions, because a curved surface cannot be projected exactly onto a flat surface. Specifically, the projection can preserve either angles or areas but not both. 
- A projection that does the former is called **conformal** and a projection that does the latter is called **equal-area**. 
- Other projections may preserve neither angles nor areas but instead preserve other quantities of interest, such as distances to some reference point or line. Finally, some projections attempt to strike a compromise between preserving angles and areas.
#### Types
1. **Azimuthal Equidistant**: Preserves distance and direction from centre point.
2. **Equidistant Conic**: Preserves proportional distance of meridians.(Land survey, choropleth map)
3. **Equal Area Conic**.
4. **Mercator**: Preserves angle(conformal), used for navigation.
5. **Cylindrical Mercator**
6. **Others**: Robinson, Goode Homolosine, Gall-Peters, Orthographic, Dymaxion Map, Waterman butterfly.

## Interference
- Use few and low saturated color for map.
- No strong lines for border
- Include only necessary spatial features.
## Animations
- Can make a map visualization richer and more interpretable, can be used for time scale.
## Facets
- Can be used to encode time, but can remove map in visualization if want to use facets in some cases.
## Implementation/Hands-on:

### Geopandas tutorial
[Geopandas intro and examples](https://github.com/nimitmk7/knowledge-treasure/blob/main/Data%20Visualization/GeoPandas%20Intro.ipynb)

### Geopandas user guide
https://geopandas.org/en/stable/docs/user_guide.html 

### Case Studies
[New York Borough and Zip Code Boundaries](https://github.com/nimitmk7/knowledge-treasure/blob/main/Data%20Visualization/Case%20Study%20-%20New%20York%20Borough%20and%20Zip%20Boundaries%20(1).ipynb) 
[NYC Subway](https://github.com/nimitmk7/knowledge-treasure/blob/f6b525058b374d975c39a76b250438b3612bbb8c/Data%20Visualization/Case%20Study%20-%20NYC%20Subway.ipynb)

## References
1. Chapter 15: https://clauswilke.com/dataviz/
2. NYU's Data Visualization Course by Qingnan Zhou
3. https://www.scribbr.com/fallacies/base-rate-fallacy/#:~:text=Revised%20on%20September%204%2C%202023,distracted%20by%20less%20relevant%20information.
