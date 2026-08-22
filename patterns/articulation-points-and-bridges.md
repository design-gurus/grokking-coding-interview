# Pattern: Articulation Points and Bridges

> Find the single points of failure in a network: the one node, or the one link, whose removal splits the graph in two.

## What it is

In an undirected graph, an articulation point is a vertex whose removal disconnects the graph. A bridge is an edge whose removal disconnects it. Both are found by one depth first search using Tarjan's algorithm.

The search records two numbers per node. The discovery time is when the search first reached it. The low-link value is the earliest discovery time reachable from that node's subtree, including one step along a back edge. Comparing a child's low-link against the parent's discovery time answers both questions.

## Recognize it when

- The problem is about critical connections, single points of failure, or which link is essential.
- Removing one node or one edge is what the question asks about.
- The graph is undirected and the words network, router, server, or cable appear.
- The problem asks for the minimum number of removals needed to disconnect something.

**Words that give it away:** "critical connection", "single point of failure", "cut vertex", "bridge", "would disconnect", "minimum days to disconnect".

## How it works

```
1 --- 2 --- 3
      |
      4

removing 2 leaves 1, 3, and 4 all separated, so 2 is an articulation point
every edge here is a bridge, because none of them lies on a cycle
```

The idea in one sentence: an edge is a bridge exactly when it lies on no cycle. A back edge to an ancestor creates a cycle, and the low-link value is how that gets detected.

Two conditions, and the difference between them is easy to get wrong:

- Edge (u, v) is a **bridge** when `low[v] > disc[u]`. The subtree at v cannot reach u or anything above it except through this edge.
- Node u is an **articulation point** when it has a child v with `low[v] >= disc[u]`. Equal counts here, because reaching u itself is not enough to survive u's removal.
- The root is special. It is an articulation point only if it has more than one child in the search tree.

## The code template

```python
def find_bridges(n, connections):
    from collections import defaultdict
    graph = defaultdict(list)
    for a, b in connections:
        graph[a].append(b)
        graph[b].append(a)

    disc = [-1] * n
    low = [0] * n
    bridges = []
    timer = [0]

    def dfs(node, parent):
        disc[node] = low[node] = timer[0]
        timer[0] += 1
        for neighbor in graph[node]:
            if neighbor == parent:
                continue                       # do not go straight back
            if disc[neighbor] == -1:
                dfs(neighbor, node)
                low[node] = min(low[node], low[neighbor])
                if low[neighbor] > disc[node]:
                    bridges.append([node, neighbor])
            else:
                low[node] = min(low[node], disc[neighbor])   # disc, not low

    for start in range(n):
        if disc[start] == -1:
            dfs(start, -1)
    return bridges
```

Note `disc[neighbor]` and not `low[neighbor]` on the back-edge line. Using the low value there is a well-known bug that gives wrong answers on some graphs.

## Complexity

| | |
|---|---|
| Time | O(V + E) |
| Space | O(V) for the arrays and the recursion stack |

## Variations

- Find all the bridges.
- Find all the articulation points, which uses the other comparison and the special root rule.
- Biconnected components, which are the pieces left when the articulation points are removed.
- Two-edge-connected components, which are the pieces left when the bridges are removed.
- Strongly connected components in a directed graph, which is a different Tarjan algorithm built on the same low-link idea.

## Problems that use it

Critical Connections in a Network, Minimum Number of Days to Disconnect Island, Minimize Malware Spread, Minimize Malware Spread II, Find Critical and Pseudo-Critical Edges.

## Common mistakes

- Mixing up the two comparisons. Bridges use strictly greater than. Articulation points use greater than or equal.
- Forgetting the root rule, which reports the root as an articulation point whenever it has any children at all.
- Taking the minimum with `low[neighbor]` on a back edge instead of `disc[neighbor]`.
- Skipping the parent by value when the graph has parallel edges. Two separate edges between the same pair mean neither is a bridge, so track the edge you came in on, not the node.
- Applying this to a directed graph. It does not carry over without changes.
- Only searching from node 0, which misses every other component in a disconnected graph.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
