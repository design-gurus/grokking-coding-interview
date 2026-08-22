# All 41 Patterns, on One Page

The night-before revision sheet. One card per pattern, in course order, in the order you should read them: the cue first, then the idea, then the cost, then the mistake that catches people.

You do not need to memorize the example problems. You need to read a problem you have never seen and say "that is a sliding window with a frequency map". Read the **Recognize it when** lines and nothing else if you are short on time.

Each card links to the full page, which adds the mechanism, a code template, the variations, and the rest of the mistakes.

## Core patterns

### 1. Two Pointers

**Core idea.** Two indices walk the same array instead of two nested loops, which turns an O(n²) scan into a single O(n) pass.

**Recognize it when.** "sorted array", "pair that sums to", "triplet", "remove duplicates in place", "without using extra space", "two ends".

**Cost.** Time: O(n), or O(n log n) if you have to sort first. Space: O(1)

**Watch out for.** Using opposite-end pointers on an unsorted array. The direction rule is only valid because the array is sorted. On unsorted input you usually want a [hash map](../patterns/hash-maps.md) instead.

[Full page](../patterns/two-pointers.md)

### 2. Fast and Slow Pointers

**Core idea.** Two pointers move through the same structure at different speeds, and the gap between them reveals cycles, midpoints, and repetition without any extra memory.

**Recognize it when.** "cycle", "loop", "middle node", "kth from the end", "palindrome linked list", "constant space".

**Cost.** Time: O(n). Space: O(1)

**Watch out for.** Not checking both `fast` and `fast.next` before taking two steps, which crashes on a null pointer.

[Full page](../patterns/fast-and-slow-pointers.md)

### 3. Sliding Window

**Core idea.** Carry the answer for one window forward to the next one, so a range of the array costs O(1) per step instead of being recomputed from scratch.

**Recognize it when.** "subarray", "substring", "contiguous", "window of size k", "longest", "smallest", "at most k distinct".

**Cost.** Time: O(n). Each index enters the window once and leaves once.. Space: O(1), or O(k) when the window keeps a frequency map

**Watch out for.** Forgetting to undo the aggregate when the left edge moves. Every add needs a matching remove.

[Full page](../patterns/sliding-window.md)

### 4. Merge Intervals

**Core idea.** Sort the intervals by start time, and a problem about overlapping ranges becomes a single left-to-right sweep.

**Recognize it when.** "intervals", "meetings", "overlap", "merge", "conflict", "free time", "rooms", "start and end time".

**Cost.** Time: O(n log n), dominated by the sort. Space: O(n) for the output, or O(1) extra if merging in place

**Watch out for.** Not deciding what to do with touching intervals like [1,3] and [3,5]. Ask the interviewer whether they count as overlapping. Both answers are defensible, and the code differs by one comparison.

[Full page](../patterns/merge-intervals.md)

### 5. Cyclic Sort

**Core idea.** When the values are a known range like 1 to n, every value already knows which index it belongs at, so you can sort in O(n) with no extra memory.

**Recognize it when.** "contains n numbers taken from the range 1 to n", "one number is missing", "one number appears twice", "without extra space", "in O(n) time".

**Cost.** Time: O(n). Space: O(1)

**Watch out for.** Advancing the index after a swap. The swap brought a new value to the current index, and that value has not been placed yet.

[Full page](../patterns/cyclic-sort.md)

### 6. In-place Reversal of a Linked List

**Core idea.** Rewire the next pointers as you walk, and a linked list reverses itself using three variables and no extra memory.

**Recognize it when.** "reverse the linked list", "reverse a sub-list", "every k nodes", "in place", "without extra space", "rotate the list".

**Cost.** Time: O(n). Space: O(1)

**Watch out for.** Overwriting `current.next` before saving it, which loses the rest of the list.

[Full page](../patterns/in-place-reversal-of-a-linked-list.md)

### 7. Stacks

**Core idea.** A stack always hands back the most recent unfinished item, which is exactly what matching, nesting, and undo problems ask for.

**Recognize it when.** "valid parentheses", "nested", "matching", "undo", "evaluate the expression", "simplify the path", "reverse", "most recent".

**Cost.** Time: O(n). Each item is pushed once and popped at most once.. Space: O(n)

**Watch out for.** Popping without checking that the stack is not empty, which crashes on input like `)))`.

[Full page](../patterns/stacks.md)

### 8. Monotonic Stack

**Core idea.** Keep the stack sorted, and every element you pop has just been answered by the element that pushed it out.

**Recognize it when.** "next greater", "next smaller", "previous greater", "nearest", "warmer", "span", "largest rectangle", "how many days until".

**Cost.** Time: O(n). Every index is pushed once and popped once, even with the inner while loop.. Space: O(n)

**Watch out for.** Picking the wrong direction. Decide first whether you want the next larger or the next smaller, then choose the comparison that pops the other kind.

[Full page](../patterns/monotonic-stack.md)

### 9. Hash Maps

**Core idea.** Remember what you have already seen, keyed by value, and a nested search collapses into a single pass.

**Recognize it when.** "duplicate", "unique", "first non-repeating", "how many times", "group by", "anagram", "seen before", "in O(n) time".

**Cost.** Time: O(n) on average. The worst case is O(n²) if every key collides, which does not happen in practice with a good hash.. Space: O(n)

**Watch out for.** Claiming O(1) worst case. The guarantee is O(1) on average, and an interviewer may ask you to say so.

[Full page](../patterns/hash-maps.md)

### 10. Tree Level Order Traversal

**Core idea.** A queue visits a tree one level at a time, so the first node that matches is always the one closest to the root.

**Recognize it when.** "level", "row", "zigzag", "minimum depth", "shortest", "right side view", "next pointer", "width of the tree".

**Cost.** Time: O(n), where n is the number of nodes. Space: O(w), where w is the widest level. For a complete tree that is about n / 2.

**Watch out for.** Not capturing the level size before the inner loop, which mixes nodes from two levels into one list.

[Full page](../patterns/tree-level-order-traversal.md)

### 11. Tree Depth First Search

**Core idea.** Go all the way down one branch before trying the next, so every question about a path has the whole path in hand when it reaches the bottom.

**Recognize it when.** "root to leaf", "path", "sum along", "depth", "height", "diameter", "ancestor", "subtree", "in-order".

**Cost.** Time: O(n) to visit every node. Path enumeration costs O(n × path length) because each path is copied.. Space: O(h) for the call stack, where h is the height. That is O(log n) for a balanced tree and O(n) for a tree shaped like a list.

**Watch out for.** Missing the null base case, which crashes on the first leaf.

[Full page](../patterns/tree-depth-first-search.md)

### 12. Graphs

**Core idea.** Model the input as nodes and edges, then walk it with a queue or a stack, marking what you have seen so nothing is explored twice.

**Recognize it when.** "connected", "path exists", "reachable", "fewest steps", "network", "routes", "friend circles", "provinces", "components".

**Cost.** Time: O(V + E), where V is the number of nodes and E the number of edges. Space: O(V + E) for the adjacency list, plus O(V) for the visited set

**Watch out for.** Leaving out the visited set, which loops forever on any cycle.

[Full page](../patterns/graphs.md)

### 13. Island (Matrix Traversal)

**Core idea.** A grid is a graph where every cell is connected to its neighbors, so counting islands is counting connected components.

**Recognize it when.** "grid", "matrix", "island", "region", "connected cells", "flood fill", "surrounded", "rotting", "shortest path in a maze".

**Cost.** Time: O(rows × cols). Every cell is visited once.. Space: O(rows × cols) in the worst case, for the stack, the queue, or the visited grid

**Watch out for.** Not marking a cell as visited, which loops forever between two neighbors.

[Full page](../patterns/island-matrix-traversal.md)

### 14. Two Heaps

**Core idea.** Split the numbers into a smaller half and a larger half, and the middle of the data sits at the top of the two heaps, ready to read in O(1).

**Recognize it when.** "median", "data stream", "as numbers arrive", "middle element", "maximize capital", "next interval".

**Cost.** Time: O(log n) per insert, O(1) per median query. Space: O(n)

**Watch out for.** Forgetting to rebalance, which lets one heap grow while the other stays empty and destroys the median.

[Full page](../patterns/two-heaps.md)

### 15. Subsets

**Core idea.** Every existing partial answer produces new partial answers when the next element is considered, which builds the whole power set without any recursion.

**Recognize it when.** "all possible", "generate every", "power set", "combinations", "permutations", "distinct subsets".

**Cost.** Time: O(n × 2^n) for subsets, O(n × n!) for permutations. The n factor is the cost of copying each result.. Space: Same as time, because the output is the space

**Watch out for.** Not sorting before deduplicating. The skip rule only works when equal values are adjacent.

[Full page](../patterns/subsets.md)

### 16. Modified Binary Search

**Core idea.** Binary search is not about sorted arrays. It works wherever you can look at the middle and rule out half the remaining space.

**Recognize it when.** "sorted", "rotated", "log n", "first occurrence", "ceiling", "peak", "minimum capacity", "smallest k such that", "infinite array".

**Cost.** Time: O(log n), or O(n log range) for answer-space search where each test costs O(n). Space: O(1)

**Watch out for.** The infinite loop. If `low` or `high` can be set to `mid` without moving, the loop never ends. Either move past the midpoint, or make sure the interval always shrinks.

[Full page](../patterns/modified-binary-search.md)

### 17. Bitwise XOR

**Core idea.** XOR cancels equal pairs, so everything that appears twice disappears and only the odd one out is left.

**Recognize it when.** "appears twice", "single number", "every element appears exactly", "without extra memory", "bitwise", "complement", "flip the bits".

**Cost.** Time: O(n). Space: O(1)

**Watch out for.** Applying XOR when values appear three times. The cancellation rule needs pairs. Three of a kind needs bit counting instead.

[Full page](../patterns/bitwise-xor.md)

### 18. Top K Elements

**Core idea.** Keep a heap of exactly K items, and the cost of finding the top K drops from sorting everything to O(n log k).

**Recognize it when.** "top k", "k largest", "k closest", "kth smallest", "most frequent", "in a stream".

**Cost.** Time: O(n log k), against O(n log n) for a full sort. Space: O(k), or O(n) when a frequency map is needed first

**Watch out for.** Picking the wrong heap direction. For K largest use a min-heap. For K smallest use a max-heap. Getting this backwards evicts the winners.

[Full page](../patterns/top-k-elements.md)

### 19. K-way Merge

**Core idea.** Hold one candidate from each sorted list in a heap, and the next smallest element across all of them is always at the top.

**Recognize it when.** "k sorted lists", "merge", "sorted matrix", "kth smallest across", "smallest range covering".

**Cost.** Time: O(N log K), where N is the total number of elements and K the number of lists. Space: O(K) for the heap

**Watch out for.** Pushing an empty list's head, which throws an index error before the merge even starts.

[Full page](../patterns/k-way-merge.md)

### 20. Greedy Algorithms

**Core idea.** Take the best-looking option at every step and never look back. The sort you choose first is what makes that safe.

**Recognize it when.** "maximum number of", "minimum number of", "non-overlapping", "assign", "as many as possible", "least effort".

**Cost.** Time: O(n log n), dominated by the sort. Space: O(1) beyond the sort

**Watch out for.** Not justifying the greedy choice. This is the single most common failure in this pattern, and interviewers ask about it directly. Be ready to say why the local choice is safe.

[Full page](../patterns/greedy-algorithms.md)

### 21. 0/1 Knapsack

**Core idea.** One yes-or-no decision per item, and the best answer for each remaining capacity is built from the best answers to smaller capacities.

**Recognize it when.** "subset", "partition", "target sum", "capacity", "at most", "exactly", "maximum value", "can you make".

**Cost.** Time: O(n × C), where C is the capacity. Space: O(n × C) for the full table, O(C) once collapsed to one row

**Watch out for.** Running the capacity loop forwards in the 1D version, which silently solves the unbounded problem instead.

[Full page](../patterns/0-1-knapsack.md)

### 22. Fibonacci Numbers

**Core idea.** Each answer is built from a fixed number of earlier answers, so storing those few turns an exponential recursion into one linear pass.

**Recognize it when.** "how many ways", "climb", "steps", "you can jump", "adjacent", "cannot take two in a row", "minimum jumps".

**Cost.** Time: O(n) when the lookback is fixed, O(n × k) when each position has k moves. Space: O(n) for the table, or O(1) once collapsed to variables

**Watch out for.** Wrong base cases, which shifts every later answer by one. Compute f(0), f(1), and f(2) by hand and check them against your table.

[Full page](../patterns/fibonacci-numbers.md)

### 23. Palindromic Subsequence

**Core idea.** The state is a range of the string rather than a position in it, and the answer for a range depends on its two ends plus the range inside them.

**Recognize it when.** "palindrome", "palindromic", "subsequence", "common subsequence", "edit distance", "insertions to make", "minimum deletions".

**Cost.** Time: O(n²) for one string, O(n × m) for two. Space: O(n²), often reducible to O(n) by keeping only the previous row or diagonal

**Watch out for.** Filling the table in plain index order, which reads cells that have not been written yet. Fill by increasing range length, or iterate i downwards.

[Full page](../patterns/palindromic-subsequence.md)

### 24. Backtracking

**Core idea.** Make a choice, recurse, then undo the choice. Cutting off branches that cannot lead anywhere is what separates this from brute force.

**Recognize it when.** "all possible", "place n queens", "solve the sudoku", "valid combinations", "word search", "partition the string", "generate all".

**Cost.** Time: Exponential in the worst case, often O(b^d) for branching factor b and depth d. Pruning changes the real runtime enormously and the worst case not at all.. Space: O(d) for the recursion stack, plus the size of the output

**Watch out for.** Forgetting the undo, which corrupts every later branch. If your answers contain elements that should not be there, look here first.

[Full page](../patterns/backtracking.md)

### 25. Trie

**Core idea.** Store words in a tree where each edge is one character, so shared prefixes are stored once and prefix questions cost only the length of the prefix.

**Recognize it when.** "prefix", "starts with", "autocomplete", "dictionary", "word search", "suggestions", "implement a data structure that".

**Cost.** Time: O(L) to insert or search a word of length L, whatever the size of the dictionary. Space: O(total characters), and much less when words share prefixes

**Watch out for.** Leaving out the end-of-word marker, which makes every prefix of a stored word look like a stored word.

[Full page](../patterns/trie.md)

### 26. Topological Sort

**Core idea.** Order the nodes of a directed graph so every arrow points forward. If that is impossible, the graph has a cycle, which is often the real question.

**Recognize it when.** "prerequisite", "dependency", "before", "build order", "course schedule", "can you finish", "valid order", "alien dictionary".

**Cost.** Time: O(V + E). Space: O(V + E) for the graph, the in-degree array, and the queue

**Watch out for.** Building an undirected graph. Adding both directions makes every in-degree at least one, so the queue starts empty and nothing is ever ordered.

[Full page](../patterns/topological-sort.md)

### 27. Union Find

**Core idea.** Keep track of which things are in the same group, and answer "are these two connected" in almost constant time, even as groups keep merging.

**Recognize it when.** "connected", "provinces", "friend circles", "redundant connection", "merge accounts", "same group", "number of components".

**Cost.** Time: Effectively O(1) per operation, amortized. The exact bound is the inverse Ackermann function, which is below 5 for any input that fits in a computer.. Space: O(n)

**Watch out for.** Skipping path compression and union by size. Without them the structure degrades to O(n) per operation, and a large test times out.

[Full page](../patterns/union-find.md)

### 28. Ordered Set

**Core idea.** A hash set answers "is this value here". An ordered set answers "what is the nearest value above or below", while the collection keeps changing.

**Recognize it when.** "closest", "just greater than", "just smaller than", "book if it does not clash", "next available", "in a changing set".

**Cost.** Time: O(log n) for insert, delete, floor, and ceiling with a balanced tree. Space: O(n)

**Watch out for.** Losing duplicates. A set stores each value once, so a multiset or a count map is needed when repeats matter.

[Full page](../patterns/ordered-set.md)

### 29. Prefix Sum

**Core idea.** Compute the running totals once, and the sum of any range becomes the difference between two of them.

**Recognize it when.** "range sum", "subarray sum equals", "how many subarrays", "pivot index", "divisible by k", "submatrix sum".

**Cost.** Time: O(n) to build, then O(1) per range query. Space: O(n)

**Watch out for.** Forgetting to seed the map with a zero entry, which misses every subarray that starts at index 0.

[Full page](../patterns/prefix-sum.md)

### 30. Multi-threaded

**Core idea.** Several threads run at once and the answer has to be correct no matter how they interleave. The goal is correctness under concurrency, not a faster big-O.

**Recognize it when.** "thread safe", "concurrently", "in order", "producer consumer", "deadlock", "synchronize", "at the same time".

**Watch out for.** Deadlock from inconsistent lock ordering. If one thread takes A then B while another takes B then A, they can each hold what the other needs. Always acquire locks in the same order.

[Full page](../patterns/multi-threaded.md)

## Advanced and specialist patterns

Narrower tools. Learn the thirty above first.

### 31. Counting

**Core idea.** Count how many times each value appears, then answer the real question from the counts rather than from the input.

**Recognize it when.** "frequency", "occurs", "how many times", "anagram", "majority", "duplicate", "distinct", "most common".

**Cost.** Time: O(n) to count, plus whatever the logic on top costs. Space: O(k), where k is the number of distinct values. O(1) when the alphabet is fixed.

**Watch out for.** Using a hash map where a 26-slot array is enough, which is slower and harder to compare.

[Full page](../patterns/counting.md)

### 32. Monotonic Queue

**Core idea.** A deque kept in sorted order, so the maximum of a sliding window is always at the front and every window costs O(1) to answer.

**Recognize it when.** "sliding window maximum", "every window of size k", "maximum in each subarray", "at most k apart", "shortest subarray with sum at least".

**Cost.** Time: O(n). Every index is pushed once and popped once.. Space: O(k)

**Watch out for.** Storing values instead of indices, which leaves you unable to tell when the front has left the window.

[Full page](../patterns/monotonic-queue.md)

### 33. Simulation

**Core idea.** Some problems have no shortcut. Model the state, apply the rules exactly as written, and step forward until it stops.

**Recognize it when.** "simulate", "repeat until", "each second", "the robot moves", "the game ends when", "spiral", "in order, until nothing changes".

**Cost.** Time: O(steps × work per step). The constraints tell you what fits.. Space: O(state)

**Watch out for.** Looking for a clever formula when the constraints clearly permit brute force. Check the limits first.

[Full page](../patterns/simulation.md)

### 34. Linear Sorting Algorithms

**Core idea.** No comparison sort can beat O(n log n). If you stop comparing and start using the values themselves as positions, you can sort in O(n).

**Recognize it when.** "values are between 0 and", "sort in linear time", "lowercase English letters", "colors", "ages", "the numbers are small".

**Cost.** Counting sort: O(n + k) time, O(k) space, where k is the size of the value range. Radix sort: O(d × (n + k)) time, where d is the number of digits. Bucket sort: O(n) on average, O(n²) in the worst case when everything lands in one bucket

**Watch out for.** Using counting sort on a huge value range. A range of a billion needs a billion slots, whatever the length of the array.

[Full page](../patterns/linear-sorting-algorithms.md)

### 35. Meet in the Middle

**Core idea.** Split the input in half, enumerate each half on its own, then match the two halves against each other. The cost falls from 2^n to about 2^(n/2).

**Recognize it when.** "n is at most 40", "subset sum", "closest to the target", "choose any number of items", together with values far too large to index.

**Cost.** Time: O(2^(n/2) × n/2) to enumerate, plus a log factor if the join needs sorting. Space: O(2^(n/2))

**Watch out for.** Using it when it is not needed. Below about n equal to 25, plain [backtracking](../patterns/backtracking.md) with pruning is simpler and fast enough. Above about 45, even the halves are too big.

[Full page](../patterns/meet-in-the-middle.md)

### 36. Mo's Algorithm

**Core idea.** Answer many range queries by reordering them, so each query's range is close to the last one and the window only has to shuffle a little between answers.

**Recognize it when.** "q queries", "given all queries in advance", "distinct elements in a range", "no updates", with n and q both large.

**Cost.** Time: O((n + q) × √n). Space: O(n) for the aggregate, plus O(q) for the sorted query order

**Watch out for.** Using it when the queries are online, meaning each answer must be produced before the next query is revealed. Mo's requires reordering, so it cannot be used.

[Full page](../patterns/mos-algorithm.md)

### 37. Serialize and Deserialize

**Core idea.** Flatten a structure into a string, and rebuild it from that string. The format has to record enough shape that the rebuild is unambiguous.

**Recognize it when.** "serialize", "encode and decode", "reconstruct the tree", "from preorder and inorder", "codec", "save and restore".

**Cost.** Time: O(n) in each direction. Space: O(n) for the string, plus O(h) for the recursion

**Watch out for.** Leaving out the null markers, which makes the shape ambiguous and the rebuild wrong.

[Full page](../patterns/serialize-and-deserialize.md)

### 38. Clone

**Core idea.** Copy a structure node by node, keeping a map from each original to its copy, so shared links and cycles are reproduced instead of chased forever.

**Recognize it when.** "deep copy", "clone", "random pointer", "independent copy", "modifications must not affect".

**Cost.** Time: O(n + e), where n is the number of nodes and e the number of links. Space: O(n) for the map

**Watch out for.** Recording the clone in the map after recursing rather than before, which loops forever on a cycle.

[Full page](../patterns/clone.md)

### 39. Articulation Points and Bridges

**Core idea.** Find the single points of failure in a network: the one node, or the one link, whose removal splits the graph in two.

**Recognize it when.** "critical connection", "single point of failure", "cut vertex", "bridge", "would disconnect", "minimum days to disconnect".

**Cost.** Time: O(V + E). Space: O(V) for the arrays and the recursion stack

**Watch out for.** Mixing up the two comparisons. Bridges use strictly greater than. Articulation points use greater than or equal.

[Full page](../patterns/articulation-points-and-bridges.md)

### 40. Segment Tree

**Core idea.** A tree over the array where each node stores the aggregate of a range, so both queries and updates cost O(log n) instead of one being O(n).

**Recognize it when.** "range sum query mutable", "update and query", "range minimum", "add to a range", "q queries with updates".

**Cost.** Build: O(n). Query: O(log n). Update: O(log n). Space: O(4n) for the array-backed version

**Watch out for.** Sizing the array as 2n. Use 4n. The tree is not always perfectly balanced and 2n overflows on some inputs.

[Full page](../patterns/segment-tree.md)

### 41. Binary Indexed Tree

**Core idea.** A prefix sum array you can update. Twenty lines of code give you O(log n) updates and O(log n) prefix queries.

**Recognize it when.** "range sum with updates", "count of smaller elements", "inversions", "cumulative frequency", "how many before this one".

**Cost.** Update: O(log n). Prefix query: O(log n). Build: O(n log n) by repeated update, or O(n) with a direct construction. Space: O(n)

**Watch out for.** Using index 0. The structure is 1-indexed, because `i & -i` is zero when i is zero and the update loop would never move. Shift every index up by one.

[Full page](../patterns/binary-indexed-tree.md)

## If you only have ten minutes

These ten cover most of what gets asked in a standard coding round. Read their cues, in this order:

[Two Pointers](../patterns/two-pointers.md), [Sliding Window](../patterns/sliding-window.md), [Hash Maps](../patterns/hash-maps.md), [Tree Level Order Traversal](../patterns/tree-level-order-traversal.md), [Tree Depth First Search](../patterns/tree-depth-first-search.md), [Graphs](../patterns/graphs.md), [Modified Binary Search](../patterns/modified-binary-search.md), [Top K Elements](../patterns/top-k-elements.md), [Backtracking](../patterns/backtracking.md), [Monotonic Stack](../patterns/monotonic-stack.md).

## Go deeper

- Work backwards from the problem instead: [how to recognize the pattern in sixty seconds](recognize-the-pattern.md)
- Every pattern in full: [patterns/](../patterns/)
- Worked solutions in six languages: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
