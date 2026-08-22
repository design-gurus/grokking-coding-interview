# Pattern: Prefix Sum

> Compute the running totals once, and the sum of any range becomes the difference between two of them.

## What it is

A prefix sum array holds, at each position, the total of everything up to that point. Building it takes one pass. After that, the sum of the range from i to j is `prefix[j] - prefix[i-1]`, which is one subtraction rather than a loop.

The second, more powerful use is counting. If two positions have the same running total, everything between them sums to zero. If they differ by K, everything between them sums to K. That turns "count the subarrays summing to K" into a single pass with a [hash map](hash-maps.md).

## Recognize it when

- The same array is queried for range sums many times, and the array does not change.
- You are counting subarrays whose sum meets a condition, rather than finding just one.
- The wording mentions a running total, a balance point, a pivot index, or an equilibrium.
- The brute force is a nested loop over every start and every end.

**Words that give it away:** "range sum", "subarray sum equals", "how many subarrays", "pivot index", "divisible by k", "submatrix sum".

## How it works

```
array:   [1, 2, 3, 4]
prefix:  [0, 1, 3, 6, 10]      with a leading zero

sum of indexes 1 to 2  =  prefix[3] - prefix[1]  =  6 - 1  =  5
```

The leading zero is not decoration. It removes the special case where the range starts at index 0, and that special case is where most off-by-one bugs live.

For counting, walk the array keeping the running total, and ask the map how many earlier positions had a total of `running - k`. Every one of them marks the start of a qualifying subarray.

## The code template

```python
def build_prefix(nums):
    prefix = [0] * (len(nums) + 1)                 # leading zero
    for i, num in enumerate(nums):
        prefix[i + 1] = prefix[i] + num
    return prefix                                  # sum(i..j) = prefix[j+1] - prefix[i]


def count_subarrays_with_sum(nums, k):
    from collections import defaultdict
    seen = defaultdict(int)
    seen[0] = 1                                    # the empty prefix, do not forget it
    running, count = 0, 0
    for num in nums:
        running += num
        count += seen[running - k]                 # every earlier match starts one
        seen[running] += 1
    return count


def count_subarrays_divisible_by(nums, k):
    """Same shape, but the key is the remainder rather than the total."""
    from collections import defaultdict
    seen = defaultdict(int)
    seen[0] = 1
    running, count = 0, 0
    for num in nums:
        running = (running + num) % k              # Python's modulo is never negative
        count += seen[running]
        seen[running] += 1
    return count
```

## Complexity

| | |
|---|---|
| Time | O(n) to build, then O(1) per range query |
| Space | O(n) |

## Variations

- Plain range sums on a fixed array.
- Counting subarrays that hit a target, using a map of running total to how often it has been seen.
- Grouping by remainder, for divisible-by-K problems.
- Two-dimensional prefix sums, for submatrix queries, using inclusion and exclusion of four corners.
- Prefix XOR and prefix product, which work the same way as long as the operation can be undone.
- Difference arrays, which apply the idea in reverse: many range updates, then one pass to read the final values.

## Problems that use it

Find the Middle Index in Array, Subarray Sum Equals K, Subarray Sums Divisible by K, Maximum Size Subarray Sum Equals k, Binary Subarrays With Sum, Range Sum Query Immutable, Product of Array Except Self, Continuous Subarray Sum.

## Common mistakes

- Forgetting to seed the map with a zero entry, which misses every subarray that starts at index 0.
- Off-by-one on the subtraction. Decide whether your prefix array has a leading zero, write the formula down, and stick to it.
- Using prefix sums on an array that changes. Once elements are updated, you need a [binary indexed tree](binary-indexed-tree.md) or a [segment tree](segment-tree.md).
- Negative remainders in languages where the modulo operator keeps the sign of the dividend. In Java and C++, use `((x % k) + k) % k`.
- Overflow, since running totals grow with the whole array.
- Reaching for prefix sums when a [sliding window](sliding-window.md) would do. A window works when all the values are positive and you need one best subarray. Prefix sums are for counting, and for arrays with negative values.

## Go deeper

- This pattern's introduction in the course: [Introduction Prefix Sum Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-prefix-sum-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
