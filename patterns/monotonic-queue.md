# Pattern: Monotonic Queue

> A deque kept in sorted order, so the maximum of a sliding window is always at the front and every window costs O(1) to answer.

## What it is

A [monotonic stack](monotonic-stack.md) can only be touched at one end. A monotonic queue is a double-ended queue, so elements can leave at both ends. That second exit is what makes it work with a sliding window.

Two rules maintain it. At the back, pop anything smaller than the incoming value. A smaller value that arrived earlier can never be the maximum again while the new one is in the window. At the front, pop anything that has slid out of the window.

## Recognize it when

- A sliding window is combined with a maximum or minimum query at every position.
- The problem says "for every window of size k" and asks for an extreme value.
- The brute force is O(n × k): scan every window for its maximum.
- A dynamic programming transition looks back over a bounded range and takes the best value in it.

**Words that give it away:** "sliding window maximum", "every window of size k", "maximum in each subarray", "at most k apart", "shortest subarray with sum at least".

## How it works

```
window size 3, array [1, 3, -1, -3, 5]
deque holds indices, values decreasing

1     back: nothing smaller, push        deque values: [1]
3     back: 1 < 3, pop it, push 3        deque values: [3]
-1    push                               deque values: [3, -1]     window max 3
-3    push                               deque values: [3, -1, -3] window max 3
5     front: 3 has slid out, pop it
      back: -3 and -1 are smaller, pop both, push 5
                                         deque values: [5]         window max 5
```

Store indices, not values. Deciding whether the front has left the window needs its position.

## The code template

```python
from collections import deque

def sliding_window_maximum(nums, k):
    window = deque()                  # indices, values decreasing
    result = []
    for i, num in enumerate(nums):
        while window and window[0] <= i - k:
            window.popleft()          # the front has slid out of the window
        while window and nums[window[-1]] <= num:
            window.pop()              # smaller and older, so never the max again
        window.append(i)
        if i >= k - 1:
            result.append(nums[window[0]])
    return result
```

For a minimum instead of a maximum, flip one comparison: pop from the back while the stored value is greater than or equal to the incoming one.

## Complexity

| | |
|---|---|
| Time | O(n). Every index is pushed once and popped once. |
| Space | O(k) |

## Variations

- Sliding window maximum or minimum.
- Shortest subarray with a sum of at least K, which runs a monotonic deque over the [prefix sums](prefix-sum.md) and handles negative numbers, which a plain window cannot.
- Constrained subsequence sum, and jump game VI, where the deque holds the best dynamic programming value in the reachable range.
- A window that has to satisfy a limit on the difference between its largest and smallest values, which needs two deques, one for each extreme.

## Problems that use it

Sliding Window Maximum, Shortest Subarray with Sum at Least K, Constrained Subsequence Sum, Jump Game VI, Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit, Max Value of Equation.

## Common mistakes

- Storing values instead of indices, which leaves you unable to tell when the front has left the window.
- Popping from the wrong end. Expired elements leave from the front, dominated elements leave from the back.
- Getting equal values wrong. Whether you pop on `<` or `<=` decides which of two equal values survives, and it matters when duplicates sit on a window boundary.
- Recording an answer before the first full window exists. The first result appears at index k minus 1.
- Reaching for a heap. A heap also gives the maximum, but removing an element that has left the window costs O(n) unless you add lazy deletion. The deque is the cleaner answer.

## Go deeper

- This pattern's introduction in the course: [Introduction to Monotonic Queue Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-monotonic-queue-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
