https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/description/

## Intuition
**Min. capacity possible**: Max weight of a package in the belt. 

Otherwise, we can’t ship all packages, as the max weight package will be left out. Requires less ship capacity, but a lot of days.

**Max capacity possible**: Sum of all weights

This way, we can ship all packages in a single day. But we need a lot of capacity, which is not ideal.

So using binary search, we find the optimal value between these 2 values, which is less than the given D days. 

## Code

```Python
class Solution(object):
    def shipWithinDays(self, weights, days):
        """
        :type weights: List[int]
        :type days: int
        :rtype: int
        """
        maxWeight, totalWeight = -1, 0
        weight_sum = 0
        for weight in weights:
            maxWeight = max(maxWeight, weight)
            totalWeight += weight
        left, right = maxWeight, totalWeight

        while left < right:
            mid = (left + right) // 2
            daysNeeded, currWeight = 1, 0
            for weight in weights:
                if currWeight + weight > mid:
                    daysNeeded += 1
                    currWeight = 0
                currWeight += weight
            if daysNeeded > days:
                left = mid+1
            else:
                right = mid
        return left

```
