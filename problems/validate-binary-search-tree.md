# Validate Binary Search Tree

> Decide whether a binary tree obeys the binary search tree rule everywhere, not just locally.

**Pattern:** [Tree Depth First Search](../patterns/tree-depth-first-search.md) | **Difficulty:** Medium

## The problem

You are given the root of a binary tree. Return whether it is a valid binary search tree: every value
in a node's left subtree is smaller than the node, every value in its right subtree is larger, and
both subtrees are themselves valid.

## Why this is a Tree Depth First Search problem

The rule relates a node to **every** descendant, not to its immediate children, and that is the whole
difficulty. Checking `node.left.val < node.val < node.right.val` at each node passes this tree:

```
      5
     / \
    1   6
       / \
      3   7      3 is in the right subtree of 5, so it breaks the rule,
                 but it is a correct left child of 6
```

An answer that depends on ancestors, rather than on immediate children, needs a walk that carries
information down the tree. Depth first search does that, because the call stack is the path from the
root, and a parameter passed down travels with it.

## The approach

Pass an allowed range down, and narrow it at each step.

1. Call the check on the root with an open range, from negative infinity to positive infinity.
2. At a node:
   - A null node is valid, so return true.
   - If the node's value is outside the allowed range, return false.
   - Recurse left with the upper bound tightened to this node's value.
   - Recurse right with the lower bound tightened to this node's value.
   - Return whether both sides passed.

The invariant: the range handed to a node is exactly the set of values that node is allowed to hold,
given every ancestor above it.

There is a second solution worth naming. An **in-order** walk of a valid binary search tree produces
the values in sorted order, so you can walk in order and check that each value is greater than the
one before. It needs only a single previous value, not a range.

## Complexity

| | |
|---|---|
| Time | O(n), every node is visited once |
| Space | O(h) for the call stack, where h is the height. O(log n) for a balanced tree, O(n) for a tree shaped like a list. |

## Edge cases to say out loud

- Duplicate values. Ask whether they are allowed and on which side. The comparison is `<` or `<=`
  depending on the answer, and the interviewer usually has an opinion.
- A null root, and a single node.
- Values at the extremes of the integer range, which is the trap in the range solution if you seed the
  bounds with the minimum and maximum integer instead of null or infinity.
- A tree shaped like a list, where the recursion is n deep.

## Related problems

- Kth Smallest Element in a BST, which stops an in-order walk after k nodes.
- Lowest Common Ancestor of a BST, which uses the ordering to pick a direction instead of searching
  both sides.
- Binary Tree Path Sum, the other common shape, where state is carried down and undone on the way back.
- Tree Diameter, where the value returned upward and the answer being tracked are different things.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
