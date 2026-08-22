# Pattern: Clone

> Copy a structure node by node, keeping a map from each original to its copy, so shared links and cycles are reproduced instead of chased forever.

## What it is

A deep copy means the new structure shares no nodes with the old one, so changing the copy leaves the original untouched.

The whole pattern is one map: original node to cloned node. Before cloning a node, check the map. If the node is already there, return the existing clone instead of making another one. That single check does two jobs at once. It stops the recursion from looping forever on a cycle, and it guarantees that two links pointing at the same original end up pointing at the same copy.

## Recognize it when

- The problem says deep copy, clone, or "the copy must be independent of the original".
- The structure has links beyond a simple tree: a graph, a random pointer, a parent pointer, cross-links.
- The structure may contain a cycle, so a naive recursion would not terminate.
- Two links can reach the same node, and the copy has to preserve that sharing.

**Words that give it away:** "deep copy", "clone", "random pointer", "independent copy", "modifications must not affect".

## How it works

```
original:   A -> B
            ^    |
            +----+

clone(A):  A not in map, make A'
           map: {A: A'}
           clone(B): B not in map, make B'
                     map: {A: A', B: B'}
                     clone(A): already in the map, return A'
```

Without the map, `clone(A)` would call `clone(B)` which would call `clone(A)` forever.

The linked list with a random pointer has a second solution worth knowing, because it uses no extra memory. Weave each copy in just after its original, so `node.next` is always that node's clone. Set the random pointers using that relationship, then unweave the two lists.

## The code template

```python
def clone_graph(node):
    clones = {}                                    # original -> clone

    def build(current):
        if not current:
            return None
        if current in clones:
            return clones[current]                 # the check that ends the recursion
        copy = Node(current.val)
        clones[current] = copy                     # record BEFORE recursing
        for neighbor in current.neighbors:
            copy.neighbors.append(build(neighbor))
        return copy

    return build(node)


def copy_random_list(head):
    """Two passes with a map. The weaving trick is the O(1) space alternative."""
    if not head:
        return None
    clones = {}
    current = head
    while current:                                 # first pass: make every node
        clones[current] = Node(current.val)
        current = current.next
    current = head
    while current:                                 # second pass: wire the links
        clones[current].next = clones.get(current.next)
        clones[current].random = clones.get(current.random)
        current = current.next
    return clones[head]
```

Recording the clone in the map **before** recursing is the critical line. Doing it after means a cycle reaches the node again while it is still unrecorded, and the recursion never ends.

## Complexity

| | |
|---|---|
| Time | O(n + e), where n is the number of nodes and e the number of links |
| Space | O(n) for the map |

## Variations

- Clone a graph, with depth first or breadth first traversal.
- Copy a linked list with random pointers, either with a map or with the weaving trick.
- Clone a tree with parent pointers, or with arbitrary cross-links.
- Clone a tree where a node can have any number of children.
- Two-pass cloning, making every node first and wiring the links second, which avoids recursion entirely.

## Problems that use it

Clone Graph, Copy List with Random Pointer, Clone N-ary Tree, Clone Binary Tree With Random Pointer, Clone a Directed Acyclic Graph.

## Common mistakes

- Recording the clone in the map after recursing rather than before, which loops forever on a cycle.
- Copying only the values and not every link, so the copy points back into the original structure.
- Missing a self-loop, where a node lists itself as its own neighbor.
- Using the node's value as the map key when values can repeat. The key must be the node itself, or its identity.
- Making a shallow copy of a list of neighbors, which copies the list but not what it points at.

## Go deeper

- This pattern's introduction in the course: [Introduction to Clone Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-clone-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
