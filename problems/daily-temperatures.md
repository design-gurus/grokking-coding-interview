# Daily Temperatures

> For each day, say how many days you must wait for a warmer one.

**Pattern:** [Monotonic Stack](../patterns/monotonic-stack.md) | **Difficulty:** Easy in the course, Medium elsewhere

## The problem

You are given an array of daily temperatures. For each day, return the number of days you have to wait
until a warmer temperature. If no warmer day ever comes, return 0 for that day.

## Why this is a Monotonic Stack problem

Rewrite the question and it names its own pattern: for each element, find the **next greater element**
to the right, and report the distance to it.

The brute force is a nested loop, scanning right from each day until something warmer turns up, which
is O(n²) and is exactly what a monotonic stack removes.

Here is the observation that makes it linear. Walk left to right, keeping a stack of days that are
still waiting for a warmer one. Those days are necessarily in decreasing temperature order: a warmer
day arriving would have already answered any cooler day beneath it. So when today is warmer than the
day on top of the stack, today is that day's answer, and you pop it. Today may answer several waiting
days at once.

Every day is pushed once and popped once, whatever the inner loop looks like.

## The approach

1. Create an answer array of zeros, so days that never warm up are already correct.
2. Keep a stack of **indices**, whose temperatures decrease from bottom to top.
3. For each day `i`:
   - While the stack is not empty and today's temperature is greater than the temperature at the top
     index, pop that index and set its answer to `i` minus that index.
   - Push `i`.
4. Whatever is left on the stack never found a warmer day, and its answers are already 0.

The invariant: the stack holds exactly the days still waiting, in decreasing temperature order.

Store indices, not temperatures. The answer is a distance, so you need to know where each waiting day
was.

## Complexity

| | |
|---|---|
| Time | O(n). Each index is pushed once and popped once, so the inner loop is O(n) in total, not per element. |
| Space | O(n) for the stack |

Say the amortized argument out loud. "There is a while loop inside a for loop, but each index can only
be popped once, so the total work is linear." That sentence is what the question is really testing.

## Edge cases to say out loud

- Equal temperatures. "Warmer" is strictly greater, so an equal day does not answer a waiting one. The
  comparison must be `>`, not `>=`.
- A strictly decreasing array, where every answer is 0 and nothing is ever popped.
- A strictly increasing array, where every day answers the one before it immediately.
- A single day, and an empty array.

## Related problems

- Next Greater Element, the same template returning the value instead of the distance.
- Stock Span, which looks left instead of right.
- Largest Rectangle in Histogram, the same stack with a width calculation and a sentinel at the end.
- Sum of Subarray Minimums, which uses previous-smaller and next-smaller together.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
