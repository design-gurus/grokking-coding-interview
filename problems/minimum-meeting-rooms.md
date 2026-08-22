# Minimum Meeting Rooms

> Given a list of meetings, find the smallest number of rooms that can hold all of them.

**Pattern:** [Merge Intervals](../patterns/merge-intervals.md) | **Difficulty:** Hard

## The problem

You are given meetings, each with a start and an end time. Two meetings can share a room only if they
do not overlap in time. Return the smallest number of rooms needed so that every meeting has one.

This problem is also known as Meeting Rooms II.

## Why this is a Merge Intervals problem

The input is ranges that may overlap, which puts it in the interval family. But the question is not
"which meetings overlap", it is **how many overlap at the busiest moment**, and that changes the tool.

Merging is wrong here. Merging collapses overlapping meetings into one range and throws away exactly
the number you need. So you keep the interval sort, which is what makes a single sweep possible, and
replace the single held interval with a collection of the meetings currently running.

The collection only ever needs one question answered: has the earliest-finishing running meeting ended
yet? That is a minimum query on a changing set, which is a min-heap.

## The approach

1. Sort the meetings by start time.
2. Keep a min-heap of the end times of the meetings currently in a room.
3. For each meeting in order:
   - Pop every end time that is at or before this meeting's start. Those rooms are now free.
   - Push this meeting's end time. It has taken a room.
4. The answer is the largest the heap ever grew.

The invariant: the heap holds exactly the meetings running at the moment the current one starts, so
its size is the number of rooms in use right then.

There is a second solution worth naming: separate the starts and the ends into two sorted lists, then
sweep both with [two pointers](../patterns/two-pointers.md), adding one on a start and subtracting one
on an end. It is the same idea without a heap, and it is often easier to write correctly.

## Complexity

| | |
|---|---|
| Time | O(n log n), for the sort and for the heap operations |
| Space | O(n) in the worst case, when every meeting overlaps |

## Edge cases to say out loud

- A meeting that ends exactly when the next one starts. Ask whether that frees the room. It usually
  does, and it is the difference between `<=` and `<` in the pop condition.
- An empty list, and a single meeting.
- Meetings that all overlap, where the answer is the count itself.
- Zero-length meetings, if the input allows a start equal to an end.

## Related problems

- [Merge Intervals](merge-intervals.md), the same sort with a different sweep.
- Maximum CPU Load, which is this problem with the heap tracking a running total instead of a count.
- Employee Free Time, which merges first and then reads the gaps.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
