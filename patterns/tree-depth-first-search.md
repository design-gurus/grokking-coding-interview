# Pattern: Tree Depth First Search

> Go all the way down one branch before trying the next, so every question about a path has the whole path in hand when it reaches the bottom.

## What it is

Depth first search on a tree means: solve the left subtree, solve the right subtree, then combine the two answers at the current node. Recursion does the bookkeeping, and the call stack is the path from the root to wherever you are.

The three classic orders differ only in when the current node is handled. Pre-order handles it before the children, in-order between them, and post-order after both. In-order on a binary search tree visits the values in sorted order, which is why so many BST problems are one in-order walk.

## Recognize it when

- The answer depends on a path: root to leaf paths, path sums, the longest path, the diameter.
- The question relates an ancestor to its descendants, like the lowest common ancestor.
- The answer at a node is built from the answers of its two subtrees.
- The tree is a binary search tree and the question involves order, rank, or a range of values.

**Words that give it away:** "root to leaf", "path", "sum along", "depth", "height", "diameter", "ancestor", "subtree", "in-order".

## How it works

```
        1          the call stack while visiting 4:
      /   \        [1, 2, 4]
     2     3       which is exactly the path from the root
    / \
   4   5
```

There are two shapes, and knowing which one you need is most of the work.

**Returning a value up.** Each call returns something to its parent, which combines the two children's answers. Height, diameter, and "is this balanced" are all this shape.

**Carrying state down.** Each call passes the running path or sum to its children. Root-to-leaf path problems are this shape, and they need the undo step. After recursing, remove the current node from the path, so it does not appear in a sibling branch.

## The code template

```python
def has_path_with_sum(node, target):
    """Value returned upward."""
    if not node:
        return False
    if not node.left and not node.right:          # a leaf, not just a null child
        return node.val == target
    remaining = target - node.val
    return (has_path_with_sum(node.left, remaining)
            or has_path_with_sum(node.right, remaining))


def all_paths_with_sum(node, target, path, result):
    """State carried downward, with the undo step."""
    if not node:
        return
    path.append(node.val)
    if not node.left and not node.right and node.val == target:
        result.append(list(path))                 # copy, or every result aliases
    else:
        all_paths_with_sum(node.left, target - node.val, path, result)
        all_paths_with_sum(node.right, target - node.val, path, result)
    path.pop()                                    # undo before returning
```

## Complexity

| | |
|---|---|
| Time | O(n) to visit every node. Path enumeration costs O(n × path length) because each path is copied. |
| Space | O(h) for the call stack, where h is the height. That is O(log n) for a balanced tree and O(n) for a tree shaped like a list. |

## Variations

- Pre-order, in-order, post-order.
- Path sum: does one exist, list them all, count them including paths that do not start at the root.
- Diameter, the longest path between any two nodes, which usually passes through neither the root nor a leaf.
- Lowest common ancestor.
- Validate a binary search tree, by passing down the allowed range rather than checking only the immediate children.
- An iterative version with an explicit [stack](stacks.md), when recursion depth is a concern.
- [Serialize and deserialize](serialize-and-deserialize.md), which is a pre-order walk with markers for the null children.

## Problems that use it

Binary Tree Path Sum, All Paths for a Sum, Sum of Path Numbers, Path With Given Sequence, Count Paths for a Sum, Tree Diameter, Path with Maximum Sum, Lowest Common Ancestor, Validate Binary Search Tree, Kth Smallest Element in a BST.

## Common mistakes

- Missing the null base case, which crashes on the first leaf.
- Testing for a leaf with `if not node`, which is a null child, not a leaf. A leaf is a node with no children, and the difference changes the answer on a tree with a single child.
- Forgetting to undo the path after recursing, so nodes from one branch appear in another branch's results.
- Appending the path list itself instead of a copy, so every entry in the result points at the same list, which ends up empty.
- Validating a BST by comparing only against the immediate parent. A node deep in the left subtree still has to be smaller than the root.
- Confusing the value returned upward with a global best. Diameter is the classic case: the function returns the height, and the diameter is tracked separately.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
