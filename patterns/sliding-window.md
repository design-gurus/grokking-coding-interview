# Pattern: Sliding Window

> Carry the answer for one window forward to the next one, so a range of the array costs O(1) per step instead of being recomputed from scratch.

## What it is

A window is a contiguous stretch of the array or string, marked by a left and a right index. The right edge moves forward to take in a new element, and the left edge moves forward to drop one. Instead of recomputing the sum, the count, or the frequency map for every window, you update it as elements enter and leave.

That update is the whole pattern. Adding the element that enters and removing the element that leaves costs O(1), so the entire scan costs O(n) rather than O(n × k).

## Recognize it when

- The answer is a contiguous subarray or substring, not a scattered set of elements.
- The problem asks for the longest, the shortest, or the maximum over all windows.
- A size K is given, or a condition describes when a window is valid ("at most K distinct characters", "no repeats", "contains all of T").
- The brute force is "for every start, for every end, recompute".

**Words that give it away:** "subarray", "substring", "contiguous", "window of size k", "longest", "smallest", "at most k distinct".

## How it works

Both indices start at 0. The right index moves forward on every iteration, and the left index moves only when the window has to shrink.

```
target sum >= 8      [2, 1, 5, 2, 3, 2]
                      L  R                 sum 3
                      L     R              sum 8, valid, record length 3, shrink
                         L  R              sum 6, invalid, grow
                         L     R           sum 8, valid, record length 3, shrink
```

Fixed-size windows shrink on every step, so left and right move together after the first K elements. Dynamic windows shrink only while the condition says to, which is why the shrink is a `while` loop and not an `if`.

## The code template

```python
def smallest_subarray_with_given_sum(arr, target):
    """Dynamic window: grow on the right, shrink on the left while still valid."""
    window_sum, start, best = 0, 0, float('inf')
    for end in range(len(arr)):
        window_sum += arr[end]                     # the element entering
        while window_sum >= target:                # shrink while still valid
            best = min(best, end - start + 1)
            window_sum -= arr[start]               # the element leaving
            start += 1
    return best if best != float('inf') else 0


def max_sum_subarray_of_size_k(arr, k):
    """Fixed window: after the first k elements, one in and one out per step."""
    window_sum, best = 0, 0
    for end in range(len(arr)):
        window_sum += arr[end]
        if end >= k - 1:
            best = max(best, window_sum)
            window_sum -= arr[end - k + 1]         # slide the left edge
    return best
```

## Complexity

| | |
|---|---|
| Time | O(n). Each index enters the window once and leaves once. |
| Space | O(1), or O(k) when the window keeps a frequency map |

## Variations

- Fixed size, where K is given.
- Dynamic size, where a condition decides when to shrink.
- A frequency map inside the window for character or value counts.
- "Exactly K distinct" solved as "at most K" minus "at most K minus 1".
- A window plus a [monotonic queue](monotonic-queue.md) when you also need the maximum inside the window.

## Problems that use it

Maximum Sum Subarray of Size K, Smallest Subarray with a Given Sum, Longest Substring with K Distinct Characters, Fruits into Baskets, Longest Substring Without Repeating Characters, Longest Substring with Same Letters after Replacement, Permutation in a String, Smallest Window containing Substring.

## Common mistakes

- Forgetting to undo the aggregate when the left edge moves. Every add needs a matching remove.
- Writing `if` instead of `while` for the shrink step, which stops shrinking too early.
- Confusing "at most K distinct" with "exactly K distinct". They are different problems, and the second is usually built from the first.
- Off-by-one in the window length. The length of the window from `start` to `end` inclusive is `end - start + 1`.
- Leaving zero counts in the frequency map, so the "how many distinct" check counts characters that are no longer in the window.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
