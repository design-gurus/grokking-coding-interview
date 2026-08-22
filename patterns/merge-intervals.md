# Pattern: Merge Intervals

> Sort the intervals by start time, and a problem about overlapping ranges becomes a single left-to-right sweep.

## What it is

An interval is a pair of numbers, a start and an end. Once the list is sorted by start time, you only ever have to compare the interval in front of you against the one you are currently holding. Everything before that has already been settled, and everything after starts even later.

That is the reason the sort comes first. Without it, any two intervals in the list might overlap, and you are back to comparing every pair.

## Recognize it when

- The input is a list of pairs that represent ranges: times, dates, positions, versions.
- The task is to merge, insert, intersect, count conflicts, or find free time.
- The problem is about meetings, calendars, bookings, or scheduling.
- Two ranges either overlap or they do not, and that relationship drives the answer.

**Words that give it away:** "intervals", "meetings", "overlap", "merge", "conflict", "free time", "rooms", "start and end time".

## How it works

Sort by start. Hold the current interval. For each next interval, ask one question: does it start at or before the end of the interval you are holding?

```
sorted:  [1,4]  [2,5]  [7,9]

hold [1,4], see [2,5]:  2 <= 4, they overlap, extend to [1,5]
hold [1,5], see [7,9]:  7 > 5, no overlap, emit [1,5] and hold [7,9]
result:  [1,5]  [7,9]
```

For "how many rooms do I need", the sweep is different. Keep a min-heap of the end times of the meetings currently running. Before starting a new meeting, pop every meeting that has already ended. The largest the heap ever gets is the answer.

## The code template

```python
def merge(intervals):
    if not intervals:
        return []
    intervals.sort(key=lambda pair: pair[0])       # sort by start time
    merged = [list(intervals[0])]
    for start, end in intervals[1:]:
        last_end = merged[-1][1]
        if start <= last_end:                     # overlaps the one being held
            merged[-1][1] = max(last_end, end)    # max, because [1,9] then [2,3]
        else:
            merged.append([start, end])
    return merged


def min_meeting_rooms(meetings):
    import heapq
    meetings.sort(key=lambda pair: pair[0])
    running = []                                   # min-heap of end times
    for start, end in meetings:
        while running and running[0] <= start:     # these meetings have finished
            heapq.heappop(running)
        heapq.heappush(running, end)
    return len(running) if running else 0
```

Note the `max` in the merge step. An interval fully inside another one, like [2,3] inside [1,9], would otherwise shorten the interval you are holding.

## Complexity

| | |
|---|---|
| Time | O(n log n), dominated by the sort |
| Space | O(n) for the output, or O(1) extra if merging in place |

## Variations

- Merge all overlapping intervals.
- Insert one interval into an already sorted list, which needs no sort and runs in O(n).
- Intersect two lists of intervals, using [two pointers](two-pointers.md) across the two lists.
- Minimum meeting rooms, with a min-heap of end times.
- Employee free time, which is the gaps left after merging.
- Remove the fewest intervals to make the rest non-overlapping, which is a [greedy](greedy-algorithms.md) problem sorted by end time.

## Problems that use it

Merge Intervals, Insert Interval, Intervals Intersection, Conflicting Appointments, Minimum Meeting Rooms, Maximum CPU Load, Employee Free Time, Non-overlapping Intervals.

## Common mistakes

- Not deciding what to do with touching intervals like [1,3] and [3,5]. Ask the interviewer whether they count as overlapping. Both answers are defensible, and the code differs by one comparison.
- Extending the held interval to the new end instead of the maximum of the two ends.
- Sorting by end time when the problem needs start time, or the reverse. Merging sorts by start. Greedy interval selection sorts by end.
- Forgetting to emit the last held interval after the loop finishes.

## Go deeper

- This pattern's introduction in the course: [Introduction to Merge Intervals Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-merge-intervals-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
