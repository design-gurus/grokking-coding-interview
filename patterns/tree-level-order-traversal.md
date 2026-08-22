# Pattern: Tree Level Order Traversal

> A queue visits a tree one level at a time, so the first node that matches is always the one closest to the root.

## What it is

Level order traversal, also called breadth first search on a tree, uses a queue. You push the root, then repeatedly pop a node and push its children. Nodes come out in order of depth: everything at depth 1, then everything at depth 2, and so on.

Recording the size of the queue before each round is what separates one level from the next. That single line is the difference between "visit every node" and "visit every level".

## Recognize it when

- The answer is structured per level: a list per level, the average of each level, the largest value in each row, zigzag order.
- The question says shortest, minimum depth, nearest, or first. Depth only ever increases, so the first match is the closest one.
- The question asks for a view of the tree from one side, or asks you to connect nodes that sit next to each other.
- The tree is wide and the answer does not depend on any root-to-leaf path.

**Words that give it away:** "level", "row", "zigzag", "minimum depth", "shortest", "right side view", "next pointer", "width of the tree".

## How it works

```
        1            queue: [1]           size 1, pop 1, push 2 and 3
      /   \
     2     3         queue: [2, 3]        size 2, pop both, push 4, 5, 6
    / \     \
   4   5     6       queue: [4, 5, 6]     size 3, pop all three
```

Capture the queue size before the inner loop. The inner loop adds children to the same queue while it runs, so reading the size inside the loop would mix two levels together.

## The code template

```python
from collections import deque

def level_order(root):
    if not root:
        return []
    result, queue = [], deque([root])
    while queue:
        level_size = len(queue)             # capture before the inner loop
        level = []
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        result.append(level)
    return result


def minimum_depth(root):
    if not root:
        return 0
    queue, depth = deque([root]), 1
    while queue:
        for _ in range(len(queue)):
            node = queue.popleft()
            if not node.left and not node.right:
                return depth                # the first leaf is the shallowest
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        depth += 1
```

## Complexity

| | |
|---|---|
| Time | O(n), where n is the number of nodes |
| Space | O(w), where w is the widest level. For a complete tree that is about n / 2. |

## Variations

- Standard level order, and reverse level order by reversing the finished list.
- Zigzag, by alternating the direction in which each level is collected.
- One statistic per level: sum, average, maximum.
- Early exit at the first match, for minimum depth or the nearest node meeting a condition.
- The level order successor, which is the next node popped after the target.
- Connecting siblings with next pointers.
- Carrying a position index per node to measure the width of a level.
- Trees where a node has more than two children.

## Problems that use it

Binary Tree Level Order Traversal, Reverse Level Order Traversal, Zigzag Traversal, Level Averages in a Binary Tree, Minimum Depth of a Binary Tree, Level Order Successor, Connect Level Order Siblings, Right View of a Binary Tree, Maximum Width of Binary Tree, N-ary Tree Level Order Traversal.

## Common mistakes

- Not capturing the level size before the inner loop, which mixes nodes from two levels into one list.
- Treating a node with one child as a leaf. For minimum depth that gives an answer that is too small, because a node with only a left child is not a leaf.
- Pushing children without checking for null, which corrupts every level size after it.
- Using a plain list as a queue. Removing from the front of a list is O(n), which quietly makes the traversal quadratic. Use `deque` in Python, `LinkedList` or `ArrayDeque` in Java.
- Forgetting the null root check at the top.
- Measuring width by counting nodes rather than by the span between positions, which ignores the gaps left by missing nodes.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
