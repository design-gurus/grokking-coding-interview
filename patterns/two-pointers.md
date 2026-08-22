# Pattern: Two Pointers

> Two indices walk the same array instead of two nested loops, which turns an O(n²) scan into a single O(n) pass.

## What it is

Two Pointers keeps two positions in the same array or string and moves them under a rule. Because each pointer only ever moves forward, or only ever moves inward, the whole array is covered in one pass. The pattern replaces the nested loop that compares every element against every other element.

The rule that moves the pointers is what makes it correct. In a sorted array, moving the left pointer right always increases the sum, and moving the right pointer left always decreases it. That is a decision you can make without looking at the rest of the array.

## Recognize it when

- The input is sorted, or you are allowed to sort it and the sort cost is acceptable.
- You are looking for a pair, a triplet, or a subarray that meets a condition on its values.
- The brute force is two nested loops over the same array.
- The problem asks you to do it in place, or in O(1) extra space, which rules out a hash map.
- The array has two meaningful ends, like the two walls in a container problem.

**Words that give it away:** "sorted array", "pair that sums to", "triplet", "remove duplicates in place", "without using extra space", "two ends".

## How it works

Put one pointer at each end. Compare what they point at against the target. If the result is too small, the only move that helps is the left pointer going right. If the result is too large, the right pointer goes left. Stop when the pointers meet.

```
target = 7
[1, 3, 4, 6, 8, 10]
 L               R      1 + 10 = 11, too big, move R left
[1, 3, 4, 6, 8, 10]
 L           R          1 + 8 = 9, too big, move R left
[1, 3, 4, 6, 8, 10]
 L        R             1 + 6 = 7, found it
```

The same-direction variant works differently. One pointer reads every element, and the other marks where the next kept element should be written. That is how remove-duplicates and partition problems work in place.

## The code template

```python
def pair_with_target_sum(arr, target):
    """arr is sorted ascending. Returns the two indices, or [-1, -1]."""
    left, right = 0, len(arr) - 1
    while left < right:
        current = arr[left] + arr[right]
        if current == target:
            return [left, right]
        if current < target:
            left += 1        # the only move that increases the sum
        else:
            right -= 1       # the only move that decreases the sum
    return [-1, -1]


def remove_duplicates(arr):
    """Same-direction variant. Returns the length of the deduplicated prefix."""
    write = 1
    for read in range(1, len(arr)):
        if arr[read] != arr[write - 1]:
            arr[write] = arr[read]
            write += 1
    return write
```

## Complexity

| | |
|---|---|
| Time | O(n), or O(n log n) if you have to sort first |
| Space | O(1) |

## Variations

- Opposite ends, converging. Pair sums, container with most water, reversing in place.
- Same direction, one reading and one writing. Remove duplicates, move zeroes, partition.
- One pointer fixed while two others converge. This is how 3Sum works: fix the first number, then run the standard scan on the rest of the array.
- Two pointers over two different arrays. Merging two sorted arrays, or intersecting them.

## Problems that use it

Pair with Target Sum, Remove Duplicates, Squaring a Sorted Array, Triplet Sum to Zero, Triplet Sum Close to Target, Subarrays with Product Less than a Target, Dutch National Flag, Container With Most Water, Trapping Rain Water.

## Common mistakes

- Using opposite-end pointers on an unsorted array. The direction rule is only valid because the array is sorted. On unsorted input you usually want a [hash map](hash-maps.md) instead.
- Forgetting to skip duplicate values in 3Sum, which returns the same triplet several times.
- Sorting when the problem needs the original indices, which the sort destroys. Sort pairs of value and index if you need both.
- Moving both pointers in one step when only one should move, which steps over the answer.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
