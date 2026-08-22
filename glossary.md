# Glossary

Terms that appear across this repo, in plain language. If a term links to a pattern page, that page explains it in full.

## Complexity

**Big O.** An upper bound on how the cost of an algorithm grows as the input grows. O(n) means the work grows in step with the input. O(n²) means doubling the input roughly quadruples the work. Constants and smaller terms are dropped, so O(3n + 5) is written O(n).

**Amortized.** The average cost per operation across a long run, even though one individual operation may be expensive. Appending to a dynamic array is amortized O(1): most appends are instant, and the occasional resize is paid off across all the cheap ones.

**Pseudo-polynomial.** A running time that looks polynomial but depends on the size of a number in the input rather than on how many inputs there are. The [0/1 knapsack](patterns/0-1-knapsack.md) table is O(n × C), and a capacity C of one billion makes that unusable.

**In place.** The algorithm rearranges the input itself and uses only a constant amount of extra memory. Sorting in place is allowed to use O(1) or O(log n) extra space, depending on who you ask, so state which you mean.

**Stable sort.** A sort that keeps equal elements in their original relative order. This matters when the elements carry extra data, and it is what makes radix sort work.

## Structure and shape

**Subarray.** A contiguous stretch of an array. [1,2,3] has subarrays like [2,3], but not [1,3].

**Subsequence.** Elements taken from an array in order, but not necessarily next to each other. [1,3] is a subsequence of [1,2,3].

**Substring.** A subarray of a string. Contiguous, like a subarray.

**Prefix.** Everything from the start of a sequence up to some position. **Suffix** is the mirror, everything from a position to the end.

**Invariant.** Something that stays true at every step of a loop or a recursion. Naming the invariant out loud is often the fastest way to convince an interviewer, and yourself, that the code is correct.

**Monotonic.** Only ever increasing, or only ever decreasing. A [monotonic stack](patterns/monotonic-stack.md) keeps its contents in one of those two orders.

## Trees and graphs

**Node and edge.** A node is a thing. An edge is a connection between two things. A graph is a set of both.

**Directed.** Edges point one way. "A is a prerequisite of B" is directed. "A and B are friends" usually is not.

**Cycle.** A path that returns to where it started. Detecting one is the actual question in many [topological sort](patterns/topological-sort.md) and [graph](patterns/graphs.md) problems.

**Connected component.** A group of nodes that can all reach each other, and cannot reach anything outside the group.

**Adjacency list.** A map from each node to the list of its neighbors. This is how a list of edges gets turned into something you can walk.

**Depth and height.** The depth of a node is its distance from the root. The height of a tree is the depth of its deepest node.

**Leaf.** A node with no children. A null child is not a leaf, and confusing the two produces wrong answers on trees where a node has only one child.

**Binary search tree.** A binary tree where everything in the left subtree is smaller than the node and everything in the right subtree is larger. An in-order walk of one produces the values in sorted order.

**Balanced.** A tree whose height is about log n rather than n. An unbalanced tree can be a straight line, and every O(log n) claim about it becomes O(n).

**Back edge.** An edge found during a depth first search that points to an ancestor already on the current path. Back edges are what create cycles, which is the basis of the [articulation points](patterns/articulation-points-and-bridges.md) algorithm.

## Data structures

**Hash map.** A structure that maps keys to values with O(1) average lookup. See [hash maps](patterns/hash-maps.md).

**Heap, or priority queue.** A structure that gives you the smallest, or the largest, element in O(1) and removes it in O(log n). It says nothing about the order of everything else. See [top K elements](patterns/top-k-elements.md).

**Deque.** A double-ended queue. You can add and remove at both ends in O(1). See [monotonic queue](patterns/monotonic-queue.md).

**Ordered set.** A set that stays sorted, so it can answer "what is the nearest value above this one". See [ordered set](patterns/ordered-set.md).

**Trie.** A tree where each edge is one character, so a path spells a prefix. See [trie](patterns/trie.md).

**Coordinate compression.** Replacing large, sparse values with their rank, so the values 5, 1000, and 10^9 become 1, 2, and 3. It is what makes a [binary indexed tree](patterns/binary-indexed-tree.md) usable on huge values.

## Techniques

**Memoization.** Caching the result of a recursive call so it is computed once. Also called top-down dynamic programming.

**Tabulation.** Filling a table from the base cases upward, so every value is ready when it is needed. Also called bottom-up dynamic programming.

**Pruning.** Abandoning a branch of a search as soon as it is clear it cannot lead to an answer. It is what makes [backtracking](patterns/backtracking.md) usable.

**Sentinel.** An extra value added to the input, chosen so the main loop handles the last case without a special branch. The trailing zero in the histogram problem is a sentinel.

**Dummy node.** A placeholder node placed before the head of a linked list, so operations on the first node need no special case. See [in-place reversal](patterns/in-place-reversal-of-a-linked-list.md).

**Lazy deletion.** Marking an element as removed and discarding it later, when it surfaces, because the structure cannot remove from the middle cheaply. Used with heaps in sliding window problems.

**Binary search on the answer.** Searching the range of possible answers rather than the input, using a test that says whether a candidate works. See [modified binary search](patterns/modified-binary-search.md).

**Offline queries.** All the queries are known before any of them is answered, so they can be reordered. [Mo's algorithm](patterns/mos-algorithm.md) needs this. **Online** means each answer must be produced before the next query is revealed.

## Interview words

**Brute force.** The obvious, usually too slow solution. Always state it before optimizing. It proves you understood the problem and it gives you something to improve.

**Edge case.** An input at the boundary of what is allowed: empty, one element, all equal, the maximum size, negative numbers. Interviewers give credit for naming these before you start coding.

**Dry run.** Walking through your code by hand on a small input. The fastest way to find an off-by-one error, and the expected thing to do before saying you are finished.
