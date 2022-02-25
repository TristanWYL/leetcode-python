# Explanation

[Problem Description](https://leetcode.com/problems/find-median-from-data-stream/)

Intuitively, maintaining a sorted list could solve this problem. However, manipulating the list lead to extra complexity. So we apply HEAP here.

Using two heaps:
 - heap_max: which is a list stores a heap with a max at its top

 - heap_min: which is a list stores a heap with a min at its top

Dynamically, ensure the following two criteria all the time along with adding numbers:

1. heap_max has a length equal to or larger by 1 than length of heap_min,
   
2. max in heap_max is smaller than min in heap_min,

In this case, the median is the max of heap_max, or mean of max of heap_max and min of heap_min.

**NOTE**

Python only support HEAP with min at its top, so we store the negative of the number in heap_max to make it act as a HEAP with max at its top.

# Solution

```python
class MedianFinder:

    def __init__(self):
        self.heap_min: List[int] = []
        self.heap_max: List[int] = []

    def addNum(self, num: int) -> None:
        if len(self.heap_max) == len(self.heap_min):
            heapq.heappush(self.heap_max, -heapq.heappushpop(self.heap_min, num))
        else:
            heapq.heappush(self.heap_min, -heapq.heappushpop(self.heap_max, -num))

    def findMedian(self) -> float:
        if len(self.heap_max) > len(self.heap_min):
            return -self.heap_max[0]
        else:
            return (-self.heap_max[0] + self.heap_min[0]) / 2

# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```

## complexity

- time: O(log(n)) for addNum, O(1) for findMedian
- space: O(n)
  