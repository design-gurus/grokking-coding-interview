# Balanced Parentheses

> Decide whether every bracket in a string is closed by the right kind, in the right order.

**Pattern:** [Stacks](../patterns/stacks.md) | **Difficulty:** Easy

## The problem

You are given a string of brackets, which may mix round, square, and curly. Return whether the string
is balanced: every opening bracket is closed by a matching one, and the pairs are properly nested, so
`([)]` is not valid even though the counts match.

Note that the course has a second problem with this name, in the
[Subsets](../patterns/subsets.md) chapter, which asks you to **generate** every valid string of n
pairs. That one is a backtracking problem. This one is a stack problem.

## Why this is a Stacks problem

Counting is the tempting first idea and it fails. Counting says `([)]` is fine, because there is one
of each. What makes a string valid is not how many brackets there are but **which one is still open**
when a closing bracket arrives.

"The most recent thing not yet dealt with" is the definition of the top of a stack. Every time you
meet an opening bracket, it becomes the thing that must be closed next. Every closing bracket must
match that one and nothing else.

The nesting in the problem statement is the cue. Nesting means last opened, first closed, and that is
last in, first out.

## The approach

1. Walk the string one character at a time.
2. On an opening bracket, push it.
3. On a closing bracket:
   - If the stack is empty, there is nothing to close, so return false.
   - Pop it. If the popped bracket is not the partner of this one, return false.
4. At the end, return whether the stack is empty.

The invariant: the stack holds every bracket that is open right now, innermost on top.

Both failure checks matter and they catch different inputs. The empty-stack check catches `)))`. The
final emptiness check catches `(((`.

## Complexity

| | |
|---|---|
| Time | O(n), each character is pushed and popped at most once |
| Space | O(n), for a string that is all opening brackets |

## Edge cases to say out loud

- The empty string, which is usually considered balanced. Ask.
- A string of only closing brackets, and a string of only opening ones. These are the two checks
  above, and skipping either one gives a wrong answer rather than a crash.
- Odd length, which can never be balanced. It falls out of the method, so no special case is needed.
- Other characters mixed in, as in `a(b)c`. Ask whether the input can contain them and what they mean.

## Related problems

- Simplify Path, where `..` pops the previous folder off the stack.
- Minimum Add to Make Parentheses Valid, which counts the leftovers instead of returning a yes or no.
- Remove All Adjacent Duplicates, the same push-and-cancel shape on plain characters.
- Balanced Parentheses in the [Subsets](../patterns/subsets.md) chapter, the generating version, which
  is [backtracking](../patterns/backtracking.md) with two counters.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
