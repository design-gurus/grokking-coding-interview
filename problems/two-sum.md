# Two Sum

> Find the two numbers in an unsorted array that add up to a target, and return their positions.

**Pattern:** [Hash Maps](../patterns/hash-maps.md) | **Difficulty:** Easy

## The problem

You are given an array of integers and a target number. Exactly one pair of positions holds two
numbers that add up to the target. Return those two positions. The array is not sorted, the same
position may not be used twice, and the numbers may be negative.

## Why this is a Hash Maps problem

Three things in the statement point the same way.

The array is **not sorted**, which rules out the converging pointers of the
[Two Pointers](../patterns/two-pointers.md) pattern. You could sort it first, but the answer is
positions, and sorting destroys them.

The question asks whether a specific value **has already been seen**. For the number 7 and a target
of 9, you want to know if 2 appeared earlier. That is a membership question over the part of the
array you have already walked, which is what a hash map answers in constant time.

And the brute force settles it: two nested loops where the inner one only ever looks backwards.
Whenever the inner loop is a backwards search, a map of what you have already passed replaces it.

## The approach

Walk the array once. At each number, the number you need is `target - current`. Ask the map whether
you have already stored it. If yes, you have both positions. If no, store the current number with
its position and move on.

1. Create an empty map from value to position.
2. For each position `i` with value `v`:
   - If `target - v` is a key in the map, return that stored position and `i`.
   - Otherwise put `v` to `i` into the map.

The invariant: the map holds exactly the numbers before `i`, keyed by value. That is why looking up
before inserting matters. Insert first and a number whose double equals the target would match
itself.

## Complexity

| | |
|---|---|
| Time | O(n), one pass, constant work per element on average |
| Space | O(n) for the map |

The often-quoted alternative is to sort and use two pointers, which is O(n log n) time and O(1)
extra space if you may sort in place. Say both out loud, then pick the map, because the question
wants positions.

## Edge cases to say out loud

- Negative numbers and zero. Nothing in the method cares, but say it, because it rules out any idea
  that depends on values being positive.
- The same value twice, as in `[3, 3]` with a target of 6. This works, because the second 3 finds the
  first one in the map, and it is exactly the case that breaks an insert-first version.
- No pair exists. The problem promises one, so ask what to return if the promise is dropped.
- Very large values. In a fixed-width language, `target - v` can overflow.

## Related problems

- [Pair with Target Sum](../patterns/two-pointers.md), the same question on a **sorted** array, where
  two pointers do it in O(1) space.
- Three Sum, which fixes one number and runs the pair search on the rest.
- [Subarray Sum Equals K](subarray-sum-equals-k.md), which is the same complement trick applied to
  running totals rather than to values.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
