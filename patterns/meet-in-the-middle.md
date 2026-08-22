# Pattern: Meet in the Middle

> Split the input in half, enumerate each half on its own, then match the two halves against each other. The cost falls from 2^n to about 2^(n/2).

## What it is

Some problems have no better answer than trying every subset, and 2^40 is far too many. Splitting the input into two halves of 20 gives two enumerations of about a million each, which is fine, plus a matching step.

The saving is enormous. 2^40 is roughly a trillion. Two runs of 2^20 is roughly two million. The pattern buys that by paying for a sorted list or a hash map to join the halves.

## Recognize it when

- The brute force is "try every subset" and n is around 30 to 45.
- The values are too large for a [knapsack](0-1-knapsack.md) table, so the pseudo-polynomial answer is out. A target sum of 10^9 with 40 items is the classic setup.
- The problem asks for a subset sum equal to, or closest to, a target.
- n is small but not small enough. That gap is the signal, since below about 25, plain enumeration or backtracking is simpler.

**Words that give it away:** "n is at most 40", "subset sum", "closest to the target", "choose any number of items", together with values far too large to index.

## How it works

```
items: [a, b, c, d]  split into [a, b] and [c, d]

all sums of the first half:   0, a, b, a+b
all sums of the second half:  0, c, d, c+d

for each sum s in the second half, look for target - s in the first half
```

If you need an exact match, a hash set makes the lookup O(1). If you need the closest value, sort the first half and binary search it, which is where the log factor comes from.

## The code template

```python
from bisect import bisect_left

def closest_subset_sum(nums, target):
    middle = len(nums) // 2
    left_sums = all_subset_sums(nums[:middle])
    right_sums = sorted(all_subset_sums(nums[middle:]))

    best = float('inf')
    for total in left_sums:
        needed = target - total
        position = bisect_left(right_sums, needed)
        for candidate in (position - 1, position):        # check both neighbors
            if 0 <= candidate < len(right_sums):
                best = min(best, abs(target - (total + right_sums[candidate])))
    return best


def all_subset_sums(items):
    sums = [0]
    for item in items:
        sums += [existing + item for existing in sums]
    return sums
```

## Complexity

| | |
|---|---|
| Time | O(2^(n/2) × n/2) to enumerate, plus a log factor if the join needs sorting |
| Space | O(2^(n/2)) |

## Variations

- Subset sum equal to a target, joined with a hash set.
- Closest subset sum, joined with a sorted list and binary search.
- Counting the subsets that reach a target, by counting matches instead of stopping at the first.
- Splitting into two groups whose sums are as close as possible.
- Bidirectional search on a graph, which is the same idea applied to paths: search forward from the start and backward from the goal until the two frontiers touch.

## Problems that use it

Subset Sum Equal to Target, Subsets having Sum between A and B, Closest Subsequence Sum, Partition Array Into Two Arrays to Minimize Sum Difference, Find the Minimum Possible Sum, Ways to Reach a Target Score.

## Common mistakes

- Using it when it is not needed. Below about n equal to 25, plain [backtracking](backtracking.md) with pruning is simpler and fast enough. Above about 45, even the halves are too big.
- Getting the join wrong. This is the hardest part, and the most common error is checking only one side of the binary search result. The closest value can be on either side of the insertion point.
- Enumerating the halves and then comparing every pair, which is 2^n again and throws away the whole saving.
- Forgetting the empty subset. It has a sum of zero and it is a legal choice on both sides.
- Running out of memory. Storing 2^25 sums is already tens of millions of entries.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
