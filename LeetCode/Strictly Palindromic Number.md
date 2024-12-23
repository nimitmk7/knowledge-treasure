## Problem
An integer n is strictly palindromic if, for every base b between 2 and n - 2 (inclusive), the string representation of the integer n in base b is palindromic.

Given an integer n, return true if n is strictly palindromic and false otherwise.

A string is palindromic if it reads the same forward and backward.
## Test Cases
Example 1:

Input: n = 9
Output: false
Explanation: In base 2: 9 = 1001 (base 2), which is palindromic.
In base 3: 9 = 100 (base 3), which is not palindromic.
Therefore, 9 is not strictly palindromic so we return false.
Note that in bases 4, 5, 6, and 7, n = 9 is also not palindromic.
Example 2:

Input: n = 4
Output: false
Explanation: We only consider base 2: 4 = 100 (base 2), which is not palindromic.
Therefore, we return false.

## Solution

```Python

def isStrictlyPalindromic(self, n):

	"""
	
	:type n: int
	
	:rtype: bool
	
	"""	  
	return False
```

- To solve this problem, we must recognize a fundamental insight: for any number n >= 4, it is impossible for n to be strictly palindromic.
- Here's why:  
    Consider a base b = n - 2. In this base, the representation of n would be "12" (because n = 1 * b + 2),  
    which is not a palindrome.
    - Similarly, in the base b = n - 1, the representation of n would be "10", which is also not a palindrome.
    - Thus, no number greater than 3 can be strictly palindromic because in at least one base, the number will not be a palindrome.

