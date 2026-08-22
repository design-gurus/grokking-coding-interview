# Complexity Cheat Sheet

Every pattern, every data structure, and every common algorithm, with what it costs. Use it to check
an answer before you commit to it, and to answer "what is the complexity" without hesitating.

## The patterns

| Pattern | Time | Space | Note |
|---|---|---|---|
| [Two Pointers](../patterns/two-pointers.md) | O(n) | O(1) | O(n log n) if you sort first |
| [Fast and Slow Pointers](../patterns/fast-and-slow-pointers.md) | O(n) | O(1) | |
| [Sliding Window](../patterns/sliding-window.md) | O(n) | O(1) or O(k) | k is the alphabet or the distinct values in the window |
| [Merge Intervals](../patterns/merge-intervals.md) | O(n log n) | O(n) | The sort dominates |
| [Cyclic Sort](../patterns/cyclic-sort.md) | O(n) | O(1) | Amortized, each swap places one number for good |
| [In-place Reversal](../patterns/in-place-reversal-of-a-linked-list.md) | O(n) | O(1) | |
| [Stacks](../patterns/stacks.md) | O(n) | O(n) | |
| [Monotonic Stack](../patterns/monotonic-stack.md) | O(n) | O(n) | Amortized, each index is pushed and popped once |
| [Hash Maps](../patterns/hash-maps.md) | O(n) average | O(n) | O(n²) worst case in theory, not in practice |
| [Tree Level Order](../patterns/tree-level-order-traversal.md) | O(n) | O(w) | w is the widest level, about n / 2 for a complete tree |
| [Tree Depth First Search](../patterns/tree-depth-first-search.md) | O(n) | O(h) | h is the height, O(log n) balanced, O(n) skewed |
| [Graphs](../patterns/graphs.md) | O(V + E) | O(V + E) | |
| [Island Traversal](../patterns/island-matrix-traversal.md) | O(rows × cols) | O(rows × cols) | |
| [Two Heaps](../patterns/two-heaps.md) | O(log n) insert, O(1) query | O(n) | |
| [Subsets](../patterns/subsets.md) | O(n × 2^n) | O(n × 2^n) | The output is the space |
| [Modified Binary Search](../patterns/modified-binary-search.md) | O(log n) | O(1) | O(n log range) when searching the answer |
| [Bitwise XOR](../patterns/bitwise-xor.md) | O(n) | O(1) | |
| [Top K Elements](../patterns/top-k-elements.md) | O(n log k) | O(k) | Against O(n log n) for a full sort |
| [K-way Merge](../patterns/k-way-merge.md) | O(N log K) | O(K) | N is every element, K is the number of lists |
| [Greedy](../patterns/greedy-algorithms.md) | O(n log n) | O(1) | The sort dominates |
| [0/1 Knapsack](../patterns/0-1-knapsack.md) | O(n × C) | O(C) | Pseudo-polynomial, C is the capacity |
| [Fibonacci Numbers](../patterns/fibonacci-numbers.md) | O(n) | O(1) | O(n × k) when each position has k moves |
| [Palindromic Subsequence](../patterns/palindromic-subsequence.md) | O(n²) | O(n²) | Often reducible to O(n) space |
| [Backtracking](../patterns/backtracking.md) | O(b^d) | O(d) | Branching factor b, depth d. Pruning changes the real time, not the worst case |
| [Trie](../patterns/trie.md) | O(L) per word | O(total characters) | L is the length of the word |
| [Topological Sort](../patterns/topological-sort.md) | O(V + E) | O(V + E) | |
| [Union Find](../patterns/union-find.md) | O(α(n)) per operation | O(n) | Effectively O(1) with path compression and union by size |
| [Ordered Set](../patterns/ordered-set.md) | O(log n) | O(n) | |
| [Prefix Sum](../patterns/prefix-sum.md) | O(n) build, O(1) query | O(n) | |
| [Multi-threaded](../patterns/multi-threaded.md) | same as single-threaded | varies | Correctness is what is graded, not the bound |
| [Counting](../patterns/counting.md) | O(n) | O(k) | k is the distinct values, O(1) for a fixed alphabet |
| [Monotonic Queue](../patterns/monotonic-queue.md) | O(n) | O(k) | |
| [Simulation](../patterns/simulation.md) | O(steps × work) | O(state) | |
| [Linear Sorting](../patterns/linear-sorting-algorithms.md) | O(n + k) | O(k) | k is the value range |
| [Meet in the Middle](../patterns/meet-in-the-middle.md) | O(2^(n/2) × n) | O(2^(n/2)) | |
| [Mo's Algorithm](../patterns/mos-algorithm.md) | O((n + q) √n) | O(n) | |
| [Serialize and Deserialize](../patterns/serialize-and-deserialize.md) | O(n) | O(n) | |
| [Clone](../patterns/clone.md) | O(V + E) | O(V) | |
| [Articulation Points](../patterns/articulation-points-and-bridges.md) | O(V + E) | O(V) | |
| [Segment Tree](../patterns/segment-tree.md) | O(log n) query and update, O(n) build | O(4n) | |
| [Binary Indexed Tree](../patterns/binary-indexed-tree.md) | O(log n) | O(n) | |

## Data structure operations

| Structure | Access | Search | Insert | Delete | Note |
|---|---|---|---|---|---|
| Array | O(1) | O(n) | O(n) | O(n) | Insert and delete shift everything after |
| Dynamic array | O(1) | O(n) | O(1) amortized at the end | O(n) | The occasional resize is paid off across the cheap appends |
| Sorted array | O(1) | O(log n) | O(n) | O(n) | |
| Linked list | O(n) | O(n) | O(1) given the node | O(1) given the node | Finding the node is the expensive part |
| Stack | O(1) top | O(n) | O(1) | O(1) | |
| Queue | O(1) front | O(n) | O(1) | O(1) | Never use a plain array, removing from the front is O(n) |
| Deque | O(1) both ends | O(n) | O(1) | O(1) | |
| Hash map or set | n/a | O(1) average | O(1) average | O(1) average | O(n) worst case, and no ordering |
| Heap | O(1) for the extreme only | O(n) | O(log n) | O(log n) | Building from n items is O(n), not O(n log n) |
| Balanced BST, ordered set | O(log n) | O(log n) | O(log n) | O(log n) | Also gives floor, ceiling, and in-order iteration |
| Trie | O(L) | O(L) | O(L) | O(L) | Independent of how many words are stored |
| Union Find | n/a | O(α(n)) | O(α(n)) | n/a | No delete |

## Sorting

| Algorithm | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Merge sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quicksort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heapsort | O(n log n) | O(n log n) | O(n log n) | O(1) | No |
| Insertion sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Counting sort | O(n + k) | O(n + k) | O(n + k) | O(k) | Yes |
| Radix sort | O(d(n + k)) | O(d(n + k)) | O(d(n + k)) | O(n + k) | Yes |

The built-in sort in Python and Java for objects is Timsort, which is stable and O(n log n) worst
case. Java's sort for primitives is a dual-pivot quicksort, which is **not** stable and has an
O(n²) worst case. Knowing which one your language uses is a good answer to a follow-up question.

## Graph algorithms

| Algorithm | Time | Note |
|---|---|---|
| BFS, DFS | O(V + E) | Shortest path only when every edge costs the same |
| Dijkstra with a binary heap | O((V + E) log V) | No negative edge weights |
| Bellman-Ford | O(V × E) | Handles negative weights, and detects negative cycles |
| Floyd-Warshall | O(V³) | All pairs, fine when V is a few hundred |
| Topological sort | O(V + E) | Directed acyclic graphs only |
| Kruskal with union find | O(E log E) | Minimum spanning tree |
| Prim with a heap | O(E log V) | Minimum spanning tree |

## Recursion and the call stack

Recursion depth is space. A recursive depth first search on a tree shaped like a list, or on a grid
of a million cells that is all land, recurses that many frames deep and can overflow the stack. Say
so, and offer the iterative version with an explicit stack.

Memoized recursion costs one entry per distinct state, so the space is the number of states, not the
number of calls.

## Go deeper

- [Which complexity the constraints allow](what-complexity-passes.md)
- [How to recognize the pattern in sixty seconds](recognize-the-pattern.md)
- Complexity from the ground up: [Grokking Algorithm Complexity and Big O](https://www.designgurus.io/course/grokking-algorithm-complexity-and-big-o)
