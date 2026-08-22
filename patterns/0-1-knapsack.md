# Pattern: 0/1 Knapsack (Dynamic Programming)

> One yes-or-no decision per item, and the best answer for each remaining capacity is built from the best answers to smaller capacities.

## What it is

You have items, each with a weight and a value, and a bag that can hold a limited weight. Each item is either taken or left. The name comes from that: 0 or 1 copies of each item.

This needs dynamic programming rather than a [greedy](greedy-algorithms.md) choice. Taking the item with the best value-to-weight ratio can fill the bag in a way that blocks two better items. You cannot know without trying both branches, so you compute both and keep the better one.

A large family of problems is this pattern with different wording. "Can I split this array into two halves with equal sums" is really "can I fill a bag of capacity total/2 exactly".

## Recognize it when

- Each element is used at most once, and you choose a subset.
- There is a budget, a capacity, a target sum, or a count you cannot exceed.
- You are maximizing or minimizing a total, or asking whether a total is reachable, or counting the ways to reach it.
- The input is too large to enumerate all 2^n subsets, but the capacity is a manageable number.

**Words that give it away:** "subset", "partition", "target sum", "capacity", "at most", "exactly", "maximum value", "can you make".

## How it works

The state is (items considered so far, capacity left). The transition is a choice.

```
dp[i][c] = the best value using the first i items with capacity c

skip item i:   dp[i-1][c]
take item i:   dp[i-1][c - weight[i]] + value[i]     if it fits

dp[i][c] = max of the two
```

Only the previous row is ever read, so the 2D table collapses to a 1D array. When it does, the capacity loop must run **backwards** for 0/1. Running it forwards would let the same item be taken twice, which is the unbounded version of the problem.

## The code template

```python
def knapsack(weights, values, capacity):
    dp = [0] * (capacity + 1)
    for i in range(len(weights)):
        for c in range(capacity, weights[i] - 1, -1):   # backwards for 0/1
            dp[c] = max(dp[c], dp[c - weights[i]] + values[i])
    return dp[capacity]


def can_partition(nums):
    """Split into two subsets with equal sums."""
    total = sum(nums)
    if total % 2:
        return False
    target = total // 2
    reachable = [False] * (target + 1)
    reachable[0] = True                                 # zero is always reachable
    for num in nums:
        for c in range(target, num - 1, -1):
            reachable[c] = reachable[c] or reachable[c - num]
    return reachable[target]
```

## Complexity

| | |
|---|---|
| Time | O(n × C), where C is the capacity |
| Space | O(n × C) for the full table, O(C) once collapsed to one row |

Note that O(n × C) is pseudo-polynomial. It depends on the size of the capacity number, not just how many items there are, which is why a capacity of one billion breaks this approach.

## Variations

- Maximize value under a weight limit.
- Subset sum, which asks only whether a total is reachable.
- Equal subset partition, and minimum subset sum difference.
- Count the subsets that reach a target.
- Target sum, where every number gets a plus or a minus, which rearranges into a subset sum.
- Unbounded knapsack, where items can be reused. Same table, but the capacity loop runs forwards. Rod cutting, coin change, and minimum coin change all live there.

## Problems that use it

0/1 Knapsack, Equal Subset Sum Partition, Subset Sum, Minimum Subset Sum Difference, Count of Subset Sum, Target Sum, Partition Array Into Two Arrays to Minimize Sum Difference, Last Stone Weight II.

## Common mistakes

- Running the capacity loop forwards in the 1D version, which silently solves the unbounded problem instead.
- Confusing 0/1 with unbounded. Read whether an item can be reused. It changes one loop direction and nothing else.
- Getting the base row wrong. A capacity of zero has value zero, and a sum of zero is always reachable with the empty subset.
- Building the table when only the yes-or-no answer is needed, which wastes memory. A boolean row is enough for subset sum.
- Using this pattern when the capacity is enormous. Check the constraints. If capacity is 10^9, the intended answer is something else.
- Forgetting that the values may be negative, which breaks the assumption that a bigger capacity is never worse.

## Go deeper

- This pattern's introduction in the course: [Introduction to 0/1 Knapsack Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-01-knapsack-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- Every dynamic programming pattern, in depth: [Grokking Dynamic Programming](https://www.designgurus.io/course/grokking-dynamic-programming)
