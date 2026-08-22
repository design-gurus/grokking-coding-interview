# Pattern: Top K Elements

> Keep a heap of exactly K items, and the cost of finding the top K drops from sorting everything to O(n log k).

## What it is

If you only need the K largest values, sorting all n of them does far more work than the question asked for. A heap of size K holds the current best K, and every new element is compared against the weakest of them in O(log k).

The counterintuitive part: to keep the K **largest** values, you use a **min-heap**. The top of that heap is the smallest of your current winners, which is exactly the one to evict when something better arrives.

## Recognize it when

- The question says top K, K largest, K smallest, K most frequent, or Kth something.
- The answer is a small set out of a large input, and the order inside that set often does not matter.
- Elements arrive over time and the answer has to stay current.
- The brute force is "sort everything, then take the first K".

**Words that give it away:** "top k", "k largest", "k closest", "kth smallest", "most frequent", "in a stream".

## How it works

```
K = 3, keeping the 3 largest of [5, 2, 9, 1, 7]

push 5        heap: [5]
push 2        heap: [2, 5]
push 9        heap: [2, 5, 9]
push 1        heap: [1, 2, 5, 9] -> over size, pop the smallest -> [2, 5, 9]
push 7        heap: [2, 5, 7, 9] -> over size, pop the smallest -> [5, 7, 9]

the top of the heap, 5, is the 3rd largest
```

For "K most frequent", count first with a [hash map](hash-maps.md), then run the same heap over the counts. The pattern is unchanged. Only the value being compared is different.

## The code template

```python
import heapq

def k_largest(nums, k):
    heap = []                            # min-heap holding the k largest so far
    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)          # evict the weakest winner
    return heap                          # heap[0] is the kth largest


def top_k_frequent(nums, k):
    from collections import Counter
    counts = Counter(nums)
    heap = []
    for value, count in counts.items():
        heapq.heappush(heap, (count, value))    # heaps compare the first field
        if len(heap) > k:
            heapq.heappop(heap)
    return [value for count, value in heap]


def k_closest_to_origin(points, k):
    heap = []
    for x, y in points:
        heapq.heappush(heap, (-(x * x + y * y), x, y))   # negated, so a max-heap
        if len(heap) > k:
            heapq.heappop(heap)                          # drops the farthest
    return [(x, y) for _, x, y in heap]
```

Note the direction flip in the third function. To keep the K **closest**, you evict the farthest, so the heap must surface the farthest, so it is a max-heap.

## Complexity

| | |
|---|---|
| Time | O(n log k), against O(n log n) for a full sort |
| Space | O(k), or O(n) when a frequency map is needed first |

## Variations

- K largest or K smallest from an array.
- The Kth largest value, which is just the top of the heap at the end.
- Kth largest in a stream, where the heap is kept between calls.
- K most frequent, and frequency sort.
- K closest points, or K closest numbers to a target.
- Connect ropes with minimum cost, and reorganize a string, which both repeatedly pull the extreme element.
- Quickselect is the O(n) average alternative when you need the Kth element and not the whole top K.

## Problems that use it

Top K Numbers, Kth Smallest Number, K Closest Points to Origin, Connect Ropes, Top K Frequent Numbers, Frequency Sort, Kth Largest Number in a Stream, K Closest Numbers, Rearrange String, Task Scheduler.

## Common mistakes

- Picking the wrong heap direction. For K largest use a min-heap. For K smallest use a max-heap. Getting this backwards evicts the winners.
- Not bounding the heap size, which turns the whole thing back into a full sort with extra steps.
- Forgetting that Python's `heapq` is a min-heap only. Negate the values, or negate the sort key, to get a max-heap.
- Comparing tuples where the second field is not comparable. If two counts tie, the heap tries to compare the values themselves, which throws if they are objects.
- Assuming the heap comes out sorted. It does not. Sort the K results at the end if the problem requires an order.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- The heap itself, from scratch: [Grokking Data Structures](https://www.designgurus.io/course/grokking-data-structures-for-coding-interviews)
