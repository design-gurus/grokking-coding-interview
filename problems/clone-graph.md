# Clone Graph

> Make a deep copy of a connected undirected graph, sharing no nodes with the original.

**Pattern:** [Clone](../patterns/clone.md) | **Difficulty:** Medium

## The problem

You are given a reference to a node in a connected undirected graph. Each node holds a value and a
list of its neighbors. Return a deep copy of the whole graph: a new set of nodes with the same values
and the same connections, sharing nothing with the original.

## Why this is a Clone problem

The words "deep copy" are the cue, and the difficulty is hidden in the word "graph".

Copying a tree is a plain recursion, because every node has exactly one parent and the walk always
ends. A graph breaks both assumptions. It can contain cycles, so a naive recursion never terminates.
And two different nodes can point at the same neighbor, so a naive recursion would copy that neighbor
twice and produce a structure that is not the same shape as the original.

One map fixes both problems at once: original node to its copy. Before copying a node, check the map.
If it is already there, hand back the existing copy. That single check both ends the recursion on a
cycle and guarantees exactly one copy per original.

## The approach

1. Keep a map from original node to cloned node.
2. To clone a node:
   - If it is already in the map, return the stored clone.
   - Create a new node with the same value, and **put it in the map before recursing**.
   - For each neighbor of the original, clone that neighbor and add the result to the new node's
     neighbor list.
   - Return the new node.

The invariant: the map holds a clone for every original node already reached, whether or not its
neighbor list is finished yet.

Recording the clone in the map before you recurse is the line the whole solution turns on. Record it
afterwards, and a cycle reaches the node again while it is still absent from the map, so the recursion
never ends.

A breadth first version works the same way: create the clone when you first see a node, push it, and
wire the neighbor lists as you pop.

## Complexity

| | |
|---|---|
| Time | O(V + E). Every node is created once and every edge is followed once from each side. |
| Space | O(V) for the map, plus O(V) for the recursion or the queue |

## Edge cases to say out loud

- A null input node, and a graph of one node with no neighbors.
- A **self-loop**, where a node lists itself as a neighbor. The map check handles it, and it is worth
  saying so.
- The graph being undirected, so every edge appears in two neighbor lists. Each copy must end up in
  both.
- Duplicate values on different nodes. The map key must be the node itself, not its value.
- Whether the graph is connected. This problem says it is. If it were not, one starting node would not
  reach everything, and you would need a pass over all nodes.

## Related problems

- Copy List with Random Pointer, the same map on a linked list with an extra pointer. It also has a
  neat O(1) space solution: weave each copy in just after its original, so `node.next` is always that
  node's clone, wire the random pointers using that, then unweave.
- Clone N-ary Tree and Clone Binary Tree With Random Pointer, the same idea on tree shapes.
- [Serialize and Deserialize Binary Tree](serialize-and-deserialize-binary-tree.md), which is the same
  problem solved through text rather than through a map.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
