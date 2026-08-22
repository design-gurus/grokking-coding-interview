# LinkedList Cycle

> Decide whether a linked list loops back on itself, without using extra memory.

**Pattern:** [Fast and Slow Pointers](../patterns/fast-and-slow-pointers.md) | **Difficulty:** Easy

## The problem

You are given the head of a singly linked list. Somewhere in it, a node's next pointer may lead back
to an earlier node, which makes a loop that never ends. Return whether such a loop exists. You are
asked to do it in constant extra space.

## Why this is a Fast and Slow Pointers problem

The obvious solution is a set of every node you have visited, and you return true the moment you meet
one twice. It works, and it costs O(n) memory. The constraint "constant extra space" is there to take
that solution away, and that constraint is the cue.

Once a set is not allowed, the only thing left to compare is the list against itself. Two pointers at
different speeds do that. On a list that ends, the fast pointer runs off the end. On a list that
loops, the fast pointer cannot escape, and because it gains one position per round on the slow one,
it eventually lands on it. There is no third outcome.

## The approach

1. Start `slow` and `fast` at the head.
2. Each round, move `slow` one node and `fast` two nodes.
3. If `fast` or `fast.next` is null, the list ends, so return false.
4. If `slow` and `fast` are the same node, return true.

The invariant: once both pointers are inside the loop, the gap between them shrinks by exactly one
node per round, so they cannot pass each other without meeting.

Say that last sentence in the interview. "They might jump over each other" is the first objection an
interviewer raises, and the gap-shrinks-by-one argument answers it.

## Complexity

| | |
|---|---|
| Time | O(n) |
| Space | O(1) |

## Edge cases to say out loud

- An empty list, and a list with one node and no loop.
- A single node pointing at itself.
- A loop that includes the head, so the whole list is the loop.
- The null check. You must test `fast` **and** `fast.next` before taking two steps, or the last step
  crashes.

## Related problems

- Start of LinkedList Cycle, which continues from the meeting point: reset one pointer to the head,
  move both one step at a time, and they meet at the first node of the loop.
- Middle of the LinkedList, the same two speeds used on a list that ends.
- Happy Number, which is this problem with different wording. The "next node" of a number is the
  sum of the squares of its digits, and the question is whether that sequence loops.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
