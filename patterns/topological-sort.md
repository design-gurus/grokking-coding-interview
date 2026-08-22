# Pattern: Topological Sort

> Order the nodes of a directed graph so every arrow points forward. If that is impossible, the graph has a cycle, which is often the real question.

## What it is

Given tasks with prerequisites, a topological order is any sequence where each task appears after everything it depends on. Kahn's algorithm builds it by repeatedly taking a task with nothing left blocking it.

The failure mode is as useful as the success. If you run out of unblocked tasks before placing them all, the ones left over depend on each other in a loop, so no valid order exists. That is how course-schedule problems detect impossible input.

## Recognize it when

- The input describes prerequisites, dependencies, build order, or steps that must happen before other steps.
- The problem asks for a valid order, all valid orders, or whether an order exists at all.
- The problem asks whether a directed graph has a cycle.
- Ordering information is spread across pairs, as in the alien dictionary, where each pair of adjacent words reveals one ordering rule.

**Words that give it away:** "prerequisite", "dependency", "before", "build order", "course schedule", "can you finish", "valid order", "alien dictionary".

## How it works

Count how many arrows point **into** each node. That count is its in-degree, which is the number of things still blocking it.

```
edges: A -> C, B -> C, C -> D

in-degree:  A 0   B 0   C 2   D 1

queue starts with A and B, the unblocked ones
take A, C drops to 1
take B, C drops to 0, so C joins the queue
take C, D drops to 0, so D joins the queue
take D
order: A, B, C, D
```

If the finished order is shorter than the number of nodes, the leftovers form a cycle.

## The code template

```python
from collections import deque, defaultdict

def topological_order(n, edges):
    """edges are (before, after) pairs. Returns an order, or [] if there is a cycle."""
    graph = defaultdict(list)
    in_degree = [0] * n
    for before, after in edges:
        graph[before].append(after)
        in_degree[after] += 1

    queue = deque(node for node in range(n) if in_degree[node] == 0)
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1        # one blocker removed
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return order if len(order) == n else []   # short means a cycle
```

Swap the queue for a min-heap and you get the lexicographically smallest valid order, which some problems ask for.

## Complexity

| | |
|---|---|
| Time | O(V + E) |
| Space | O(V + E) for the graph, the in-degree array, and the queue |

## Variations

- One valid order, using a queue.
- Detect whether any order exists, which is cycle detection.
- The lexicographically smallest order, using a min-heap.
- All valid orders, using [backtracking](backtracking.md) over the choices whenever more than one node is unblocked. This is exponential.
- Is the order unique, which is true when the queue never holds more than one node.
- Alien dictionary, where the edges have to be derived from adjacent word pairs first.
- The depth first variant, which pushes nodes onto a stack after visiting all their descendants.

## Problems that use it

Task Scheduling, Task Scheduling Order, All Tasks Scheduling Orders, Course Schedule, Course Schedule II, Alien Dictionary, Reconstructing a Sequence, Minimum Height Trees, Sequence Reconstruction.

## Common mistakes

- Building an undirected graph. Adding both directions makes every in-degree at least one, so the queue starts empty and nothing is ever ordered.
- Getting the edge direction backwards. "To take course A you need course B" means the edge runs from B to A.
- Forgetting the length check at the end, which returns a partial order on cyclic input instead of reporting failure.
- Missing nodes that have no edges at all. They belong in the answer, and they start with in-degree zero.
- Decrementing the in-degree more than once for a duplicate edge, which can leave a node permanently blocked. Deduplicate the edges if the input allows repeats.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
