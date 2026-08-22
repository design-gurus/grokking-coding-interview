# Pattern: Binary Indexed Tree (Fenwick Tree)

> A prefix sum array you can update. Twenty lines of code give you O(log n) updates and O(log n) prefix queries.

## What it is

A binary indexed tree, also called a Fenwick tree, is a flat array where each slot holds the sum of a block of the original array. The size of each block is decided by the lowest set bit of the slot's index. That is why the whole structure needs no pointers and almost no code.

It does the same job as a [segment tree](segment-tree.md) for sums, in a fraction of the lines. The limitation is that the operation must be reversible, because a range sum is computed by subtracting one prefix from another. Sums and XORs qualify. Minimums and maximums do not.

## Recognize it when

- You need prefix sums on an array whose values keep changing.
- The problem counts how many earlier elements are smaller or larger than the current one: inversions, smaller numbers after self, reverse pairs.
- The values are large but sparse, so they can be compressed into a small set of ranks first.
- You are about to write a segment tree for plain sums, and time is short.

**Words that give it away:** "range sum with updates", "count of smaller elements", "inversions", "cumulative frequency", "how many before this one".

## How it works

The expression `i & -i` isolates the lowest set bit of i, which is the size of the block that slot i covers.

```
index:  1    2    3    4    5    6    7    8
covers: [1] [1-2] [3] [1-4] [5] [5-6] [7] [1-8]

query prefix up to 7:   7 -> 6 -> 4 -> 0
                        add tree[7], tree[6], tree[4], then stop

update index 5:         5 -> 6 -> 8 -> past the end
                        add the delta to tree[5], tree[6], tree[8]
```

Queries walk down by clearing the lowest set bit. Updates walk up by adding it. Both take at most log n steps, because each step removes or carries one bit.

Counting smaller elements is the second use, and the one that appears more often in interviews. Treat the array as a frequency table over values rather than positions. Insert each value as you sweep, and query the prefix below it to learn how many smaller values you have already passed.

## The code template

```python
class BIT:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)          # 1-indexed, index 0 is unused

    def update(self, index, delta):
        """index is 1-based."""
        while index <= self.n:
            self.tree[index] += delta
            index += index & -index        # walk up

    def prefix_sum(self, index):
        """Sum of 1..index, 1-based."""
        total = 0
        while index > 0:
            total += self.tree[index]
            index -= index & -index        # walk down
        return total

    def range_sum(self, left, right):
        return self.prefix_sum(right) - self.prefix_sum(left - 1)


def count_smaller_after_self(nums):
    """Sweep from the right, asking how many smaller values are already inserted."""
    ranks = {value: i + 1 for i, value in enumerate(sorted(set(nums)))}
    bit = BIT(len(ranks))
    answer = []
    for value in reversed(nums):
        answer.append(bit.prefix_sum(ranks[value] - 1))
        bit.update(ranks[value], 1)
    return answer[::-1]
```

## Complexity

| | |
|---|---|
| Update | O(log n) |
| Prefix query | O(log n) |
| Build | O(n log n) by repeated update, or O(n) with a direct construction |
| Space | O(n) |

## Variations

- Prefix sums with point updates, which is the standard use.
- Range updates with point queries, using a difference array inside the tree.
- Range updates with range queries, which needs two trees.
- Two-dimensional trees for submatrix sums.
- Order statistics over compressed coordinates, which is how counting problems use it.
- Prefix XOR, which works because XOR is its own inverse.

## Problems that use it

Range Sum Query Mutable, Count of Smaller Numbers After Self, Count of Range Sum, Reverse Pairs, Number of Longest Increasing Subsequence, Create Sorted Array through Instructions, Global and Local Inversions.

## Common mistakes

- Using index 0. The structure is 1-indexed, because `i & -i` is zero when i is zero and the update loop would never move. Shift every index up by one.
- Using it for minimum or maximum. Range queries here rely on subtraction, and a minimum cannot be undone. Use a segment tree.
- Forgetting coordinate compression when the values are large. A value of one billion would need a tree of one billion slots, while the ranks of a thousand distinct values need only a thousand.
- Passing an absolute value to `update` where a delta is expected. The update adds, it does not assign. To set a value, add the difference from the old one.
- Getting the sweep direction wrong in counting problems. "How many smaller elements come after this one" needs a right-to-left sweep.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
