Link: https://leetcode.com/problems/insert-delete-getrandom-o1/description/ 

## Solution

Initialize a `vector<int>` and `map<int, int>`.  

Use the map to check presence and location, vector to store the elements.

### Insert
Check the map for presence of the integer, if present return False.
Else, Insert integer at the end.

### Delete
Check map for presence of integer. If absent, return False.

If present, and is at the end, remove entry from map , and remove last element of vector.

If present, and in the middle, swap with the last entry, and then remove last entry.


Watch Video: 
<iframe width="560" height="315" src="https://www.youtube.com/embed/j4KwhBziOpg?si=DYFS0kYpx7Rp9hbj" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Link: 
https://youtu.be/j4KwhBziOpg?si=sLT5svp7LnhTUx1e 

Solution link:
https://leetcode.com/problems/insert-delete-getrandom-o1/solutions/85401/java-solution-using-a-hashmap-and-an-arraylist-along-with-a-follow-up-131-ms 

