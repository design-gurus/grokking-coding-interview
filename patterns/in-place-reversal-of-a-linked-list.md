# Pattern: In-place Reversal of a Linked List

> Rewire the next pointers as you walk, and a linked list reverses itself using three variables and no extra memory.

## What it is

Reversing a linked list by building a new one is easy and costs O(n) memory. Reversing it in place costs nothing extra: you walk the list once and turn each pointer around as you pass it.

The whole pattern is three variables. `previous` is the part already reversed. `current` is the node being turned around. `next` holds the rest of the list, saved before you overwrite the pointer that leads to it.

## Recognize it when

- The input is a linked list and the words "reverse", "rotate", "reorder", or "swap nodes" appear.
- The problem says in place, or O(1) space, on a linked list.
- Only part of the list is reversed: between two positions, or every K nodes, or alternating groups.
- The answer needs the list read backwards, and you are not allowed to store the values in an array.

**Words that give it away:** "reverse the linked list", "reverse a sub-list", "every k nodes", "in place", "without extra space", "rotate the list".

## How it works

Turn one pointer around per step, and carry the rest of the list forward in a temporary variable.

```
before:   null   1 -> 2 -> 3 -> 4 -> null
                 ^
              current

step:     save next = 2, point 1 at previous (null), move both forward

after:    null <- 1   2 -> 3 -> 4 -> null
                  ^   ^
             previous current
```

For a sub-list, do the same thing between the two boundary nodes, then stitch the reversed piece back in. Keep a pointer to the node before the sub-list, and to the first node of the sub-list. After the reversal, that first node has become the last one.

## The code template

```python
def reverse(head):
    previous, current = None, head
    while current:
        following = current.next     # save the rest before overwriting
        current.next = previous      # turn this pointer around
        previous = current           # the reversed part grows
        current = following          # move on
    return previous                  # the old tail is the new head


def reverse_sub_list(head, p, q):
    """Reverse the nodes between positions p and q, counting from 1."""
    if p == q:
        return head
    dummy = ListNode(0, head)        # a dummy head removes the "p is 1" special case
    before = dummy
    for _ in range(p - 1):
        before = before.next

    previous, current = None, before.next
    tail_of_reversed = current       # this node ends up last
    for _ in range(q - p + 1):
        following = current.next
        current.next = previous
        previous = current
        current = following

    before.next = previous           # stitch the front back on
    tail_of_reversed.next = current  # stitch the back back on
    return dummy.next
```

## Complexity

| | |
|---|---|
| Time | O(n) |
| Space | O(1) |

## Variations

- Reverse the whole list.
- Reverse a sub-list between two positions.
- Reverse every group of K nodes.
- Reverse alternating groups of K nodes.
- Rotate the list by K places, which is a re-link rather than a reversal.
- Reorder a list by finding the middle with [fast and slow pointers](fast-and-slow-pointers.md), reversing the second half, then weaving the two halves together.

## Problems that use it

Reverse a Linked List, Reverse a Sub-list, Reverse Every K-element Sub-list, Reverse Alternating K-element Sub-list, Rotate a Linked List, Reorder List, Swap Nodes in Pairs.

## Common mistakes

- Overwriting `current.next` before saving it, which loses the rest of the list.
- Forgetting that the original head becomes the tail, and leaving its next pointer set, which creates a cycle.
- Not using a dummy head node, which forces a special case every time the reversal starts at the first node.
- Losing track of the boundary nodes in a sub-list reversal. Draw the four pointers involved before writing any code. It saves more time than it costs.
- On "every K nodes", not checking that a full group of K remains. Most versions of the problem leave a shorter final group as it is.

## Go deeper

- This pattern's introduction in the course: [Introduction to In-place Reversal of a Linked List Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-inplace-reversal-of-a-linked-list-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
