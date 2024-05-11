The pigeonhole principle states that if $n$ items are put into $m$ containers, with $n > m$, then at least one container must contain more than one item.

### Sample Problem

Write a function that, given an array A of N integers, returns the smallest positive integer (greater than 0) that does not occur in A.

For example, given A = $[1, 3, 6, 4, 1, 2]$, the function should return 5.

Given A = $[1, 2, 3]$, the function should return 4.

Given A = $[−1, −3]$, the function should return 1.


### Solution

At least one of the numbers 1, 2, ..., n+1 is not in the array. Let us create a boolean array of size n+1 to store whether each of these numbers is present.

Now, we process the input array. If we find a number from 1 to n+1, we mark the corresponding entry in . If the number we see does not fit into these bounds, just discard it and proceed to the next one. Both cases are O(1) per input entry, total O(n).

After we are done processing the input, we can find the first non-marked entry in our boolean array trivially in O(n).


``` Python
def solution(A): 
# Implement your solution here 
n = len(A)
count = (n+1) * [0] 
for i in A: 
	if i > 0: 
		count[i] = 1 

for i in range(1, n+1): 
	if count[i] == 0: 
		return i 

return n+1
pass

```
