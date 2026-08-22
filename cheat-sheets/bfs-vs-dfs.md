# BFS vs DFS: Which Search, and Why

Both visit every node once and both cost O(V + E). Choosing between them is not about speed. It is
about what the question asks for, and picking the wrong one usually still runs, still terminates, and
still gives the wrong answer.

## The one-line rule

**Fewest steps means breadth first. Everything else usually means depth first.**

Breadth first search explores in rings of increasing distance, so the first time it reaches a node,
it reached it by the shortest route. Depth first search follows one route to its end before backing
up, so it has the whole path in hand at every point.

## Side by side

| | Breadth first | Depth first |
|---|---|---|
| Structure | Queue | Stack, or recursion |
| Explores | Level by level, outward | One branch to the end, then backtrack |
| Finds the shortest path | Yes, on unweighted graphs | No |
| Knows the current path | No, only distances | Yes, the call stack is the path |
| Space | O(w), the widest level | O(h), the deepest path |
| Worst case space | O(V) on a wide graph | O(V) on a long thin graph |
| Stack overflow risk | None, the queue is on the heap | Real, on deep recursion |
| Natural for | Distance, levels, nearest match | Paths, cycles, ordering, connectivity |

## Pick by what the question asks

| The question asks for | Use | Why |
|---|---|---|
| Fewest steps, minimum moves, shortest path on equal edges | BFS | The first arrival is the shortest |
| Minimum depth of a tree | BFS | Return at the first leaf popped |
| The answer grouped per level | BFS | See [level order traversal](../patterns/tree-level-order-traversal.md) |
| A view of a tree from one side | BFS | First or last node of each level |
| Something spreading over time | BFS, multi-source | Each round is one unit of time |
| All root-to-leaf paths | DFS | The call stack is the path |
| Path sums, tree diameter, lowest common ancestor | DFS | Combine child answers at the parent |
| Does a path exist, is it connected | Either | DFS is usually less code |
| Count connected components or islands | Either | DFS recursion is shortest, unless the input is huge |
| Cycle detection | DFS | Needs the current path, which BFS does not have |
| Topological order | Either | Kahn's queue, or DFS post-order. See [topological sort](../patterns/topological-sort.md) |
| Every valid configuration | DFS | That is [backtracking](../patterns/backtracking.md) |
| Shortest path with different edge weights | Neither | Dijkstra with a priority queue |

## The three mistakes

**Marking visited on pop instead of on push.** In BFS, a node can be pushed by several neighbors
before it is ever popped. Mark it when you push it, or the queue fills with duplicates and the search
stops being linear.

```
push start, mark start
while queue not empty:
    node = pop
    for neighbor in graph[node]:
        if neighbor not marked:
            mark neighbor        <- here, not after popping it
            push neighbor
```

**Using BFS on a weighted graph and calling it shortest.** BFS counts hops. If edges have different
costs, the fewest hops and the cheapest route are different answers.

**Recursing too deep.** A DFS on a grid of a million cells that is all land recurses a million frames.
Say this out loud and offer the iterative version, or switch to BFS.

## Distance without a distance variable

A neat BFS trick worth knowing: instead of storing a distance with every queued node, process the
queue one full level at a time and count the levels.

```
level = 0
while queue not empty:
    for _ in range(len(queue)):     # exactly the nodes at this distance
        node = pop
        ...
    level += 1
```

Reading the queue size **before** the inner loop is what separates one level from the next, since the
inner loop pushes onto the same queue while it runs. That single line is the most commonly missed
detail in the whole [level order](../patterns/tree-level-order-traversal.md) pattern.

## Multi-source BFS

If several starting points spread at the same time, push all of them before the loop starts. Nothing
else changes. This is how "how many minutes until every orange rots" and "distance to the nearest 0"
are solved, and it is much simpler than running one search per source.

## Go deeper

- [Graphs](../patterns/graphs.md), [Tree Level Order Traversal](../patterns/tree-level-order-traversal.md), [Tree Depth First Search](../patterns/tree-depth-first-search.md), [Island Traversal](../patterns/island-matrix-traversal.md)
- Graph algorithms beyond these two: [Grokking Graph Algorithms](https://www.designgurus.io/course/grokking-graph-algorithms-for-coding-interviews)
