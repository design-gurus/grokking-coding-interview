# Pattern: Linear Sorting Algorithms

> No comparison sort can beat O(n log n). If you stop comparing and start using the values themselves as positions, you can sort in O(n).

## What it is

Comparison sorts have a proven lower bound of O(n log n), because there are n! possible orderings and each comparison only halves the possibilities.

Counting sort escapes that bound by not comparing anything. If the values are integers from 0 to k, you count how many of each there are and write them back out in order. Radix sort applies counting sort digit by digit. Bucket sort spreads values into ranges and sorts each range separately.

The escape has a price: it only works when the values are constrained.

## Recognize it when

- The values are integers in a small, stated range, like 0 to 1000 or 1 to n.
- The input is lowercase letters, digits, or another small fixed alphabet.
- The problem asks for O(n) sorting, or the constraints make O(n log n) too slow.
- The array has only a few distinct values, as in sorting an array of 0s, 1s, and 2s.

**Words that give it away:** "values are between 0 and", "sort in linear time", "lowercase English letters", "colors", "ages", "the numbers are small".

## How it works

```
counting sort of [2, 0, 2, 1]  with values 0 to 2

counts:      [1, 1, 2]              one 0, one 1, two 2s
write back:  [0, 1, 2, 2]
```

For a stable sort, which matters when the values carry extra data, do not just write the counts back. Turn them into running totals, then walk the input **backwards** and place each element at the position its running total points to. Stability is what makes radix sort work at all, since each pass must preserve the order produced by the previous one.

Sorting three distinct values has an even better answer: the Dutch national flag partition, which is one pass with [three pointers](two-pointers.md) and no extra memory.

## The code template

```python
def counting_sort(nums, max_value):
    counts = [0] * (max_value + 1)
    for num in nums:
        counts[num] += 1
    out, index = nums[:], 0
    for value, count in enumerate(counts):
        for _ in range(count):
            out[index] = value
            index += 1
    return out


def sort_colors(nums):
    """Dutch national flag: sort 0s, 1s and 2s in one pass, no extra memory."""
    low, mid, high = 0, 0, len(nums) - 1
    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:
            nums[mid], nums[high] = nums[high], nums[mid]
            high -= 1                    # do not advance mid, the swapped value is unseen
    return nums
```

## Complexity

| | |
|---|---|
| Counting sort | O(n + k) time, O(k) space, where k is the size of the value range |
| Radix sort | O(d × (n + k)) time, where d is the number of digits |
| Bucket sort | O(n) on average, O(n²) in the worst case when everything lands in one bucket |

## Variations

- Counting sort, for a small integer range.
- Radix sort, least significant digit first or most significant digit first.
- Bucket sort, for values spread evenly over a range.
- Pigeonhole sort, for a dense range with no duplicates.
- Dutch national flag, for exactly three groups.
- Counting sort used as a step, as in sorting by frequency after the counts are built.

## Problems that use it

Sort Colors, Relative Sort Array, Height Checker, Maximum Gap, Array Partition, Sort an Array, H-Index.

## Common mistakes

- Using counting sort on a huge value range. A range of a billion needs a billion slots, whatever the length of the array.
- Losing stability in radix sort. If the inner sort is not stable, the earlier digit passes are destroyed and the result is wrong.
- Forgetting to shift when the values can be negative. Add an offset so the smallest value maps to index 0.
- Advancing the middle pointer after swapping with the high pointer in the Dutch national flag partition. The value that just arrived has not been looked at yet.
- Choosing a bucket size that puts everything in one bucket, which collapses bucket sort to its worst case.
- Reaching for this when the language's built-in sort is fast enough. Say why linear sorting is possible, then use it only if it actually matters.

## Go deeper

- This pattern's introduction in the course: [Introduction to Linear Sorting Algorithms](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-linear-sorting-algorithms)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
