# Pattern: Two Heaps

> Split the numbers into a smaller half and a larger half, and the middle of the data sits at the top of the two heaps, ready to read in O(1).

## What it is

A heap gives you the smallest or the largest element instantly, but nothing about the middle. Two heaps facing each other fix that. A max-heap holds the smaller half, so its top is the biggest of the small numbers. A min-heap holds the larger half, so its top is the smallest of the large numbers. The median is sitting between them.

The cost of keeping this arrangement is one rebalance per insert, which is O(log n).

## Recognize it when

- Numbers arrive one at a time and a query has to be answered after each one.
- The query is about the median, or about the middle of the data.
- You need both "the largest of the small ones" and "the smallest of the large ones" at the same time.
- The problem pairs two lists, and which item you can afford from the second depends on what you have taken from the first.

**Words that give it away:** "median", "data stream", "as numbers arrive", "middle element", "maximize capital", "next interval".

## How it works

```
inserted so far: 1, 3, 5, 7, 9

max-heap (small half)      min-heap (large half)
      5                          7
     / \                        / \
    1   3                      9

top of max-heap = 5, which is the median
```

Keep the two sizes equal, or let one side hold exactly one extra element. After every insert, if the sizes drift apart by more than one, move the top of the bigger heap to the other heap.

Python only has a min-heap, so a max-heap is built by pushing negated values. You will need that in almost every Python solution that uses a max-heap.

## The code template

```python
import heapq

class MedianFinder:
    def __init__(self):
        self.small = []      # max-heap, stored negated
        self.large = []      # min-heap

    def add(self, num):
        heapq.heappush(self.small, -num)                    # always into small first
        heapq.heappush(self.large, -heapq.heappop(self.small))   # move its top over
        if len(self.large) > len(self.small):               # rebalance
            heapq.heappush(self.small, -heapq.heappop(self.large))

    def median(self):
        if len(self.small) > len(self.large):
            return -self.small[0]
        return (-self.small[0] + self.large[0]) / 2
```

Pushing into one heap and immediately moving its top to the other is a shortcut that removes the comparison. The number lands in the right half no matter what it was.

## Complexity

| | |
|---|---|
| Time | O(log n) per insert, O(1) per median query |
| Space | O(n) |

## Variations

- Median of a number stream.
- Sliding window median, which also needs removal. Plain heaps cannot remove from the middle, so use lazy deletion: mark elements as removed and discard them when they reach the top.
- Maximize capital, where a max-heap of profits is fed from a min-heap of costs as your capital grows.
- Next interval, which pairs a heap of starts against a heap of ends.

## Problems that use it

Find the Median of a Number Stream, Sliding Window Median, Maximize Capital, IPO, Next Interval.

## Common mistakes

- Forgetting to rebalance, which lets one heap grow while the other stays empty and destroys the median.
- Being inconsistent about which side holds the extra element when the count is odd. Pick one rule and use it in both the insert and the query.
- Integer division on the two-element median. The median of 3 and 4 is 3.5, not 3.
- Trying to remove an arbitrary element from a heap in a sliding window problem. Use lazy deletion or an [ordered set](ordered-set.md).
- Forgetting to negate consistently when imitating a max-heap with a min-heap.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- The heap itself, from scratch: [Grokking Data Structures](https://www.designgurus.io/course/grokking-data-structures-for-coding-interviews)
