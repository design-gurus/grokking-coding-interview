# The 41 Coding Interview Patterns

Every pattern in [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview), one page each, in course order.

Read the middle column first. It is the cue you get from the problem statement, before you know how to solve anything. Learning to read that cue quickly is the whole point of pattern-based preparation.

If you would rather work backwards from the problem, start with the [pattern selection guide](../cheat-sheets/recognize-the-pattern.md).

## Core patterns

| # | Pattern | Recognize it when | Typical cost |
|---|---------|-------------------|--------------|
| 1 | [Two Pointers](two-pointers.md) | Sorted input, looking for a pair or triplet, or an in-place rewrite | O(n) |
| 2 | [Fast and Slow Pointers](fast-and-slow-pointers.md) | Linked list, cycle or middle, and no extra memory allowed | O(n) |
| 3 | [Sliding Window](sliding-window.md) | A contiguous subarray or substring, longest or shortest | O(n) |
| 4 | [Merge Intervals](merge-intervals.md) | Ranges that may overlap: meetings, bookings, time slots | O(n log n) |
| 5 | [Cyclic Sort](cyclic-sort.md) | Values are a dense range 1 to n, and one is missing or duplicated | O(n) |
| 6 | [In-place Reversal of a Linked List](in-place-reversal-of-a-linked-list.md) | Reverse all or part of a list, in place | O(n) |
| 7 | [Stacks](stacks.md) | Nesting, matching, undo, or "the most recent" | O(n) |
| 8 | [Monotonic Stack](monotonic-stack.md) | Next or previous greater or smaller element, spans, histograms | O(n) |
| 9 | [Hash Maps](hash-maps.md) | Seen before, how many times, or grouped by a computed key | O(n) |
| 10 | [Tree Level Order Traversal](tree-level-order-traversal.md) | An answer per level, or the shallowest match | O(n) |
| 11 | [Tree Depth First Search](tree-depth-first-search.md) | An answer per path, or an ancestor to descendant relationship | O(n) |
| 12 | [Graphs](graphs.md) | Nodes and edges, reachability, connectivity, fewest hops | O(V + E) |
| 13 | [Island (Matrix Traversal)](island-matrix-traversal.md) | A 2D grid where neighboring cells form groups | O(rows × cols) |
| 14 | [Two Heaps](two-heaps.md) | The median, or the middle, of a stream of numbers | O(log n) per insert |
| 15 | [Subsets](subsets.md) | Generate all subsets, permutations, or combinations | O(n × 2^n) |
| 16 | [Modified Binary Search](modified-binary-search.md) | Sorted, rotated, or monotonic input, or a search over the answer | O(log n) |
| 17 | [Bitwise XOR](bitwise-xor.md) | Everything appears in pairs except one, in O(1) space | O(n) |
| 18 | [Top K Elements](top-k-elements.md) | Top K, Kth largest, or K most frequent | O(n log k) |
| 19 | [K-way Merge](k-way-merge.md) | Several sorted lists merged into one | O(N log K) |
| 20 | [Greedy Algorithms](greedy-algorithms.md) | One sort makes every choice obvious, and you never look back | O(n log n) |
| 21 | [0/1 Knapsack](0-1-knapsack.md) | Take it or leave it, under a capacity or a target sum | O(n × C) |
| 22 | [Fibonacci Numbers](fibonacci-numbers.md) | Each answer built from a fixed number of earlier answers | O(n) |
| 23 | [Palindromic Subsequence](palindromic-subsequence.md) | Palindromes, or two strings compared character by character | O(n²) |
| 24 | [Backtracking](backtracking.md) | Build a configuration, undo it, and prune what cannot work | Exponential |
| 25 | [Trie](trie.md) | A dictionary searched by prefix, or autocomplete | O(L) per word |
| 26 | [Topological Sort](topological-sort.md) | Prerequisites, dependencies, or a valid build order | O(V + E) |
| 27 | [Union Find](union-find.md) | Connectivity and grouping, while connections keep being added | Effectively O(1) |
| 28 | [Ordered Set](ordered-set.md) | The nearest value above or below, in a set that keeps changing | O(log n) |
| 29 | [Prefix Sum](prefix-sum.md) | Many range sums on a fixed array, or counting subarrays | O(1) per query |
| 30 | [Multi-threaded](multi-threaded.md) | Threads, ordering, and shared state | Correctness, not cost |

## Advanced and specialist patterns

These match the advanced section of the course. Some are narrow, and one of them, Mo's algorithm, is more common in competitive programming than in interviews. Learn the thirty above first.

| # | Pattern | Recognize it when | Typical cost |
|---|---------|-------------------|--------------|
| 31 | [Counting](counting.md) | Frequency, anagrams, duplicates, the majority element | O(n) |
| 32 | [Monotonic Queue](monotonic-queue.md) | A sliding window that also needs its maximum or minimum | O(n) |
| 33 | [Simulation](simulation.md) | The problem describes a process, and the constraints are small | O(steps) |
| 34 | [Linear Sorting Algorithms](linear-sorting-algorithms.md) | The values are integers in a small known range | O(n + k) |
| 35 | [Meet in the Middle](meet-in-the-middle.md) | Subset search where n is about 30 to 45 | O(2^(n/2)) |
| 36 | [Mo's Algorithm](mos-algorithm.md) | Many offline range queries on an array that never changes | O((n + q) × √n) |
| 37 | [Serialize and Deserialize](serialize-and-deserialize.md) | Encode a structure as text and rebuild it | O(n) |
| 38 | [Clone](clone.md) | Deep copy a graph or a structure with extra pointers | O(n + e) |
| 39 | [Articulation Points and Bridges](articulation-points-and-bridges.md) | Single points of failure in an undirected graph | O(V + E) |
| 40 | [Segment Tree](segment-tree.md) | Range queries and updates, including minimum and maximum | O(log n) |
| 41 | [Binary Indexed Tree](binary-indexed-tree.md) | Prefix sums with updates, and counting smaller elements | O(log n) |

## Adding a pattern

Copy [_template.md](_template.md), fill in every section, and add a row to the right table above. See [CONTRIBUTING.md](../CONTRIBUTING.md).
