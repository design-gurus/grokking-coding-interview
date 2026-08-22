# Find the Median of a Number Stream

> Numbers arrive one at a time. Return the median at any point, without re-sorting.

**Pattern:** [Two Heaps](../patterns/two-heaps.md) | **Difficulty:** Medium

## The problem

Design a structure with two operations. One inserts a number. The other returns the median of every
number inserted so far. For an odd count the median is the middle number, and for an even count it is
the average of the two middle numbers.

## Why this is a Two Heaps problem

Two words in the statement decide it: **stream** and **median**.

"Stream" means the answer is asked for repeatedly, as the data grows, so any solution that redoes work
per query is wrong. Sorting on each query is O(n log n) per call. Keeping a sorted list makes the query
O(1) but each insert costs O(n) to shift elements.

"Median" means you need the middle, and a single heap cannot give you that. A heap surfaces one
extreme, and the middle is the one thing it hides.

Two heaps facing each other put the middle where a heap can see it. A max-heap holds the smaller half,
so its top is the largest small number. A min-heap holds the larger half, so its top is the smallest
large number. The median is exactly at that boundary.

## The approach

1. Keep a max-heap for the smaller half and a min-heap for the larger half.
2. On insert, push the number into the max-heap, then immediately move that heap's top into the
   min-heap. That places the number in the correct half without comparing anything.
3. Rebalance: if the min-heap now holds more than the max-heap, move its top back.
4. To read the median: if the heaps differ in size, the median is the top of the larger one. If they
   are equal, average the two tops.

The invariant: every number in the max-heap is at or below every number in the min-heap, and the sizes
differ by at most one.

Most languages give you a min-heap only. A max-heap is built by pushing negated values, and the sign
has to be flipped back consistently on the way out.

## Complexity

| | |
|---|---|
| Insert | O(log n) |
| Median query | O(1) |
| Space | O(n) |

## Edge cases to say out loud

- The very first insert, where one heap is empty.
- An even count, where the median is an average. Integer division here is the most common bug: the
  median of 3 and 4 is 3.5, not 3.
- Duplicate values, which are fine and can sit in either half.
- Negative numbers, which matter if you built the max-heap by negating.
- Which side holds the extra element when the count is odd. Pick a rule and use the same one in both
  the insert and the query.

## Related problems

- Sliding Window Median, which also has to **remove** the number leaving the window. Heaps cannot
  remove from the middle, so this needs lazy deletion, meaning you mark an element as removed and
  discard it when it reaches the top, or an [ordered set](../patterns/ordered-set.md).
- Maximize Capital, where a min-heap of costs feeds a max-heap of profits as your capital grows.
- [Kth Largest Number in a Stream](../patterns/top-k-elements.md), the same streaming shape when you
  need a single extreme rather than the middle.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
