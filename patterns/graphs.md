# Pattern: Graphs

> Model the input as nodes and edges, then walk it with a queue or a stack, marking what you have seen so nothing is explored twice.

## What it is

A graph is a set of nodes and the connections between them. Most of the work in a graph problem happens before the traversal. Turn the input into an adjacency list, decide whether the edges point one way or both ways, and decide what counts as a node.

After that, there are only two walks. A queue explores in rings of increasing distance, which finds the fewest hops. A stack, or recursion, follows one route to its end before backing up, which is better for reachability and for building paths.

## Recognize it when

- The input describes entities and relationships: friends, flights, routes, dependencies, networks, word transformations.
- The question is about reachability, connectivity, the number of groups, or the fewest steps.
- The input is a list of edges or pairs, which is a graph written down.
- A grid is given and each cell connects to its neighbors. That is a graph too, and it usually goes to the [island](island-matrix-traversal.md) pattern.

**Words that give it away:** "connected", "path exists", "reachable", "fewest steps", "network", "routes", "friend circles", "provinces", "components".

## How it works

Build an adjacency list first. Then pick the walk that matches the question.

```
edges: (0,1) (1,2) (3,4)

adjacency: 0 -> [1]
           1 -> [0, 2]
           2 -> [1]
           3 -> [4]
           4 -> [3]

BFS from 0 reaches 0, 1, 2. Node 3 is never reached, so the graph
has two components and you need a fresh start from every unvisited node.
```

Breadth first search finds the shortest path when every edge costs the same. If the edges have different weights, that is Dijkstra's algorithm and a priority queue, not a plain queue.

## The code template

```python
from collections import deque, defaultdict

def build_graph(n, edges, directed=False):
    graph = defaultdict(list)
    for a, b in edges:
        graph[a].append(b)
        if not directed:
            graph[b].append(a)
    return graph


def shortest_hops(graph, start, goal):
    visited = {start}                       # mark on push, not on pop
    queue = deque([(start, 0)])
    while queue:
        node, distance = queue.popleft()
        if node == goal:
            return distance
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, distance + 1))
    return -1


def count_components(n, graph):
    visited, components = set(), 0
    for start in range(n):                  # a disconnected graph needs this loop
        if start in visited:
            continue
        components += 1
        stack = [start]
        visited.add(start)
        while stack:
            node = stack.pop()
            for neighbor in graph[node]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    stack.append(neighbor)
    return components
```

## Complexity

| | |
|---|---|
| Time | O(V + E), where V is the number of nodes and E the number of edges |
| Space | O(V + E) for the adjacency list, plus O(V) for the visited set |

## Variations

- Breadth first search for the fewest hops on an unweighted graph.
- Depth first search for reachability and for building paths.
- Multi-source search, where several starting nodes go into the queue at once.
- Two-coloring to test whether a graph is bipartite.
- Cycle detection, which differs between directed and undirected graphs.
- Implicit graphs, where the nodes are states rather than given objects. Word ladders and puzzle boards are the usual examples.
- For ordering under dependencies, use [topological sort](topological-sort.md). For grouping and connectivity queries, [union find](union-find.md) is often simpler.

## Problems that use it

Find if Path Exists in Graph, Number of Provinces, Word Ladder, Bus Routes, Find Eventual Safe States, Minimum Number of Vertices to Reach All Nodes, Clone Graph, Course Schedule, Open the Lock.

## Common mistakes

- Leaving out the visited set, which loops forever on any cycle.
- Marking a node visited when you pop it rather than when you push it. The node can be pushed many times before it is popped, which turns a linear search into an exponential one.
- Assuming the graph is connected. If it is not, you need an outer loop that starts a fresh search from every unvisited node.
- Building an undirected adjacency list for a directed problem, or the reverse. Prerequisites and flights are directed. Friendships and roads usually are not.
- Using breadth first search on a weighted graph and expecting the shortest path. That needs a priority queue.
- Forgetting that the input node labels might not be 0 to n minus 1, so a dictionary is safer than an array.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- The data structure itself, from scratch: [Grokking Data Structures](https://www.designgurus.io/course/grokking-data-structures-for-coding-interviews)
