# Subsets

> Generate every subset of a set of distinct numbers.

**Pattern:** [Subsets](../patterns/subsets.md) | **Difficulty:** Easy

## The problem

You are given a list of distinct integers. Return every possible subset, including the empty subset
and the full list. The order of the subsets does not matter, and neither does the order inside each
one.

## Why this is a Subsets problem

"Return every possible X" over a small input means enumeration, and enumeration means one of two
shapes: the iterative build in this pattern, or [backtracking](../patterns/backtracking.md). They
explore the same tree.

The number of subsets is 2 to the power n, because each element is independently in or out. That
number is also the constraint check. If n could be 40, no enumeration finishes, and the problem would
have to be counting or optimizing rather than listing, which usually means
[dynamic programming](../patterns/0-1-knapsack.md). Seeing n at 10 or 20 in the constraints is the
confirmation that listing is intended.

## The approach

Start with a list holding just the empty subset. For each input element, copy every subset built so
far and add the element to the copies.

1. Result starts as `[[]]`.
2. For each number:
   - For every subset currently in the result, make a copy with the number appended.
   - Add all those copies to the result.
3. Return the result.

The invariant: after processing the first k elements, the result holds exactly the 2 to the power k
subsets of those elements.

```
start:   [[]]
add 1:   [[], [1]]
add 2:   [[], [1], [2], [1,2]]
add 3:   [[], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]]
```

The list doubles at every step, which is where the cost comes from.

## Complexity

| | |
|---|---|
| Time | O(n × 2^n). There are 2^n subsets and copying each one costs up to n. |
| Space | O(n × 2^n), which is the output itself |

You cannot beat that, because the output is that large. Say so, so the interviewer knows you are not
confusing the cost of the algorithm with the cost of the answer.

## Edge cases to say out loud

- The empty input, whose only subset is the empty subset, so the answer is `[[]]` and not `[]`.
- Appending the working list itself rather than a copy of it. Every entry would then point at the same
  list, and the result would be n copies of the final state.
- **Duplicates in the input.** This problem says the numbers are distinct. If they are not, sort the
  input first and, when the current element equals the previous one, extend only the subsets created in
  the previous round. Extending all of them produces the same subset twice.

## Related problems

- Subsets With Duplicates, the version that needs the sort-and-skip rule above.
- Permutations, where order matters, so the count is n factorial rather than 2 to the power n.
- Combination Sum and Word Search, which are the same tree with a rule that lets you prune branches
  early, which is what makes them [backtracking](../patterns/backtracking.md) problems.
- [0/1 Knapsack](../patterns/0-1-knapsack.md), which is the same in-or-out decision when the input is
  too large to list and you only need the best answer.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
