# Pattern: Segment Tree

> A tree over the array where each node stores the aggregate of a range, so both queries and updates cost O(log n) instead of one being O(n).

## What it is

[Prefix sums](prefix-sum.md) answer range queries in O(1), but only while the array never changes. One update and the whole prefix array has to be rebuilt.

A segment tree keeps both operations cheap. Each leaf holds one array element, and each internal node holds the combined value of its two children. A query touches at most O(log n) nodes, and an update walks one path from a leaf to the root.

Unlike a [binary indexed tree](binary-indexed-tree.md), a segment tree works for any operation that can be combined, including minimum and maximum, which cannot be undone by subtraction.

## Recognize it when

- The array changes **and** ranges of it are queried, both many times.
- The aggregate is a minimum, a maximum, a greatest common divisor, or something else that cannot be reversed.
- A whole range is updated at once, for example "add 5 to everything between i and j".
- The brute force is O(n) per query and the constraints show that is too slow.

**Words that give it away:** "range sum query mutable", "update and query", "range minimum", "add to a range", "q queries with updates".

## How it works

```
array:            [1, 3, 5, 7]

                 sum 16
                /      \
            sum 4      sum 12
            /   \       /   \
           1     3     5     7

query the range from index 1 to 2:
  the node for [0,1] partly overlaps, so go down: take 3
  the node for [2,3] partly overlaps, so go down: take 5
  answer 8, after touching a handful of nodes
```

Every query recursion hits one of three cases. No overlap, so return the identity value. Total overlap, so return the stored aggregate. Partial overlap, so recurse into both children and combine.

For range updates, lazy propagation stores a pending change at a node and only pushes it down to the children when someone actually looks at them. Without it, updating a range of a million elements touches a million leaves.

## The code template

```python
class SegmentTree:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (4 * self.n)      # 4n is the safe size
        self._build(nums, 1, 0, self.n - 1)

    def _build(self, nums, node, left, right):
        if left == right:
            self.tree[node] = nums[left]
            return
        middle = (left + right) // 2
        self._build(nums, node * 2, left, middle)
        self._build(nums, node * 2 + 1, middle + 1, right)
        self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

    def update(self, index, value, node=1, left=0, right=None):
        if right is None:
            right = self.n - 1
        if left == right:
            self.tree[node] = value
            return
        middle = (left + right) // 2
        if index <= middle:
            self.update(index, value, node * 2, left, middle)
        else:
            self.update(index, value, node * 2 + 1, middle + 1, right)
        self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

    def query(self, want_left, want_right, node=1, left=0, right=None):
        if right is None:
            right = self.n - 1
        if want_right < left or right < want_left:
            return 0                        # no overlap: the identity for sum
        if want_left <= left and right <= want_right:
            return self.tree[node]          # total overlap
        middle = (left + right) // 2
        return (self.query(want_left, want_right, node * 2, left, middle)
                + self.query(want_left, want_right, node * 2 + 1, middle + 1, right))
```

To switch from sums to minimums, change the combine step to `min` and the no-overlap return value to infinity. Everything else stays the same.

## Complexity

| | |
|---|---|
| Build | O(n) |
| Query | O(log n) |
| Update | O(log n) |
| Space | O(4n) for the array-backed version |

## Variations

- Range sum, minimum, maximum, or greatest common divisor, with point updates.
- Range updates with lazy propagation.
- Counting inversions or smaller elements, over compressed coordinates.
- A merge sort tree, where each node holds a sorted list rather than one value.
- Two-dimensional segment trees for submatrix queries.
- Persistent segment trees, which keep every past version.

## Problems that use it

Range Sum Query Mutable, Range Minimum Query, Count of Smaller Numbers After Self, Falling Squares, My Calendar III, Longest Increasing Subsequence (the O(n log n) counting version), Rectangle Area II.

## Common mistakes

- Sizing the array as 2n. Use 4n. The tree is not always perfectly balanced and 2n overflows on some inputs.
- Returning the wrong identity for the no-overlap case. Zero for sums, infinity for minimums, negative infinity for maximums. Returning zero for a minimum query silently gives wrong answers on positive arrays.
- Forgetting to push a lazy value down before recursing into the children, so the children answer with stale data.
- Recomputing the parent from its children only in `build` and not in `update`, which leaves the whole path above the leaf stale.
- Reaching for a segment tree when a [binary indexed tree](binary-indexed-tree.md) would do. If the aggregate is a sum and the updates are point updates, the shorter structure is easier to write correctly under time pressure.

## Go deeper

- This pattern's introduction in the course: [Introduction to Segment Tree Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-segment-tree-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
