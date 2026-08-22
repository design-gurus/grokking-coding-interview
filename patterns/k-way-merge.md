# Pattern: K-way Merge

> Hold one candidate from each sorted list in a heap, and the next smallest element across all of them is always at the top.

## What it is

Merging two sorted lists is easy with [two pointers](two-pointers.md): compare the two heads and take the smaller one. With K lists, comparing K heads every step costs O(K) per element.

A min-heap fixes that. Put the head of every list into the heap. The smallest of all K heads is at the top, so pop it, and push the next element from whichever list it came from. The heap always holds exactly one candidate per list.

## Recognize it when

- The input is several sorted lists, arrays, or streams.
- The output is one merged sorted sequence, or the Kth element across all of the inputs.
- A matrix is given whose rows and columns are both sorted, which is K sorted lists described differently.
- The problem asks for a range that touches every list at once.

**Words that give it away:** "k sorted lists", "merge", "sorted matrix", "kth smallest across", "smallest range covering".

## How it works

```
lists:  [1, 4, 9]   [2, 5]   [3, 6]

heap holds (value, which list, which index)
(1,A) (2,B) (3,C)   pop 1, push 4 from A
(2,B) (3,C) (4,A)   pop 2, push 5 from B
(3,C) (4,A) (5,B)   pop 3, push 6 from C
...
```

The critical part of each heap entry is not the value. It is the bookkeeping that says which list the value came from, so you know where to get its replacement.

## The code template

```python
import heapq

def merge_k_sorted_lists(lists):
    heap = []
    for list_index, values in enumerate(lists):
        if values:                                    # skip empty lists
            heapq.heappush(heap, (values[0], list_index, 0))

    merged = []
    while heap:
        value, list_index, position = heapq.heappop(heap)
        merged.append(value)
        next_position = position + 1
        if next_position < len(lists[list_index]):
            heapq.heappush(heap, (lists[list_index][next_position],
                                  list_index, next_position))
    return merged


def smallest_range(lists):
    """The smallest range that contains at least one number from every list."""
    heap, current_max = [], float('-inf')
    for list_index, values in enumerate(lists):
        heapq.heappush(heap, (values[0], list_index, 0))
        current_max = max(current_max, values[0])

    best = [float('-inf'), float('inf')]
    while len(heap) == len(lists):                    # stop when a list runs out
        value, list_index, position = heapq.heappop(heap)
        if current_max - value < best[1] - best[0]:
            best = [value, current_max]
        next_position = position + 1
        if next_position < len(lists[list_index]):
            next_value = lists[list_index][next_position]
            current_max = max(current_max, next_value)
            heapq.heappush(heap, (next_value, list_index, next_position))
    return best
```

## Complexity

| | |
|---|---|
| Time | O(N log K), where N is the total number of elements and K the number of lists |
| Space | O(K) for the heap |

## Variations

- Merge K sorted linked lists, or K sorted arrays.
- Kth smallest element across K lists, which stops after K pops instead of draining the heap.
- Kth smallest in a sorted matrix, where each row is a list.
- The smallest range covering at least one element from every list, which tracks the running maximum alongside the heap.
- K pairs with the smallest sums, where the "lists" are generated on demand rather than given.

## Problems that use it

Merge K Sorted Lists, Kth Smallest Number in M Sorted Lists, Kth Smallest Number in a Sorted Matrix, Smallest Number Range, K Pairs with Largest Sums, Merge Two Sorted Lists.

## Common mistakes

- Pushing an empty list's head, which throws an index error before the merge even starts.
- Forgetting to record which list an element came from, which leaves you unable to push its successor.
- Pushing the entire list into the heap instead of one element per list. It works, but it costs O(N) space and gives up the whole point of the pattern.
- In the smallest-range problem, stopping when the heap empties rather than when one list runs out. Once a list is exhausted, no range can cover all of them.
- Comparing tuples whose second element is not comparable, which throws when two values tie.

## Go deeper

- This pattern's introduction in the course: [Introduction to K-way Merge Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-kway-merge-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
