A **Binary Search Tree** is a data structure used in computer science for organizing and storing data in a sorted manner. 

Each node in a **Binary Search Tree** has at most two children, a **left** child and a **right** child.

The **left** child contains values less than the parent node.
The **right** child containing values greater than the parent node. 

This hierarchical structure allows for efficient **searching**, **insertion**, and **deletion** operations on the data stored in the tree.

## Example
![[Pasted image 20240514142457.png | 500]]

## Common operations

### Insertion
- Time Complexity: $O(n)$
### Deletion
   - Time Complexity: $O(n)$
### Search
- To search for a key $k$, trace a downward path starting at the root. The next node visited depends on the outcome of the comparison of $k$ with the key of the current node. 
- If we reach a leaf, the key is not found and we return null.
- Time Complexity: $O(\log n$)
#### Example
![[Pasted image 20240514143029.png]]


