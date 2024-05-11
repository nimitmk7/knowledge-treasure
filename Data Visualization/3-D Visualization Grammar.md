## Pipeline

![[Pasted image 20240505094859.png]]

## Components

![[Pasted image 20240505094939.png]]

## Data

- Geometric Data
	- Describes the 3-D shape/space.
	- Point vs Curve vs Surface vs Volume
- Data related to Geometry
	- Observations with 3-D spatial association
	- Scalar vs Vector vs Tensor

![[Pasted image 20240505095515.png | 500]]
- Similar to spatial data for map-based visualization.

### Example

![[Pasted image 20240505095628.png | 500]]

### Shape representation methods
- Analytical formula
- Piecewise-linear approximation
- High order approximation
- Implicit representation

## Transform
- It is the process of generating/modifying data for easy visualization.
- 3 types of transformations:
	- Interpolation (Transforming data associated with the geometry)
	- Aggregation (Transforming data associated with the geometry)
	- Generation (Transforming data into geometry)

### Interpolation
![[Pasted image 20240505200851.png | 300]]
Interpolation is a mathematical technique that estimates the values of unknown data points between known data points. 

It's a type of estimation in numerical analysis that involves creating new data points based on a range of known data points. Interpolation is used to fill in missing data, smooth existing data, and make predictions.
#### Types
● Barycentric coordinates. 
● Mean value coordinates.
● Harmonic coordinates.
● Biharmonic coordinates. 
● Green coordinates.


### Aggregation

● Combine multiple “nearby” data points into a single data point. 
● “Nearby” is often measured using geodesic distance. 
● Voronoi diagram is often used.

![[Pasted image 20240505201416.png | 300]]

### Generation
- Compute new data from existing data
- Common examples:
	- Iso-contours
	- Streamlines
	- Exploded/cut-away aways.

#### Iso-contours
![[Pasted image 20240505201619.png]]

#### Streamlines
![[Pasted image 20240505201720.png]]

#### Cut-away view
![[Pasted image 20240505201813.png | 300]]

#### Exploded View
![[Pasted image 20240505201842.png | 400]]


## Visual Channels

### Position
- Encoding the geometry.
![[Pasted image 20240505210614.png | 300]]
### Color
- Encode quantitative/categorical data. 
- Lights/shadows may change the color.
- Our eyes/brains are trained to recognize colors under different lighting conditions.
#### Example


![[Pasted image 20240505210649.png | 400]]

1. Categorical data
2. Cutaway view
3. Scalar field

#### Material 
![[Pasted image 20240505210936.png]]

### Size
- Not used as often because foreshortening makes it difficult to distinguish different sizes.
- Often need to add additional visual cues such as, “Shadow” or “blurring”.

![[Pasted image 20240505211131.png]]
![[Pasted image 20240505211205.png]]

## Marks
- Points (0-D elements)
- Curves/lines(1-D elements)
- Triangles/quads (2-D elements)
- Tetrahedra/Hexahedra (3-D elements)

### Points

![[Pasted image 20240505211440.png]]

### Curves/Lines
![[Pasted image 20240505211506.png]]

### Triangles/Quads

![[Pasted image 20240505211534.png]]
### Tetrahedra/Hexahedra

![[Pasted image 20240505211604.png]]

## Layer Composition

![[Pasted image 20240505211956.png]]

