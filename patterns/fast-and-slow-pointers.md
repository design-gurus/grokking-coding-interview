# Pattern: Fast and Slow Pointers

> Two pointers move through the same structure at different speeds, and the gap between them reveals cycles, midpoints, and repetition without any extra memory.

## What it is

One pointer takes one step at a time. The other takes two. That difference in speed is the whole idea. If the structure ends, the fast pointer reaches the end first, and the slow pointer is sitting at the halfway mark. If the structure loops, the fast pointer eventually laps the slow one and they land on the same node.

This is the memory-free alternative to storing every node you have seen in a hash set. It is also known as Floyd's cycle detection, or the tortoise and hare.

## Recognize it when

- The input is a linked list and the question is about a cycle, the middle, or the kth node from the end.
- The problem says "do not use extra memory" or "O(1) space" on a linked list.
- A sequence repeats itself and you need to find where the repetition starts. Happy Number is a sequence of numbers rather than a list, but it has the same shape.
- Your first instinct is a hash set of visited nodes.

**Words that give it away:** "cycle", "loop", "middle node", "kth from the end", "palindrome linked list", "constant space".

## How it works

Move slow one step and fast two steps per round.

```
no cycle:   1 -> 2 -> 3 -> 4 -> 5 -> null
            fast runs off the end, slow sits at 3, the middle

cycle:      1 -> 2 -> 3 -> 4
                      ^      |
                      +-- 5 <+
            fast laps slow inside the loop, so slow meets fast
```

To find where the cycle begins, reset one pointer to the head after the meeting and move both one step at a time. They meet at the entry to the cycle. The reason is arithmetic. The distance from the head to the cycle entry equals the distance from the meeting point to the cycle entry, going forward around the loop.

## The code template

```python
def find_cycle_start(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:                  # a cycle exists
            pointer = head
            while pointer is not slow:    # both move one step now
                pointer = pointer.next
                slow = slow.next
            return pointer                # the first node of the cycle
    return None                           # fast ran off the end, no cycle


def middle_node(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow          # for an even length this is the second middle
```

## Complexity

| | |
|---|---|
| Time | O(n) |
| Space | O(1) |

## Variations

- Cycle detection, yes or no.
- Finding the first node of the cycle.
- Finding the length of the cycle, by moving one pointer around the loop from the meeting point until it returns.
- Finding the middle, which is how you split a list for merge sort or a palindrome check.
- Palindrome linked list: find the middle, reverse the second half in place, compare, then put it back.
- Happy Number, where "the next node" means the sum of the squares of the digits.

## Problems that use it

Linked List Cycle, Start of Linked List Cycle, Middle of the Linked List, Palindrome Linked List, Rearrange a Linked List, Happy Number, Cycle in a Circular Array.

## Common mistakes

- Not checking both `fast` and `fast.next` before taking two steps, which crashes on a null pointer.
- Getting the wrong middle. With an even number of nodes there are two middles, and the loop condition decides which one you get. Read the problem and pick deliberately.
- Comparing values instead of node identity. Two different nodes can hold the same value.
- Forgetting that after reversing the second half for a palindrome check, some interviewers want the list restored.

## Go deeper

- This pattern's introduction in the course: [Introduction to Fast & Slow Pointers Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-fast-slow-pointers-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
