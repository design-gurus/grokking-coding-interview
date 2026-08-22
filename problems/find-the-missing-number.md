# Find the Missing Number

> An array holds n distinct numbers taken from 0 to n. Find the one that is not there.

**Pattern:** [Cyclic Sort](../patterns/cyclic-sort.md) | **Difficulty:** Easy

## The problem

You are given an array of n distinct numbers, all drawn from the range 0 to n. Since that range holds
n plus one numbers and the array holds n of them, exactly one is missing. Return it. You are asked for
O(n) time and constant extra space.

## Why this is a Cyclic Sort problem

The cue is a relationship stated in the problem: **the length of the array and the range of the values
are tied together**. The values are not arbitrary integers. Each one names an index.

That is what makes sorting unnecessary. In a normal array you learn where a value belongs by comparing
it against others. Here the value 5 belongs at index 5, and you know that without looking at anything
else. So you can place every number in one pass, and then the first index whose value is wrong is the
answer.

The constraint "constant extra space" rules out both a hash set and a boolean array of seen values,
which is the other reason the problem is written that way.

## The approach

1. Walk the array with an index `i`.
2. Look at `nums[i]`. If it is inside the range and not already at its home index, swap it there.
3. Do not advance `i` after a swap. The swap brought a new value to `i`, and it has not been checked.
4. Advance `i` only when the value at `i` is already home, or is out of range.
5. Walk the array once more. The first index whose value is not equal to the index is the missing
   number. If every index matches, the missing number is n.

The invariant: after the first pass, every value that has a home in the array is sitting in it.

## Complexity

| | |
|---|---|
| Time | O(n). The inner swapping looks quadratic, but every swap puts one number in its final home, so there are at most n swaps in total. |
| Space | O(1) |

Two other solutions are worth naming, because an interviewer may ask for them.

The sum method: the numbers 0 to n add up to `n * (n + 1) / 2`, so the missing one is that total minus
the actual sum. It is O(n) and O(1), and it is shorter, but the sum can overflow in a fixed-width
language.

The XOR method: XOR every index with every value. Every present number cancels its own index and the
missing one is left. Same cost, no overflow. See [Bitwise XOR](../patterns/bitwise-xor.md).

## Edge cases to say out loud

- The missing number is n, so the array is `[0, 1, ..., n-1]` and the first pass finds nothing wrong.
  Say how you handle it, because this is the case most first attempts get wrong.
- The missing number is 0.
- A single-element array.
- Advancing `i` after a swap, which leaves values misplaced. Say why you do not.

## Related problems

- Find all Missing Numbers, where more than one is absent, so you collect every mismatched index.
- Find the Duplicate Number, the same placement pass reading the other kind of mismatch.
- Find the Corrupt Pair, which returns the duplicate and the missing number together.
- Find the Smallest Missing Positive Number, where values outside the range are skipped rather than
  placed.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
