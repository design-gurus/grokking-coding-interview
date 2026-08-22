# Merge Intervals

> Combine every set of overlapping intervals into one, and return the intervals that remain.

**Pattern:** [Merge Intervals](../patterns/merge-intervals.md) | **Difficulty:** Medium

## The problem

You are given a list of intervals, each a start and an end. Some of them overlap. Return a list where
every group of overlapping intervals has been replaced by the single interval that covers the group.
The input is in no particular order.

## Why this is a Merge Intervals problem

The input is pairs of numbers that represent ranges, which is the shape itself. But the useful cue is
what makes the problem hard before you sort and easy after.

Unsorted, any interval might overlap any other, so checking them all is O(n²). Sorted by start time,
you only ever have to compare the interval in front of you against the one you are currently holding.
Everything earlier is settled, and everything later starts even later, so it cannot reach back past
what you are holding.

That reduction, from "compare everything to everything" to "compare each item to one running value",
is what the sort buys, and it is the argument to state before writing code.

## The approach

1. Sort the intervals by start time.
2. Hold the first interval as the current one.
3. For each following interval, compare its start against the current end.
   - If the start is at or before the current end, they overlap. Extend the current end to the
     **maximum** of the two ends.
   - Otherwise there is a gap. Emit the current interval and hold the new one.
4. Emit whatever is still held when the list runs out.

The invariant: the current interval covers every interval seen so far that overlaps it, and every
interval already emitted is final.

## Complexity

| | |
|---|---|
| Time | O(n log n), the sort dominates. The sweep after it is O(n). |
| Space | O(n) for the output, or O(1) extra if you merge in place |

## Edge cases to say out loud

- **Touching intervals**, like [1,3] and [3,5]. Ask whether these count as overlapping. Both answers
  are defensible, and the code differs by one comparison, `<` against `<=`.
- **One interval inside another**, like [2,3] inside [1,9]. This is the case that catches people who
  set the current end to the new end rather than to the maximum of the two.
- An empty list, and a list of one interval.
- Intervals given as end before start, if the problem allows malformed input.
- Forgetting to emit the last held interval, which silently drops one from the answer.

## Related problems

- Insert Interval, where the list is already sorted, so no sort is needed and the sweep is O(n).
- Intervals Intersection, which walks two sorted lists with [two pointers](../patterns/two-pointers.md).
- [Minimum Meeting Rooms](minimum-meeting-rooms.md), which sorts by start and then needs a heap of end
  times rather than a single running interval.
- Non-overlapping Intervals, which sorts by **end** time, because it is a
  [greedy](../patterns/greedy-algorithms.md) selection rather than a merge.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
