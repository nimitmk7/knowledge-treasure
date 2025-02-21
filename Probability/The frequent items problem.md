# k-Frequent Items (Heavy-Hitters)
## Problem Statement

Consider a stream of $n$ items $x_{1} , . . . , x_{n}$ with duplicates. These items take $u$ possible values.

Return any item at appears at least $\frac{n}{k}$ times.
$x_{1} , x_{2} , x_{3} , x_{4} , x_{5} , x_{6} , . . .$
$v_{10} , v_{10} , v_{2} , v_{5} , v_{10} , v_{2}, . . .$

We want very fast detection, without having to scan through
database/logs. I.e., want to maintain a running list of frequent items
that appear in a stream of data items.


# Applications
- Finding top/viral items (i.e., products on Amazon, videos
watched on Youtube, Google searches, etc.)
- Finding very frequent IP addresses sending requests (to detect
DoS attacks/network anomalies).
- ‘Iceberg queries’ for all items in a database with frequency
above some threshold.

# Approximate Frequent Items
## Motivation
We can prove that no algorithm using $o(u)$ space can output just the items with frequency $\ge \frac{n}{k}$ . We will only be able to solve the problem approximately.

## Problem statement
**$(ϵ, k)$-Frequent Items Problem**: Consider a stream of n items $x_{1} , . . . , x_{n}$. 
Return a set of items F, ==including all items that appear $\ge \frac{n}{k}$ times== and only items that appear $\ge (1-\epsilon)\frac{n}{k}$ times.

The deterministic Misra-Gries algorithm solves this problem using $O(k/ϵ)$ space. We need a randomized algorithm that matches this, and is more flexible in many settings.

## Count-min Sketch
A random-hashing based method for the frequent elements problem. 

### Point-Query Problem
The algorithm addresses the slightly different problem of estimating the frequency of any item $v$ in the stream. Formally, let $f(v) = \sum_{i=1}^n \mathbb{1}[x_i=v]$ be the number of times item $v$ appears in the stream. 

Here, we use the notation $\mathbb{1}[x_i=v]$ to denote the indicator random variable that the $i$th item in the stream is $v$. An indicator random variable is 1 if the event happens and 0 otherwise.

**Goal**: Return estimate $\hat{f}(v)$ such that $f(v) \leq \hat{f}(v) \leq f(v) +\frac{\epsilon}{k} n$ with high probability. 

Solving Frequent items: Just return all items for which $\hat{f}(v) \geq \frac{n}{k}$.
## Method
![[Pasted image 20250131085123.png]]

**Count-Min Update**: 
- Choose random hash function $h$ mapping to ${1, . . . , m}$. 
- For $i = 1, . . . , n$
	-  Given item $x_i$ , set $A[h(x_{i})] = A[h(x_{i})] + 1$

![[Pasted image 20250131084923.png]]
We want to estimate the frequency of item v, $f(v) = \sum^n_{i=1} \mathbb{1}[x_{i} = v]$. To do this using our small space “sketch” A, return $\hat{f}(v) = A[h(v)]$.

![[Pasted image 20250131085612.png]]

**Claim 1**: We always have $A[h(v)] ≥ f(v)$. Why?
Hash function Collisions. Our estimate for the frequency contains the true frequency plus an error term for all the other items that are hashed to the same cell.

$\hat{f}(v) = f(v) + \sum_{y \in \mathcal{U} \setminus v} f(y)\mathbb{1}[h(y)=h(v)]$

$A[h(v)] = f(v) + \sum_{y \neq v} \mathbb{1}[h(y)=h(v)] \cdot f(y)$


