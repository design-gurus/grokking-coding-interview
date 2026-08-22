# Pattern: Modified Binary Search

> Binary search is not about sorted arrays. It works wherever you can look at the middle and rule out half the remaining space.

## What it is

The textbook version searches a sorted array for a value. The pattern is more general. If you can stand at the midpoint and answer "is what I want to the left or to the right", you can halve the search space every step. That works whatever the space is.

That space does not have to be the input. In "binary search on the answer", you search the range of possible answers, and the test at the midpoint is "is this answer good enough". The array is never searched at all.

## Recognize it when

- The input is sorted, or rotated, or has a peak, or is otherwise monotonic in some direction.
- The expected complexity is O(log n), or the input is far too large to scan.
- You are asked for a boundary rather than a value: the first element greater than X, the last occurrence, the insertion point, the ceiling.
- The question asks for the smallest capacity, the slowest speed, or the minimum days that still works. That is answer-space search, and the test is a simulation.

**Words that give it away:** "sorted", "rotated", "log n", "first occurrence", "ceiling", "peak", "minimum capacity", "smallest k such that", "infinite array".

## How it works

```
find the first element >= 5 in [1, 3, 5, 5, 8]

low=0 high=4 mid=2 value 5   good enough, remember it, search left
low=0 high=1 mid=0 value 1   too small, search right
low=1 high=1 mid=1 value 3   too small, search right
low=2 high=1                 loop ends, the answer is index 2
```

For boundary questions, do not return as soon as you find a match. Record it and keep searching the side that might hold an earlier one.

For answer-space search, the shape is different. Write a helper that answers "does this candidate work". Make sure that answer is monotonic, meaning that once a candidate works, every larger candidate works too. Then binary search the candidate range.

## The code template

```python
def search_range_start(arr, target):
    """The first index where arr[i] >= target, or len(arr) if none."""
    low, high = 0, len(arr) - 1
    answer = len(arr)
    while low <= high:
        mid = low + (high - low) // 2         # avoids overflow in fixed-width languages
        if arr[mid] >= target:
            answer = mid                      # a candidate, but keep looking left
            high = mid - 1
        else:
            low = mid + 1
    return answer


def min_eating_speed(piles, hours):
    """Answer-space search: the smallest speed that finishes in time."""
    def hours_needed(speed):
        return sum((pile + speed - 1) // speed for pile in piles)

    low, high = 1, max(piles)
    while low < high:
        mid = low + (high - low) // 2
        if hours_needed(mid) <= hours:        # works, so try slower
            high = mid
        else:
            low = mid + 1                     # too slow
    return low
```

## Complexity

| | |
|---|---|
| Time | O(log n), or O(n log range) for answer-space search where each test costs O(n) |
| Space | O(1) |

## Variations

- Order-agnostic search, where you do not know if the array ascends or descends.
- Ceiling, floor, next letter, and the insertion point.
- First and last occurrence of a repeated value.
- Search in a rotated sorted array, and finding the rotation point.
- Find a peak element, where the comparison is against the neighbor rather than a target.
- Search in an infinite or unbounded array, where you first double the bounds until you overshoot.
- Binary search on the answer: minimum capacity, slowest speed, smallest largest sum.
- Two-dimensional search in a sorted matrix.

## Problems that use it

Order-agnostic Binary Search, Ceiling of a Number, Next Letter, Number Range, Search in a Sorted Infinite Array, Minimum Difference Element, Bitonic Array Maximum, Search in Rotated Array, Rotation Count, Koko Eating Bananas, Capacity to Ship Packages.

## Common mistakes

- The infinite loop. If `low` or `high` can be set to `mid` without moving, the loop never ends. Either move past the midpoint, or make sure the interval always shrinks.
- Mixing the two loop styles. `while low <= high` pairs with `mid + 1` and `mid - 1`. `while low < high` pairs with `mid + 1` and `mid`. Pick one and keep it consistent.
- Writing `(low + high) / 2` in a fixed-width language, which can overflow. `low + (high - low) / 2` cannot.
- Returning on the first match in a boundary problem, which gives you some occurrence rather than the first one.
- Answer-space search on a test that is not monotonic. If a larger candidate can fail after a smaller one succeeded, binary search is invalid.
- Forgetting that a rotated array has one sorted half at every step. That is what the comparison has to identify.

## Go deeper

- This pattern's introduction in the course: [Introduction to Modified Binary Search Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-modified-binary-search-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
