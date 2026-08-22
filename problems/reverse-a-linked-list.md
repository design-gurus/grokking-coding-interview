# Reverse a LinkedList

> Turn every pointer in a singly linked list around, using three variables and no extra memory.

**Pattern:** [In-place Reversal of a Linked List](../patterns/in-place-reversal-of-a-linked-list.md) | **Difficulty:** Easy

## The problem

You are given the head of a singly linked list. Reverse it, so the last node becomes the head and
every next pointer runs the other way. Return the new head. Do it in place, without building a second
list.

## Why this is an In-place Reversal problem

The words "reverse" and "in place" on a linked list are the whole cue. There is no cleverness to find
here, and that is worth knowing: the problem is testing whether you can rewire pointers without losing
the rest of the list.

The reason it is asked so often is that it appears inside larger problems. Palindrome checks, reorder,
reverse in groups of k, and rotate all reverse a section of a list as one step. Getting the three-line
loop automatic is what makes those solvable in an interview.

## The approach

Keep three references. `previous` is the part already reversed, `current` is the node being turned
around, and `following` holds the rest of the list while you overwrite the pointer that leads to it.

1. Set `previous` to null and `current` to the head.
2. While `current` is not null:
   - Save `current.next` into `following`.
   - Point `current.next` at `previous`.
   - Move `previous` to `current`.
   - Move `current` to `following`.
3. Return `previous`, which is the old tail.

The invariant: everything from `previous` backwards is reversed and correctly linked, and everything
from `current` forwards is untouched.

The order inside the loop is the whole problem. Save before you overwrite. Overwriting first loses
the rest of the list, and there is no way to get it back.

## Complexity

| | |
|---|---|
| Time | O(n) |
| Space | O(1) |

The recursive version is also O(n) time but O(n) space for the call stack, so on a long list it can
overflow. Mention it, then write the loop.

## Edge cases to say out loud

- An empty list, where the loop never runs and returning `previous` correctly returns null.
- A single node, which is its own reverse.
- The original head becoming the tail. Its next pointer must end up null, and it does, because
  `previous` starts as null.
- Returning `current` at the end instead of `previous`. `current` is null by then, which returns an
  empty list.

## Related problems

- Reverse a Sub-list, the same loop run between two positions and then stitched back in. Use a dummy
  node in front of the head so a sub-list that starts at position 1 needs no special case.
- Reverse Every K-element Sub-list, which repeats that stitching.
- Rotate a Linked List, which is a re-link rather than a reversal.
- Palindrome LinkedList, which finds the middle with
  [fast and slow pointers](../patterns/fast-and-slow-pointers.md), reverses the second half, compares,
  and then puts the list back.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
