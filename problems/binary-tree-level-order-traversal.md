# Binary Tree Level Order Traversal

> Return the nodes of a binary tree grouped by depth, one list per level.

**Pattern:** [Tree Level Order Traversal](../patterns/tree-level-order-traversal.md) | **Difficulty:** Easy

## The problem

You are given the root of a binary tree. Return a list of lists, where the first list holds the root,
the second holds its children from left to right, the third holds their children, and so on.

## Why this is a Level Order Traversal problem

The output shape is the cue, and it is unmissable: **the answer is grouped by level**. A depth first
walk visits nodes in an order that mixes levels together, so grouping afterwards would mean carrying
a depth with every node and sorting at the end. A queue produces the grouping as it goes, at no extra cost.

Any question whose answer is one value per level, or one list per level, or "the first node at the
shallowest depth", lands here.

## The approach

The one idea that matters: **record the size of the queue before you process the level**.

1. If the root is null, return an empty list.
2. Put the root in a queue.
3. While the queue is not empty:
   - Read its current size, and call that the level size.
   - Loop exactly that many times: pop a node, add its value to the current level list, and push its
     non-null children.
   - Add the finished level list to the result.

The invariant: at the top of each round, the queue holds exactly the nodes of one level, in left to
right order.

The inner loop pushes children onto the same queue while it runs, so reading the size inside the loop
would mix two levels together. That single line is what the problem is testing.

## Complexity

| | |
|---|---|
| Time | O(n), every node is enqueued and dequeued once |
| Space | O(w), where w is the widest level. For a complete tree that is about n / 2. |

## Edge cases to say out loud

- A null root.
- A tree shaped like a list, where every level has one node.
- Pushing children without a null check, which corrupts every level size after it.
- Using a plain array as a queue. Removing from the front of an array is O(n), which quietly makes the
  traversal quadratic. Name the right structure for your language: `deque` in Python, `ArrayDeque` in
  Java.

## Related problems

- Reverse Level Order Traversal, the same walk with the result list reversed at the end.
- Zigzag Traversal, which alternates the direction each level is collected in.
- Level Averages, one number per level instead of a list.
- Minimum Depth of a Binary Tree, which returns at the first leaf it pops, because depth only ever
  increases.
- Right View of a Binary Tree, the last node of each level.
- Connect Level Order Siblings, which links each node to the next one in its level.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
