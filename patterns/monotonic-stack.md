# Pattern: Monotonic Stack

> Keep the stack sorted, and every element you pop has just been answered by the element that pushed it out.

## What it is

A monotonic stack is a stack whose values are kept in order, either always increasing or always decreasing from bottom to top. Before pushing a new value, you pop everything that would break that order.

The pops are the point. Say the value 7 pops the value 3 off a decreasing stack. That means 7 is the first value to the right of 3 that is larger than it. You have just computed the next greater element for 3, for free, as a side effect of maintaining the order.

## Recognize it when

- The question is "for each element, find the nearest larger or smaller element to its left or right".
- The problem is about spans, temperatures until it gets warmer, or how far you can see.
- Histogram rectangles, water trapped between walls, or anything where a taller neighbor bounds a shorter one.
- The brute force is "for each element, scan right until I find something bigger".

**Words that give it away:** "next greater", "next smaller", "previous greater", "nearest", "warmer", "span", "largest rectangle", "how many days until".

## How it works

Walk the array once, keeping a stack of indices whose values are in monotonic order. Pop while the new value breaks the order, and each pop is an answer.

```
next greater element, decreasing stack of indices

[2, 1, 2, 4]
2   stack empty, push 2            stack: [2]
1   1 < 2, no pop, push 1          stack: [2, 1]
2   2 > 1, pop 1, answer(1) = 2    stack: [2]
    2 == 2, no pop, push 2         stack: [2, 2]
4   4 > 2, pop, answer(2) = 4      stack: [2]
    4 > 2, pop, answer(2) = 4      stack: []
    push 4                         stack: [4]
end anything left has no next greater element
```

Store indices rather than values. You almost always need to know where the popped element was, to compute a width or to write into an answer array.

## The code template

```python
def next_greater_elements(nums):
    """answer[i] is the first value to the right of i that is larger, or -1."""
    answer = [-1] * len(nums)
    stack = []                                   # indices, values decreasing
    for i, value in enumerate(nums):
        while stack and nums[stack[-1]] < value:
            answer[stack.pop()] = value          # value is the next greater
        stack.append(i)
    return answer


def largest_rectangle(heights):
    stack = []                                   # indices, heights increasing
    best = 0
    for i, height in enumerate(heights + [0]):   # the 0 flushes the stack at the end
        while stack and heights[stack[-1]] >= height:
            tall = heights[stack.pop()]
            left = stack[-1] + 1 if stack else 0
            best = max(best, tall * (i - left))
        stack.append(i)
    return best
```

The trailing `[0]` in the second function is a sentinel: a value smaller than everything, appended so the final loop empties the stack instead of leaving answers uncomputed.

## Complexity

| | |
|---|---|
| Time | O(n). Every index is pushed once and popped once, even with the inner while loop. |
| Space | O(n) |

## Variations

- Next greater, next smaller, previous greater, previous smaller. Four questions, one template, differing in scan direction and comparison.
- Strict versus non-strict comparison, which decides how equal values are treated.
- A sentinel at the end to flush the stack.
- Circular arrays, handled by running the loop twice over the indices modulo n.
- Trapping rain water, which the [two pointers](two-pointers.md) pattern also solves in O(1) space.

## Problems that use it

Next Greater Element, Next Smaller Element, Daily Temperatures, Stock Span, Largest Rectangle in Histogram, Sum of Subarray Minimums, Trapping Rain Water, Remove K Digits, Remove Duplicate Letters.

## Common mistakes

- Picking the wrong direction. Decide first whether you want the next larger or the next smaller, then choose the comparison that pops the other kind.
- Storing values instead of indices, and then finding you cannot compute the width.
- Forgetting the elements left on the stack at the end. They have no answer, and either a sentinel or a cleanup loop has to handle them.
- Getting equal values wrong. Using `<` versus `<=` changes which of two equal elements is treated as the boundary, which matters in histogram and subarray-minimum problems.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
