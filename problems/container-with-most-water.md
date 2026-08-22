# Container With Most Water

> Pick two lines from an array of heights so the water held between them is as large as possible.

**Pattern:** [Two Pointers](../patterns/two-pointers.md) | **Difficulty:** Medium

## The problem

You are given an array where each entry is the height of a vertical line, and the lines stand one
unit apart. Choosing two lines forms a container. The water it holds is the distance between them
times the height of the **shorter** one, because water spills over the shorter side. Return the most
water any pair can hold.

## Why this is a Two Pointers problem

The answer is a pair, and the array has two meaningful ends. The brute force checks every pair, which
is two nested loops and O(n²).

The array is not sorted, and it does not need to be, which is the part that surprises people. What
makes the pointer move safe here is not sortedness but the shape of the area formula. Start at the
widest possible container, one pointer at each end. Any move inward loses width. The only way to gain
is a taller limiting line, and the limiting line is the shorter of the two. So moving the taller
pointer can never help: the width shrinks and the height is still capped by the same short line. Move
the shorter one.

That argument is the whole problem. If you can state it, the code is four lines.

## The approach

1. Put `left` at the first index and `right` at the last.
2. The area is `(right - left) * min(height[left], height[right])`. Record it if it is the best so far.
3. Move whichever pointer refers to the shorter line inward by one.
4. Stop when the pointers meet.

The invariant: every container you have skipped was no better than one you have already measured.

## Complexity

| | |
|---|---|
| Time | O(n), each index is visited once |
| Space | O(1) |

## Edge cases to say out loud

- Fewer than two lines, where no container exists.
- Lines of height zero, which hold nothing but still take up width.
- Equal heights at both pointers. Either may move. Say so, and say that it does not change the answer,
  because both containers that remain are covered by the moves that follow.
- All heights the same, where the answer is always the widest pair.

## Related problems

- [Trapping Rain Water](../patterns/monotonic-stack.md), which sounds similar and is not: there you
  sum the water over **every** position rather than choosing one pair, and the limiting height is the
  smaller of the tallest bar on each side.
- Pair with Target Sum, the sorted-array version of the converging pointer walk.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
