Trees grow wider way faster than they grow higher. This is a special property of trees.

## Using Node-Link Diagram

### Example
![[Pasted image 20240308234039.png]]

## Dendrogram
-  Specially suitable for hierarchical data
-  Binary Tree
-  Represents a heirarchical cluster
-  Height of the link between 2 objects presents their "similarity distance".
-  Can be used in conjuction with fixed layout or be combined with an adjacency matrix as well.
### Heirarchical clustering
- Data mining algorithm to organize objects into a heirarchy according to a similarity metric defined between the objects. 
	1. Compute Similarity of all node pairs
	2. Find the closest pair
	3. Connect those nodes with a link.
	4. Height of the link is indicating how similar they are.
	5. Do this over and over, and keep connecting.
	6. You get one root, representing the entire data.
### Use case
1. Useful to group together and visualize any set of complex objects on top of which a distance function can be defined.
### Example
![[Pasted image 20240309064355.png]]

![[Pasted image 20240309064602.png]]
![[Pasted image 20240309064631.png]]
## TreeMap
- A space filling visualization technique.
- Treemaps **display hierarchical (tree-structured) data as a set of nested rectangles**. Each branch of the tree is given a rectangle, which is then tiled with smaller rectangles representing sub-branches.
- Division direction alternates between vertical and horizontal.

### What kind of info to visualize ?
Area: Quantity
Color: Quantity/Category
Heirarchy: Nesting

### Example
![[Pasted image 20240309065344.png]]

### Advantages
1. Scalability
	1. Node visibility
	2. No overlapping marks
	3. Supports size and color.
### Drawbacks
1.  **Area is not the most accurate channel**: When rectangles have very different aspect ratios(proportion of height vs width), comparing areas gets harder. (Especially with thin elongated rectangles). **Solution**: Squarified Treemaps, but in a balanced tree it loses the heirarchial information(use Cushion Treemaps instead).

![[Pasted image 20240309065545.png]]
2. Structures are hard to discern.
### Other variants
1. Circle-packing TreeMaps
2. Voronoi TreeMaps
3. News TreeMaps
4. Cushion Tree

## Sunburst and Icicle Diagrams

### Sunburst
- More beginner friendly than tree-map for newbies, but for trained people treemap between better.
![[Pasted image 20240309071914.png]]

#### Anatomy

![[Pasted image 20240309071920.png]]


### Icicle

![[Pasted image 20240309072140.png]]

- Same as sunburst, but using rectangular layout instead of radial.


## References
1. Information Visualization Course - NYU Tandon School of Engineering(Spring 2024)

## Further reading
1. [Treemaps for space-constrained visualization of hierarchies](https://www.cs.umd.edu/hcil/treemap-history/)
2. 
