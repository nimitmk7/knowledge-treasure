****Breadth First Search (BFS)**** is a graph traversal algorithm that explores all the vertices in a graph at the current depth before moving on to the vertices at the next depth level. It starts at a specified vertex and visits all its neighbors before moving on to the next level of neighbors.
## Example

**Step 1:** Initially queue and visited arrays are empty.

![[Pasted image 20240512202032.png]]
**Step 2:** Push node 0 into queue and mark it visited.

![[Pasted image 20240512202053.png]]

**Step 3**: Remove node 0 from the front of queue and visit the unvisited neighbors and push them into queue.

![[Pasted image 20240512202124.png]]

**Step 4**: Remove node 1 from the front of queue and visit the unvisited neighbors and push them into queue.

![[Pasted image 20240512202252.png]]

**Step 5:** Remove node 2 from the front of queue and visit the unvisited neighbors and push them into queue.

![[Pasted image 20240512202330.png]]

**Step 6:** Remove node 3 from the front of queue and visit the unvisited neighbours and push them into queue.  As we can see that every neighbors of node 3 is visited, so move to the next node that are in the front of the queue.

![[Pasted image 20240512202414.png]]
**Step 7:** Remove node 4 from the front of queue and visit the unvisited neighbors and push them into queue.  As we can see that all neighbors of node 4 are visited, so move to the next node that is in the front of the queue. 

![[Pasted image 20240512202448.png]]

Now, Queue becomes empty. So, we terminate the process of iteration.



