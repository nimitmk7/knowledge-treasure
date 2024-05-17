The Euclidean algorithm is one of the oldest numerical algorithms still to be in common use. It solves the problem of computing the greatest common divisor (gcd) of two positive integers.

``` python
def gcd(a, b):
	if a % b == 0:
		return b
	else
		return gcd(b, a%b)
```


