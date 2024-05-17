The Sieve of Eratosthenes is a very simple and popular technique for finding all the prime numbers in the range from 2 to a given number $n$. The algorithm takes its name from the process of sieving—in a simple way we remove multiples of consecutive numbers.

## Procedure
Initially, we have the set of all the numbers $\{2, 3, . . . , n\}$. At each step we choose the smallest number in the set and remove all its multiples. 

Notice that every composite number has a divisor of at most √n. In particular, it has a divisor which is a prime number. It is sufficient to remove only multiples of prime numbers not exceeding √n. In this way, all composite numbers will be removed.

![[Pasted image 20240511104059.png | Seiving for $n = 17$]]

The above algorithm can be slightly improved. Notice that we needn’t cross out multiples of i which are less than $i^2$. Such multiples are of the form $k · i$, where $k < i$. These have already been removed by one of the prime divisors of $k$. 

After this improvement, we obtain the following implementation:

```Python
def sieve(n):
	sieve = [True] * (n+1)
	sieve[0]= sieve[1] = False
	i = 2
	# Iterate on divisors till sqrt(n)
	while (i * i <= n):
		if(sieve[i]):
			k = i*i
		# For the divisor, iterate from i sq. to n
		while(k <= n)
			sieve[k] = False
			k+=i
	i+=1
	return sieve
```

Let’s analyse the time complexity of the above algorithm. For each prime number $p_{j} \le \sqrt n$ we cross out at most $n$ $p_{j}$ numbers, so we get the following number of operations:

$$\frac{n}{2} + \frac{n}{3} + \frac{n}{5} + \cdots = \sum_{p_j \le\sqrt{n}} \frac{n}{p_{j}} = n \cdot\sum_{p_j \le\sqrt{n}} \frac{1}{p_{j}}$$
The sum of the reciprocals of the primes $p_j \le n$ equals asymptotically O(log log n). So the overall time complexity of this algorithm is $O(n \log \log n)$. The proof is not trivial, and is beyond the scope of this article.


## References
1. https://codility.com/media/train/9-Sieve.pdf 