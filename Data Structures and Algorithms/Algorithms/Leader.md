 Let us consider a sequence $a_{0}, a_{1}, . . . , a_{n−1}$. The leader of this sequence is the element whose value occurs more than $\frac{n}{2}$ times.

![Leader is highlighted in gray](leader.png)

Notice that the sequence can have at most one leader. If there were two leaders then their total occurrences would be more than $2 · \frac{n}{2} = n$, but we only have $n$ elements.

**Task**: Find the value of the leader of the sequence $a_{0}, a_{1}, . . . , a_{n−1}$, such that $0 \le a_{i} \le 10^9.$ If there is no leader, the result should be $-1$.

> [!tip] Floor division
> In Python, the `//` symbol is used to indicate floor division operator. 

## $O(n^2)$ time solution

Count occurrences of every element. 

```Python
def slowLeader(A):
	n = len(A)
	leader = -1
	for k in xrange(n):
		candidate = A[k]
		count = 0
		for i in xrange(n):
			if (A[i] == candidate):
				count += 1
		if (count > n // 2):
			leader = candidate
	return leader
```

## $O(n \log n)$ time solution
If the sequence is presented in non-decreasing order, then identical values are adjacent to each other. Having sorted the sequence, we can easily count slices of the same values and find the leader in a smarter way. 

![[Pasted image 20240509075051.png]]

Notice that if the leader occurs somewhere in our sequence, then it must occur at index $\frac{n}{2}$ (the central element). This is because, given that the leader occurs in more than half the total values in the sequence, there are more leader values than will fit on either side of the central element in the sequence. 

```Python
def fastLeader(A):
	n = len(A)
	leader = -1
	A.sort()
    candidate = A[n // 2]
	count = 0
	for i in xrange(n):
		if (A[i] == candidate):
			count += 1
	if (count > n // 2):
		leader = candidate
	return leader
```

The time complexity of the above algorithm is $O(n \log n)$ due to the sorting time.

## $O(n)$ time solution

Notice that if the sequence $a_{0}, a_{1}, . . . , a_{n−1}$ contains a leader, then after removing a pair of elements of different values, the remaining sequence still has the same leader. 

- **Case 1**: Both the removed elements are not the leader.
	- The proportion of the leader just increased in the sequence.
- **Case 2**: One of the removed elements is the leader.
	- The leader in the new sequence occurs more than $\frac{n}{2} − 1 = \frac{n−2}{2}$ times. Consequently, it is still the leader of the new sequence of $n − 2$ elements.

![[Pasted image 20240509075530.png | Case 2 example]]

Removing pairs of different elements is not trivial. Let’s create an empty stack onto which we will be pushing consecutive elements. 

After each such operation we check whether the two elements at the top of the stack are different. If they are, we remove them from the stack. This is equivalent to removing a pair of different elements from the sequence.

![[Pasted image 20240509075649.png | Algorithm example]]

In fact, we don’t need to remember all the elements from the stack, because all the values below the top are always equal. It is sufficient to remember only the values of elements and the size of the stack.

```Python
def goldenLeader(A):
	n = len(A)
	size = 0
	for k in xrange(n):
		if size == 0:
			size += 1
			value = A[k]
		else:
			if(value != A[k])
				size -= 1
			else
				size += 1
		candidate = -1
		if (size > 0):
			candidate = value
		leader = -1
		count = 0
		for k in xrange(n):
			if (A[k] == candidate):
				count += 1
		if (count > n // 2)
			leader = candidate
	return leader

```

At the beginning we notice that if the sequence contains a leader, then after the removal of different elements the leader will not have changed. After removing all pairs of different elements, we end up with a sequence containing all the same values. This value is not necessarily the leader; it is only a candidate for the leader. 

Finally, we should iterate through all the elements and count the occurrences of the candidate; if it is greater than $\frac{n}{2}$ then we have found the leader; otherwise the sequence does not contain a leader. The time complexity of this algorithm is $O(n)$ because every element is considered only once. The final counting of occurrences of the candidate value also works in $O(n)$ time.

## References
1. https://codility.com/media/train/6-Leader.pdf
2. 