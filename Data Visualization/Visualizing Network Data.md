## What is Network Data ?
- **Network Data** = Info. about Objects + Info. about relationships between the objects.

![[Pasted image 20240306125707.png]]
The objects are called **Nodes** and the relationship between them are called **Links**.

### Example

![[Pasted image 20240306125843.png]]

## Visualization Techniques

### Node-link diagram techniques
**Nodes**: dots/markers (Use size, shape, color encodings )
**Links**: Lines connecting dots(Use thickness, color, line style encodings)
#### Forced Directed Layout
A force-directed layout is a type of graph visualization that ==uses a physics-based simulation to position nodes and edges==.

There are 3 physical forces involved in positioning the nodes and links in the layout:

1. Repulsion
2. Springs
3. Network Energy

- **Nodes** are treated like charged particles that produce a repulsive force that moves them apart.
- **Springs** pull the nodes closer.
- We add some **energy** to the system by setting each node to move in a random direction.

![[Pasted image 20240306133059.png]]

The layout simulates this system until the nodes settle in a stable configuration.
![[Pasted image 20240306133119.png]]
![[Pasted image 20240306133207.png]]
##### Importance
When you’re dealing with huge, complex networks of connected data, you need a layout that can reveal the underlying structure of every connected component. The force-directed approach clearly identifies connected subnetworks and positions them carefully, making it far easier to compare clusters of nodes in different parts of the network.

A force-directed layout minimizes overlaps in the graph, evenly distributes nodes and links, and organizes items so that links are of a similar length. It is clear, reliable and makes a good all-rounder for any type or size of dataset, because the focus is on finding patterns and symmetries.

##### Structures
1. Fan
 ![[Pasted image 20240306133630.png]]
2. Clique
![[Pasted image 20240306133724.png]]

##### Arrow types and readability
Read: [An Extended Evaluation of the Readability of Tapered, Animated, and Textured Directed-Edge Representations in Node-Link Graphs](https://www.researchgate.net/publication/221536322_An_Extended_Evaluation_of_the_Readability_of_Tapered_Animated_and_Textured_Directed-Edge_Representations_in_Node-Link_Graphs)

##### Pros 
1. Intuitive
2. Flexible
3. Interactive
4. Structure Revealing
##### Cons
1. Overcrowding
2. Edge crossing
3. Non-unique
4. Does not scale

#### Fixed Layout
- Positions of the nodes are fixed based on some criteria. 
##### Types of fixed Layout:
1. Linear
![[Pasted image 20240307104028.png]]
2. Circular
![[Pasted image 20240307104116.png]]
3. Grid
![[Pasted image 20240307104137.png]]
- Most popular one is circular, because we need to draw links between nodes and circular layout allows us to draw links going through the middle of the circle easily.

##### Fixed Layout vs Force-Directed Layout
- Force directed Layout reveals the structure of the layout, but if we already know the structure, then you can take advantage of the knowledge and lay the points out directly.

##### Edge bundling
- Fixed Layout does not really solve the over-plotting issue. 
- Edge bundling handles this by bundling edges going in the same direction, till the point when they need to branch out.
- Computationally performed by building a heirarchy of nodes, and then connecting them. 
- Read this: [Intro to Heirarchical Edge Bundling with R](https://r-graph-gallery.com/309-intro-to-hierarchical-edge-bundling)
###### Example
![[Pasted image 20240308232050.png]]


##### Pros
1. Visibility of nodes and edges
2. Nodes can be grouped into meaningful categories
3. Good for spatial data(as nodes are fixed on map)

##### Example
![[Pasted image 20240307104636.png]]
#### Clutter Reduction
 1. Edge Bundling
 2. Clustering
	-  ![[Pasted image 20240308232338.png]]
	- Group and highlight similar nodes, to increase graph readability
3. Drawing edges on demand. 
4. Motif Simplification
	- Common patterns of nodes and links are replaced with compact and meaningful simple symbol.
	- ![[Pasted image 20240308232723.png]]
	

### Matrix techniques
- Adjacency matrix: Nodes become rows and columns, edges become cells.

##### Example
![[Pasted image 20240308233003.png]]

##### Pros
1. Node visibility, every node is present.
2. Compact representation
3. Easy computation
4. No line crossing
5. No Hairball effect(The hairball – **showing connections that are so dense, they can't be usefully visualized**)
##### Cons
1. Familiarity
2. Scalability
3. Needs Reordering, pattern visibility depends on how columns/rows are ordered.
4. Most cells are empty(Data is mostly sparse)
5. Limited flexibility

### [[Visualizing Trees]]

## References
1. Information Visualization Course - NYU Tandon School of Engineering(Spring 2024)
2. https://cambridge-intelligence.com/keylines-faq-force-directed-layouts/
3. https://observablehq.com/@d3/force-directed-graph-component
4. https://truth-and-beauty.net/projects/muesli-ingredient-network 