# Smallest Window containing Substring

> Find the shortest stretch of a string that contains every character of a pattern, counting repeats.

**Pattern:** [Sliding Window](../patterns/sliding-window.md) | **Difficulty:** Hard

## The problem

You are given a string and a pattern. Return the shortest substring of the string that contains every
character of the pattern, including repeated characters the right number of times. The characters may
appear in any order, and the substring may contain extra characters. If no such substring exists,
return the empty string.

This problem is also known as Minimum Window Substring.

## Why this is a Sliding Window problem

The answer is **contiguous**, which is the first thing to notice. A substring is a window by
definition, and the moment the answer must be one unbroken stretch, the whole family of scattered
selection techniques is out.

The second cue is the word **shortest**. A shortest-window question has a natural shape: grow the
window on the right until it becomes valid, then shrink it from the left for as long as it stays
valid, recording the best length each time. Grow to become correct, shrink to become small.

The third is that validity is a property you can update in O(1). Adding one character to the window
changes one count. That is what makes the window cheap and the brute force, which rechecks every
substring, unnecessary.

## The approach

1. Count the characters of the pattern in a map.
2. Track `matched`, the number of pattern characters fully satisfied by the window.
3. Move `end` forward. If the entering character is in the pattern, decrement its remaining count, and
   when that count reaches zero, increment `matched`.
4. While `matched` equals the number of distinct pattern characters, the window is valid:
   - Record it if it is shorter than the best so far.
   - Remove the character at `start` from the window, undoing step 3, and move `start` forward.

The invariant: `matched` is exactly the number of distinct pattern characters whose required count is
met inside the window right now.

The shrink must be a `while`, not an `if`. A window can lose several leading characters and stay
valid, and the shortest answer is found only after all of them are dropped.

## Complexity

| | |
|---|---|
| Time | O(n + m), where n is the string and m is the pattern. Each index enters and leaves the window once. |
| Space | O(m) for the counts |

## Edge cases to say out loud

- The pattern is longer than the string, so no window exists.
- Repeated characters in the pattern, as in pattern `aab`. This is the case that breaks a solution
  built on a set instead of counts.
- Characters in the string that are not in the pattern at all. They are allowed inside the window and
  must not affect `matched`.
- The whole string is the answer, and the empty result when nothing matches.

## Related problems

- Longest Substring with K Distinct Characters, the same window with the opposite goal, where you
  shrink because the window has become **invalid** rather than while it is still valid.
- Permutation in a String, a fixed-size window with the same counting.
- String Anagrams, which returns every valid window instead of the shortest one.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
