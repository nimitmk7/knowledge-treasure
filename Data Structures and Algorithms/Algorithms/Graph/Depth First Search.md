Depth-first search is an algorithm for traversing or searching tree or graph data structures. The algorithm starts at the root node (selecting some arbitrary node as the root node in the case of a graph) and explores as far as possible along each branch before backtracking.

### Example

**Step 1**: Initially stack and visited arrays are empty.
![[Pasted image 20240512201050.png]]
**Step 2:** Visit 0 and put its adjacent nodes which are not visited yet into the stack. 
![[Pasted image 20240512201133.png]]

**Step 3**: Now, Node 1 at the top of the stack, so visit node 1 and pop it from the stack and put all of its adjacent nodes which are not visited in the stack.
![[Pasted image 20240512201207.png]]

**Step 4:** Now, Node 2 at the top of the stack, so visit node 2 and pop it from the stack and put all of its adjacent nodes which are not visited (i.e, 3, 4) in the stack.
![[Pasted image 20240512201257.png]]
**Step 5:** Now, Node 4 at the top of the stack, so visit node 4 and pop it from the stack and put all of its adjacent nodes which are not visited in the stack.
![[Pasted image 20240512201348.png]]
**Step 6:** Now, Node 3 at the top of the stack, so visit node 3 and pop it from the stack and put all of its adjacent nodes which are not visited in the stack.
![[Pasted image 20240512201418.png]]
Now, Stack becomes empty, which means we have visited all the nodes and our DFS traversal ends.

## Complexity analysis

**Time Complexity**: O(V + E)
**Space Complexity**: O(V) for recursive implementation

