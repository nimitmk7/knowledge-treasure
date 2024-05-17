You are given a sequence of n integers $a_{0}, a_{1}, . . . , a_{n−1}$ and the task is to find the slice with the largest sum. More precisely, we are looking for two indices $p, q$ such that the total $a_{p} + a_{p+1} + . . . + a_{q}$ is maximal. We assume that the slice can be empty and its sum equals 0.

![[Pasted image 20240513075331.png | Slice with largest sum example]]

## $O(n^3)$ complexity solution
The simplest approach is to analyze all the slices and choose the one with the largest sum.

```Python
def slow_max_slice(A):
	n = len(A)
	result = 0
	for p in range(n):
		sum = 0
		for i in range(p, q+1):
			sum += A[i]
		result = max(result, sum)
	return result
```

Analyzing all possible slices → $O \left(n^2 \right)$ complexity
Compute the total → $O \left(n \right)$ complexity

Straight-forward but far from optimal. 
## $O(n^2)$ solution

We can easily improve our last solution using **prefix sum**. Notice that the prefix sum allows the sum of any slice to be computed in a constant time. With this approach, the time complexity of the whole algorithm reduces to $O(n^2)$. We assume that pref is an array of prefix sums $(pref_{i} = a_0 + a_1 + . . . + a_{i−1})$.

```Python
def quadratic_max_slice(A, pref):
	n = len(A)
	result = 0
	for p in range(n):
		for q in rangee(p, n):
			sum = pref[q+1] - pref[p]
			result = max(result, sum)
	return result
```

We can also solve this problem without using prefix sums, within the same time complexity. 

Assume that we know the sum of slice $(p, q)$, so $s = a_p + a_{p+1} + . . . + a_q$ . 
The sum of the slice with one more element $(p, q + 1)$ equals $s + a_{q+1}$. 

Following this observation, there is no need to compute the sum each time from the beginning; we can use the previously calculated sum.

```Python
def quadratic_max_slice(A): 
	n = len(A), result = 0 
	for p in xrange(n):
		sum = 0
		for q in xrange(p, n):
			sum += A[q]
			result = max(result, sum)
	return result
```

## Solution with O(n) time complexity

This problem can be solved even faster. For each position, we compute the largest sum that ends in that position. 

If we assume that the maximum sum of a slice ending in position $i$ equals $max \_ending$, then the maximum slice ending in position $i+1$ equals $max(0, \max\_ending + a_{i+1})$.

```Python

def golden_max_slice(A): 
	max_ending = max_slice = 0
	for a in A:
		max_ending = max(0, max_ending + a)
		max_slice = max(max_slice, max_ending)
	return max_slice
```
 We are okay with adding negative numbers to the slice as long as there are positive numbers ahead which compensate for it. In that case, max_slice will not be equal to max_ending if we add a -ve number and it is less than max_ending. 

But as soon as the positive numbers that follow are enough in magnitude to compensate for the negative numbers, max_ending increases and the max_slice is updated to a higher value. 
 
 Positive numbers are always welcome because they only contribute to the sum.
