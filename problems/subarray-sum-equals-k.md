# Subarray Sum Equals K

> Count the contiguous subarrays whose values add up to k, where the values may be negative.

**Pattern:** [Prefix Sum](../patterns/prefix-sum.md) | **Difficulty:** Medium

## The problem

You are given an array of integers and a target k. Return how many contiguous subarrays add up to
exactly k. The array may contain negative numbers and zeros.

## Why this is a Prefix Sum problem

Two details decide it, and both are easy to walk past.

The answer is a **count**, not one best subarray. And the values may be **negative**.

Those two facts together rule out a [sliding window](../patterns/sliding-window.md), which is the first
instinct for anything about contiguous subarrays. A window works by growing while the sum is too small
and shrinking while it is too large. That reasoning needs the sum to rise when you add an element and
fall when you remove one. With negative numbers neither is true, so the window has no direction to
move in.

What does survive is arithmetic. If the running total at position j is S, and some earlier position i
had a running total of S minus k, then everything between them sums to exactly k. So the question
becomes: how many earlier positions had a running total of S minus k? That is a counting lookup, which
is a [hash map](../patterns/hash-maps.md) over prefix sums.

## The approach

1. Keep a running total and a map from running total to how many times it has occurred.
2. Seed the map with a total of 0 occurring once, standing for the empty prefix before the array
   starts.
3. For each number:
   - Add it to the running total.
   - Add `map[running - k]` to the answer. Every earlier position with that total starts a qualifying
     subarray ending here.
   - Increment `map[running]`.

The invariant: the map holds the running totals of every prefix strictly before the current position,
with their counts.

Add to the answer **before** recording the current total. Doing it the other way lets a subarray of
length zero count itself when k is 0.

## Complexity

| | |
|---|---|
| Time | O(n) |
| Space | O(n) for the map |

## Edge cases to say out loud

- **The zero seed.** Without `map[0] = 1`, every subarray that starts at index 0 is missed. This is the
  single most common bug in the problem, and naming it before you code is worth doing.
- k equal to 0, and runs of zeros in the array, which produce many overlapping answers.
- All negative numbers.
- A subarray that is the entire array.
- Overflow, since running totals grow with the whole array.

## Related problems

- [Two Sum](two-sum.md), the same complement lookup applied to values rather than to running totals.
- Subarray Sums Divisible by K, where the map key is the remainder instead of the total. Watch the
  sign of the modulo operator in Java and C++.
- Maximum Size Subarray Sum Equals k, which stores the **first** index for each total instead of a
  count, because it wants the longest subarray rather than how many there are.
- Smallest Subarray with a Given Sum, which **is** a sliding window, because there the values are
  promised to be positive. That contrast is worth being able to state.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
