## Definition
The combination of a set of position scales and their relative geometric arrangement is called a coordinate system.

## Cartesian Coordinates
The most widely used coordinate system for data visualization is the 2D Cartesian coordinate system, where each location is uniquely specified by an x and a y value. The x and y axes run orthogonally to each other, and data values are placed in an even spacing along both axes.

Any data values between these axis limits are placed at the appropriate respective location in the plot. Any data values outside the axis limits are discarded.

![[Pasted image 20240206141040.png]]
- Spacing between grid lines along axis = Discrete steps in data units
- The 2 axes can represent 2 different units, which is the case when variables being mapped to **x** and **y** are different.
- When the 2 axes are measured in different units, we can stretch or compress one relative to the other and maintain a valid visualization of the data, depending on the story we would like to convey.
	- Tall and narrow - Emphasize change along the y-axis.
	- Short and wide - Emphasize change along the x-axis.
	Choose an aspect ratio that ensures that any important differences in position are noticeable.

![[Pasted image 20240206141929.png]]
- On the other hand, if the x and y axes are measured in the same units, then the grid spacings for the two axes should be equal, such that the same distance along the x or y axis corresponds to the same number of data units. 
- Cartesian co-ordinates are invariant under linear transformations. Therefore, you can change the units of your data and the resulting figure will not change as long as you change the axes accordingly.

## Non-linear Axes

In a non-linear scale, even spacing in data units correspond to uneven spacing in the visualization, or conversely even spacing in the visualization corresponds to uneven spacing in data units.

### Log Scale

The most commonly used nonlinear scale is the **logarithmic scale**, or log scale for short. Log scales are linear in multiplication, such that a unit step on the scale corre‐ sponds to multiplication with a fixed value. To create a log scale, we need to log- transform the data values while exponentiating the numbers that are shown along the axis grid lines. 

![[Pasted image 20240206225340.png]]
Mathematically, there's no difference between:
1. Plotting log-transformed data on a linear scale
2. Plotting original data on a logarithmic scale
Although, the latter is preferable;
1)  Places less mental burden on the reader to interpret the numbers shown as the axis tick labels.
2) Less risk of confusion about the base of an algorithm

Because multiplication on a log scale looks like addition on a linear scale, log scales are the natural choice for any data that has been obtained by multiplication or division. In particular, ratios should generally be shown on a log scale. 

On a log scale, the value 1 is the natural midpoint, similar to 0 on a linear scale.
Values > 1 ⇒ Multiplication
Values < 1 ⇒ Division

Log scales are frequently used when the dataset contains numbers of very different magnitudes.

**Disadvantage**: Logarithmic scale can't represent the value 0 or values very near to 0. As $log(0) = -\infty$. Solution: the Square root scale

### Square-root scale
Compresses larger numbers into a smaller scale, but allows for the presence of 0. 
It is the natural scale for data that comes in squares. E.g. Areas of geographical regions

![[Pasted image 20240206231509.png]]
**Disadvantages**:

1. The meaning of a unit step on a square-root scale depends on the scale value at which we’re starting.
2. It is unclear how to best place axis ticks on a square-root scale.

## Curved Axes
### Polar coordinates

In particular, in the polar coordinate system, we specify positions via an angle and a radial distance from the origin, and therefore the angle axis is circular.  
Useful for data of periodic nature.

![[Pasted image 20240206232326.png]]

## Geospatial Data(Maps)

Locations on the globe are specified by their longitude and latitude. But because the earth is a sphere, drawing latitude and longitude as Cartesian axes is misleading and not recommended. 

Instead, we use various types of nonlinear projections that attempt to minimize artifacts and that strike different balances between conserving areas or angles relative to the true shape lines on the globe.

![[Pasted image 20240206232718.png]]
