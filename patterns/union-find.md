# Pattern: Union Find

> Keep track of which things are in the same group, and answer "are these two connected" in almost constant time, even as groups keep merging.

## What it is

Union Find, also called a disjoint set union, maintains a collection of groups. It supports two operations: `find`, which returns the representative of an element's group, and `union`, which merges two groups.

Each element points at a parent. Following those pointers upward reaches a root, and two elements are in the same group when they reach the same root. Two optimizations keep the trees flat. Path compression repoints every node visited during a `find` straight at the root. Union by size always puts the smaller tree under the larger one.

## Recognize it when

- The question is about connectivity or grouping, and connections are added over time.
- The problem asks how many groups there are, or whether two items are already related.
- Adding an edge that connects two already-connected nodes would create a cycle, and detecting that is the task.
- A [graph traversal](graphs.md) would work but would have to be re-run from scratch after every new edge.

**Words that give it away:** "connected", "provinces", "friend circles", "redundant connection", "merge accounts", "same group", "number of components".

## How it works

```
start:  every element is its own group
        0   1   2   3

union(0,1)     0       union(2,3)     2
               |                      |
               1                      3

union(1,3)     0
              / \
             1   2
                 |
                 3

find(3) walks 3 -> 2 -> 0, and path compression then points 3 straight at 0
```

The two optimizations matter. Without them, a chain of unions can build a tree shaped like a linked list, and every `find` walks the whole thing.

## The code template

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))     # every element starts alone
        self.size = [1] * n
        self.groups = n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]   # path compression
            x = self.parent[x]
        return x

    def union(self, a, b):
        root_a, root_b = self.find(a), self.find(b)
        if root_a == root_b:
            return False                 # already together, so this edge is redundant
        if self.size[root_a] < self.size[root_b]:
            root_a, root_b = root_b, root_a
        self.parent[root_b] = root_a     # smaller tree hangs under the larger
        self.size[root_a] += self.size[root_b]
        self.groups -= 1
        return True
```

The boolean returned by `union` is the whole answer to several problems. `False` means the two nodes were already connected, so the edge closes a cycle.

## Complexity

| | |
|---|---|
| Time | Effectively O(1) per operation, amortized. The exact bound is the inverse Ackermann function, which is below 5 for any input that fits in a computer. |
| Space | O(n) |

## Variations

- Count the connected components.
- Detect a cycle in an undirected graph, using the return value of `union`.
- Kruskal's minimum spanning tree, which sorts edges and unions the ones that do not close a cycle.
- Merging accounts or emails, where the elements are strings and the parent map is a dictionary.
- Grouping grid cells, as an alternative to the [island](island-matrix-traversal.md) pattern, and the better choice when land is added over time.
- Weighted union find, which stores a relationship along each parent link rather than plain equivalence.

## Problems that use it

Redundant Connection, Number of Provinces, Number of Islands II, Accounts Merge, Most Stones Removed with Same Row or Column, Is Graph Bipartite, Satisfiability of Equality Equations, Longest Consecutive Sequence.

## Common mistakes

- Skipping path compression and union by size. Without them the structure degrades to O(n) per operation, and a large test times out.
- Linking the elements instead of their roots. `parent[a] = b` is wrong. It has to be `parent[find(a)] = find(b)`.
- Using it on a directed graph. Union Find has no notion of direction, so it cannot answer reachability in a directed graph or replace [topological sort](topological-sort.md).
- Counting the groups by scanning the parent array for `parent[i] == i` without calling `find` first, which miscounts on a partly compressed structure. Keeping a running counter in `union` is safer.
- Forgetting to map non-integer elements to indices, or to a dictionary, before using the structure.

## Go deeper

- This pattern's introduction in the course: [Introduction to Union Find Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-union-find-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
