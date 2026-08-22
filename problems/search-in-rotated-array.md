# Search in Rotated Array

> Find a target in a sorted array that has been rotated at an unknown point, in O(log n).

**Pattern:** [Modified Binary Search](../patterns/modified-binary-search.md) | **Difficulty:** Medium

## The problem

You are given an array that was sorted in ascending order and then rotated at some unknown pivot, so
`[1,2,3,4,5]` might arrive as `[4,5,1,2,3]`. Return the index of a target value, or -1 if it is not
there. The expected cost is O(log n).

## Why this is a Modified Binary Search problem

The stated O(log n) is the loudest cue in the problem. Nothing that touches every element can be
logarithmic, so a scan is out before you start.

The array is not sorted, which seems to rule out binary search, and this is the insight the problem is
built on: **one half of a rotated array is always sorted**. Cut anywhere, and either the left part or
the right part is in plain ascending order, because there is only one rotation point and it can only
be in one of the halves.

Once you know which half is sorted, you can tell in one comparison whether the target lies inside it.
If it does, search there. If it does not, search the other half. Either way you have discarded half
the array, which is all binary search ever needs.

## The approach

1. Set `low` to 0 and `high` to the last index.
2. While `low` is at or below `high`:
   - Compute `mid`. If the value there is the target, return `mid`.
   - Decide which side is sorted by comparing the value at `low` against the value at `mid`.
   - If the left side is sorted: if the target lies between the value at `low` and the value at `mid`,
     move `high` to `mid - 1`. Otherwise move `low` to `mid + 1`.
   - If the right side is sorted: if the target lies between the value at `mid` and the value at
     `high`, move `low` to `mid + 1`. Otherwise move `high` to `mid - 1`.
3. Return -1.

The invariant: the target, if it exists, is always inside the range from `low` to `high`.

## Complexity

| | |
|---|---|
| Time | O(log n) |
| Space | O(1) |

## Edge cases to say out loud

- An array rotated zero times, which is just sorted, and one rotated n times, which is the same array.
- A single element, and an empty array.
- The target sitting exactly at `low`, `mid`, or `high`. The range comparisons need to be inclusive at
  the right ends, and this is where off-by-one bugs live.
- **Duplicate values.** If duplicates are allowed, the comparison at `low` and `mid` can no longer tell
  you which half is sorted, as in `[3,1,3,3,3]`. The usual fix is to advance `low` by one when the two
  are equal, which makes the worst case O(n). Say this even if the problem promises distinct values.
- Computing the midpoint as `(low + high) / 2` in a fixed-width language, which can overflow. Use
  `low + (high - low) / 2`.

## Related problems

- Rotation Count, which finds the pivot itself with the same halving logic.
- Find Minimum in Rotated Sorted Array, the same search for the smallest value.
- Search in a Sorted Infinite Array, where you first double the bounds until you overshoot, then
  search normally.
- Koko Eating Bananas, the other family in this pattern, where you binary search the **answer** rather
  than the input.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
