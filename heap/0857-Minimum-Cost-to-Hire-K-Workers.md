# Explanation

[Problem Description](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

For each worker, there is a ratio of wage over quality, i.e. `ratio = wage / quality`. In the wanted group of k workers, for each work, the final wage will be calculated with `quality * max(ratios of all k members)`. So, the wage for all members should be `quality_total * max(ratios of all k members)`

The above statement implies two restrictionis, for the wanted group:

1. least `max(ratios of all k members)` is preferable,
2. least `quality_total` is preferable,

and, the optimization function is minimize the total cost.

# Solution

```python
class Solution:
    def mincostToHireWorkers(self, quality: List[int], wage: List[int], k: int) -> float:
        workers = [(w/q, q) for q, w in zip(quality, wage)]
        workers.sort(key=lambda worker: worker[0])
        quality_wanted = []
        quality_total = 0
        cost_total = float("inf")
        for ratio, q in workers:
            quality_total += q
            heapq.heappush(quality_wanted, -q)
            if len(quality_wanted) > k:
                quality_total += heapq.heappop(quality_wanted)
            if len(quality_wanted) == k:
                cost_total = min(cost_total, quality_total * ratio)
        return cost_total
```

## complexity

- time: 
    
    O(n\*log(n)) for sorting, O(nlog(k)) for the iteration, so the time complexity is O(n*log(n))
  

- space: O(k) for storing the wanted worker group
  