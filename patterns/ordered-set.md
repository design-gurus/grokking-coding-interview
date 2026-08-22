# Pattern: Ordered Set

> A hash set answers "is this value here". An ordered set answers "what is the nearest value above or below", while the collection keeps changing.

## What it is

An ordered set keeps its elements sorted at all times, usually with a balanced binary search tree. Inserting, deleting, and finding a neighbor all cost O(log n).

The two operations that matter are `floor`, the largest element at or below a value, and `ceiling`, the smallest element at or above it. A sorted array can answer those with binary search, but only while nothing is being inserted. An ordered set answers them on a collection that is still being modified.

## Recognize it when

- You need the closest value above or below a target, not an exact match.
- The collection is inserted into or deleted from as the algorithm runs, so re-sorting each time would be too slow.
- Bookings, calendars, or reservations where a new item may clash with an existing one.
- A sliding window that needs both its largest and its smallest value at once.

**Words that give it away:** "closest", "just greater than", "just smaller than", "book if it does not clash", "next available", "in a changing set".

## How it works

The insight for interval problems: a new interval can only clash with its immediate neighbors.

```
existing bookings, kept sorted:  [10,20]  [30,40]  [50,60]

new booking [35,45]
  floor by start   -> [30,40]   does it end after 35? yes, clash
  ceiling by start -> [50,60]   does it start before 45? no

two lookups settle it, no scan of the whole set
```

Language support varies, and interviewers expect you to know this about your own language. Java has `TreeMap` and `TreeSet`. C++ has `std::set` and `std::map`. Python has no built-in ordered set, so say so and name what you would use instead. The `sortedcontainers` library, a heap plus lazy deletion, or a plain sorted list with `bisect` when the input is small enough for O(n) inserts.

## The code template

```python
from bisect import bisect_left, insort

class Calendar:
    """A sorted list of intervals. insort is O(n), which is fine for small inputs.
    For large ones, use a balanced tree: TreeMap in Java, std::map in C++,
    SortedList from sortedcontainers in Python."""

    def __init__(self):
        self.bookings = []                      # kept sorted by start

    def book(self, start, end):
        position = bisect_left(self.bookings, (start, end))

        before = self.bookings[position - 1] if position > 0 else None
        after = self.bookings[position] if position < len(self.bookings) else None

        if before and before[1] > start:        # the previous one ends too late
            return False
        if after and after[0] < end:            # the next one starts too early
            return False

        insort(self.bookings, (start, end))
        return True
```

## Complexity

| | |
|---|---|
| Time | O(log n) for insert, delete, floor, and ceiling with a balanced tree |
| Space | O(n) |

## Variations

- A sorted map from key to value, rather than a bare set.
- A set of non-overlapping intervals, for calendars and range assignment.
- A multiset, or a map from value to count, when duplicates have to be kept.
- A sliding window that needs both extremes, where elements are removed as the window moves.
- Order statistics, meaning "how many elements are smaller than this", which needs a [binary indexed tree](binary-indexed-tree.md) rather than a plain set.

## Problems that use it

My Calendar I, My Calendar II, 132 Pattern, Merge Similar Items, Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit, Contains Duplicate III, Sliding Window Median.

## Common mistakes

- Losing duplicates. A set stores each value once, so a multiset or a count map is needed when repeats matter.
- Not handling the ends. `floor` of the smallest element and `ceiling` of the largest both return nothing, and that needs a check before use.
- Re-sorting the list after every insert, which is O(n log n) per operation and slower than the brute force it was meant to replace.
- Assuming Python has an ordered set. It does not, and saying so out loud is a point in your favor.
- Using an ordered set when a [heap](top-k-elements.md) is enough. If you only ever need the single largest element, a heap is simpler and faster.

## Go deeper

- This pattern's introduction in the course: [Introduction to Ordered Set Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-ordered-set-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
