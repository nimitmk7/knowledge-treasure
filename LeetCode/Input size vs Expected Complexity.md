To determine the expected time complexity for LeetCode problems based on input size, follow these guidelines derived from competitive programming practices and platform constraints:

## Input Size vs. Acceptable Complexity
**General rule**: Your algorithm's operations should stay under ~10⁸ per second (standard judge system throughput). This creates clear boundaries for acceptable complexities:

| Input Size (N) | Maximum Acceptable Complexity | Example Operations |
| -------------- | ----------------------------- | ------------------ |
| N ≤ 10         | O(N!)                         | 3.6M ops at N=10   |
| N ≤ 20         | O(2ᴺ)                         | 1M ops at N=20     |
| N ≤ 100        | O(N⁴)                         | 100M ops at N=100  |
| N ≤ 1,000      | O(N²)                         | 1M ops at N=1k     |
| N ≤ 10⁴        | O(N²)                         | 100M ops at N=1e4  |
| N ≤ 10⁵        | O(N log N)                    | 500k ops at N=1e5  |
| N ≤ 10⁶        | O(N)                          | 1M ops at N=1e6    |
| N > 10⁶        | O(1) or O(log N)              | Constant-time ops  |

**Common LeetCode patterns**:
- Array problems with N=1e5 usually require O(N) or O(N log N) solutions
- Matrix problems with N=100-300 often need O(N³) approaches
- Tree traversal with N=1e4 nodes typically uses O(N) DFS/BFS

## Adjustment Factors
1. **Operation cost**: Sorting (O(N log N)) counts as 1 "operation" but costs more than simple array access
2. **Problem constraints**: Some problems explicitly require specific complexities
3. **Hidden constants**: Algorithms with O(1B) constants might fail even with "good" complexity

When solving:
4. Check input constraints in the problem statement
5. Calculate maximum possible operations (N^2, N log N, etc.)
6. Ensure result stays under 1e8 operations threshold
7. Test edge cases manually (e.g., N=1e5 input with O(N²) = 1e10 operations would timeout)

Example: For N=1e4, O(N²) = 100M operations (acceptable), but O(N² log N) ≈ 400M operations (might timeout).

---
Answer from Perplexity: pplx.ai/share