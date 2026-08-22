# Coding Interview Flashcards

103 question and answer cards over the patterns, the structures, and the traps. Cover the answer, say
yours out loud, then check. Each section links to the page that explains it in full.

The cards worth most are the ones that ask **which pattern** and **why**, not the ones that ask what a
heap is. Those are the questions an interview actually puts to you.

## Reading the constraints

See [what complexity passes](what-complexity-passes.md).

**n is up to 10^5. Which complexities are allowed?**
O(n), O(n log n), and anything smaller. O(n²) is about 10^10 operations, which will not finish.

**n is up to 20. What does that tell you?**
The intended answer is probably exponential. Enumerate subsets or backtrack, and stop looking for a
formula.

**n is up to 40 and the values are huge. What now?**
[Meet in the middle](../patterns/meet-in-the-middle.md). Too large for 2^n, and the values are too
large for a knapsack table.

**The constraint says values are between 1 and n. Which pattern?**
[Cyclic sort](../patterns/cyclic-sort.md). Each value names an index, so no comparison sorting is
needed.

**The constraint says "in O(1) extra space" on an array question. What does it rule out?**
A hash map and a visited array. Look at [two pointers](../patterns/two-pointers.md),
[cyclic sort](../patterns/cyclic-sort.md), or [XOR](../patterns/bitwise-xor.md).

**The problem says the input is sorted. Why does that matter?**
It is never mentioned by accident. It enables [binary search](../patterns/modified-binary-search.md)
or converging [two pointers](../patterns/two-pointers.md).

**n is 10^9 and the question asks for the smallest value that works. Which technique?**
Binary search on the answer. Search the range of candidate answers, not the input.

**What makes binary search on the answer valid?**
The test must be monotonic: once a candidate works, every larger one must also work.

**What is a pseudo-polynomial running time?**
One that depends on the size of a number in the input rather than the count of inputs. The
[knapsack](../patterns/0-1-knapsack.md) table is O(n × C), so a capacity of 10^9 breaks it.

**Why is O(n log n) usually fine at n = 10^5 when O(n²) is not?**
The log factor multiplies the work by about 17. The square multiplies it by 100,000.

## Arrays and strings

See [Two Pointers](../patterns/two-pointers.md), [Sliding Window](../patterns/sliding-window.md), [Prefix Sum](../patterns/prefix-sum.md).

**Sliding window or prefix sum?**
A window needs all-positive values and finds one best subarray. Prefix sums handle negatives and count
many subarrays.

**Why does a sliding window fail when values can be negative?**
The window shrinks because the sum is too large and grows because it is too small. With negatives,
adding an element can decrease the sum, so neither direction is reliable.

**When shrinking a window, why a `while` and not an `if`?**
A window can lose several leading elements and stay valid. An `if` stops shrinking too early and
misses the shortest answer.

**How do you count subarrays with exactly K distinct values?**
"At most K" minus "at most K minus 1". Each half is a standard window.

**What is the window length from `start` to `end` inclusive?**
`end - start + 1`.

**In "subarray sum equals k", what is the one line most people forget?**
Seeding the map with a running total of 0 occurring once. Without it, every subarray starting at index
0 is missed.

**Why do opposite-end two pointers need a sorted array?**
The direction rule depends on it. Moving left rightwards must increase the sum and moving right
leftwards must decrease it, which is only guaranteed under sorting.

**Container With Most Water is not sorted. Why do two pointers still work?**
The area is limited by the shorter line. Moving the taller pointer inward loses width and cannot gain
height, so it can never improve the answer.

**How do you avoid duplicate triplets in 3Sum?**
Sort first, then skip over values equal to the one you just used, at every level.

**What is the Dutch national flag partition?**
A one-pass sort of three distinct values using three pointers. After swapping with the high pointer,
do not advance the middle one, because the incoming value has not been examined.

**Why is building a string by concatenation in a loop O(n²)?**
Strings are immutable, so each concatenation copies everything so far. Use a builder or collect the
pieces and join once.

**When is a fixed array better than a hash map for counting?**
When the value range is small and known, as with the 26 lowercase letters. It is faster and easier to
compare.

## Linked lists

See [Fast and Slow Pointers](../patterns/fast-and-slow-pointers.md), [In-place Reversal](../patterns/in-place-reversal-of-a-linked-list.md).

**How do you detect a cycle in O(1) space?**
Two pointers, one moving one step and one moving two. If they meet, there is a cycle.

**Why must the fast pointer eventually meet the slow one inside a cycle?**
Once both are in the loop, the gap closes by exactly one node per round, so they cannot pass without
meeting.

**After the two pointers meet, how do you find the start of the cycle?**
Reset one pointer to the head and move both one step at a time. They meet at the first node of the
cycle.

**How do you find the middle of a linked list in one pass?**
Move slow one step and fast two. When fast reaches the end, slow is at the middle.

**Which three variables does an in-place reversal need?**
Previous, current, and a temporary holding the rest of the list, saved before you overwrite the
pointer that leads to it.

**What is the point of a dummy node in front of the head?**
It removes the special case for operations that touch the first node, which is where most linked list
bugs live.

**After reversing a list, what must the old head's next pointer be?**
Null. Leaving it set creates a cycle.

**How do you check whether a linked list is a palindrome in O(1) space?**
Find the middle, reverse the second half in place, compare the halves, then restore the list.

## Stacks, queues, and monotonic structures

See [Stacks](../patterns/stacks.md), [Monotonic Stack](../patterns/monotonic-stack.md), [Monotonic Queue](../patterns/monotonic-queue.md).

**Which questions call for a stack?**
Anything about nesting, matching, undo, or "the most recent thing not yet handled".

**Why does counting brackets fail on `([)]`?**
Counting ignores order. Validity depends on which bracket is still open, which is the top of the
stack.

**Which two checks does bracket validation need?**
An empty-stack check before popping, which catches `)))`, and a final emptiness check, which catches
`(((`.

**What does a monotonic stack compute as a side effect?**
Whenever a new value pops an old one, the new value is the popped one's next greater, or next smaller,
element.

**Why is a monotonic stack O(n) despite the nested loop?**
Each index is pushed once and popped at most once, so the total number of pops is n.

**Why store indices rather than values in a monotonic stack?**
You usually need a width or a distance, which requires knowing where the popped element was.

**Monotonic stack or monotonic queue?**
A stack answers next greater or smaller for each element. A queue answers the extreme inside a moving
window, because it can also drop items from the front.

**Why not use a heap for sliding window maximum?**
A heap cannot remove an element that has left the window without an O(n) search, unless you add lazy
deletion. A deque drops it from the front in O(1).

## Trees

See [Level Order](../patterns/tree-level-order-traversal.md), [Depth First Search](../patterns/tree-depth-first-search.md).

**Level order or depth first?**
Per level, or shortest depth, means level order. Per path, or ancestor to descendant, means depth
first.

**What is the one line that separates one level from the next?**
Reading the queue size before the inner loop, because the inner loop pushes children onto the same
queue while it runs.

**Why is minimum depth wrong if you treat a null child as a leaf?**
A node with one child is not a leaf. Counting it as one returns a depth that is too small.

**Why is an array a bad queue?**
Removing from the front is O(n), which quietly makes the whole traversal quadratic.

**In-order traversal of a binary search tree produces what?**
The values in sorted order, which is why so many BST problems are one in-order walk.

**What are the two shapes of a tree depth first solution?**
Returning a value up to the parent, and carrying state down to the children. Path problems use the
second and need an undo step.

**What is the undo step in a path problem?**
Removing the current node from the path after recursing, so it does not appear in a sibling branch.

**Why append a copy of the path rather than the path itself?**
Every entry would otherwise point at the same list, which is empty by the time you return.

**Why is comparing a node only against its parent wrong for BST validation?**
The rule covers every descendant. A node deep in the left subtree must still be smaller than the root.
Pass an allowed range down instead.

**What is the space cost of a recursive tree traversal?**
O(h) for the call stack, which is O(log n) on a balanced tree and O(n) on a tree shaped like a list.

**How do you serialize a binary tree unambiguously?**
A pre-order walk with an explicit marker for every null child. Values alone do not record the shape.

**Which tree needs no null markers when serialized?**
A binary search tree, because the ordering rule already says where each value belongs.

## Graphs

See [Graphs](../patterns/graphs.md), [Topological Sort](../patterns/topological-sort.md), [Union Find](../patterns/union-find.md), [Island Traversal](../patterns/island-matrix-traversal.md).

**BFS or DFS for the fewest steps?**
BFS. It explores in rings of increasing distance, so the first arrival is the shortest route.

**When should a node be marked visited?**
When you push it, not when you pop it. Marking on pop lets the same node enter the queue many times.

**Why does BFS not give the shortest path on a weighted graph?**
It counts hops. With different edge costs, the fewest hops and the cheapest route are different
answers. Use Dijkstra.

**What does it mean when a topological sort places fewer nodes than the graph has?**
The remainder form a cycle, so no valid order exists.

**"To take course A you need course B." Which way does the edge point?**
From B to A. Getting this backwards produces a wrong answer rather than a crash.

**Why does building an undirected graph break a topological sort?**
Every in-degree becomes at least one, so the queue starts empty and nothing is ever scheduled.

**Union Find or a graph traversal?**
If edges arrive over time and you must answer as they arrive, union find. If the graph is fixed, one
traversal is simpler.

**What do path compression and union by size buy you?**
They keep the trees flat, so each operation is effectively O(1). Without them it degrades to O(n) per
operation.

**In union find, what does it mean when `union` returns false?**
The two nodes were already connected, so this edge closes a cycle. That is the whole answer to the
redundant connection problem.

**How do you count islands in a grid?**
Scan for an unvisited land cell, run one search that consumes the whole island, and count the
searches.

**When is union find better than a search for grid connectivity?**
When land is added over time, so re-running the search after each addition would be too slow.

**What is multi-source BFS?**
Pushing every starting point into the queue before the loop begins. It is how spreading problems like
rotting oranges are solved.

## Heaps, sorting, and selection

See [Top K Elements](../patterns/top-k-elements.md), [Two Heaps](../patterns/two-heaps.md), [K-way Merge](../patterns/k-way-merge.md).

**To keep the K largest values, which heap do you use?**
A min-heap of size K. Its top is the weakest winner, which is exactly what to evict.

**What does a heap of size K buy over sorting?**
O(n log k) instead of O(n log n), and O(k) space instead of O(n).

**How do you get the median of a stream?**
Two heaps: a max-heap for the smaller half and a min-heap for the larger half, rebalanced so the sizes
differ by at most one.

**What breaks a heap solution to sliding window median?**
Removing the element that leaves the window. Heaps cannot remove from the middle, so you need lazy
deletion or an ordered set.

**What does each entry of a K-way merge heap have to carry?**
Which list the element came from, so you can push its successor after popping it.

**What is the cost of building a heap from n items?**
O(n), not O(n log n). Repeated insertion is O(n log n), but heapify is linear.

**Which built-in sorts are stable?**
Timsort, used by Python and by Java for objects. Java's primitive sort is a dual-pivot quicksort,
which is not stable.

**When can you sort in O(n)?**
When the values are integers in a small known range. Counting, radix, and bucket sort all need that
constraint.

**Why does radix sort require a stable inner sort?**
Each pass must preserve the order produced by the previous digit, or the earlier work is destroyed.

**What is quickselect for?**
Finding the Kth element in O(n) average time, when you do not need the whole top K.

## Dynamic programming

See [0/1 Knapsack](../patterns/0-1-knapsack.md), [Fibonacci Numbers](../patterns/fibonacci-numbers.md), [Palindromic Subsequence](../patterns/palindromic-subsequence.md).

**What are the two conditions for dynamic programming?**
Overlapping subproblems, and an optimal answer built from optimal answers to smaller problems.

**Greedy or dynamic programming?**
If an early choice can block a better later one, greedy is wrong. Try to build that counterexample
before committing.

**Coins of 1, 3, and 4, target 6. Why does greedy fail?**
Greedy takes 4 and then two 1s, which is three coins. Two 3s is two coins.

**In the 1D knapsack, which way does the capacity loop run?**
Backwards for 0/1, so an item cannot be used twice. Forwards gives the unbounded version.

**What is the state in a palindrome problem?**
A range from i to j, not a single position, because a palindrome is defined by both of its ends.

**Why must a range table be filled by increasing length?**
A range of length 4 needs the answer for the range of length 2 inside it, which plain index order has
not computed yet.

**Subsequence or substring?**
A subsequence may skip characters. A substring must be contiguous. The recurrences differ.

**What is the safest route from a recursion to a fast solution?**
Write the plain recursion, name the changing arguments as the state, add a cache, and only then turn
it into a table.

**When does a memoized solution give a wrong answer rather than a slow one?**
When the cache key does not capture everything the answer depends on, so two different situations
share an entry.

**What is the O(n²) time and O(1) space method for longest palindromic substring?**
Expand around each center, treating both single characters and gaps between characters as centers.

## Recursion and backtracking

See [Backtracking](../patterns/backtracking.md), [Subsets](../patterns/subsets.md).

**What are the four parts of a backtracking function?**
A base case that records a finished answer, a loop over the choices, a validity test, and the make,
recurse, undo sequence.

**What happens if you forget the undo?**
State from a failed branch leaks into every later branch, so valid answers come back as failures.

**Backtracking or dynamic programming?**
If the output is a list of every valid answer, backtracking. If it is a count or one best value,
dynamic programming.

**How many subsets does a set of n elements have, and why?**
2^n, because each element is independently in or out.

**How do you avoid duplicate subsets when the input has duplicates?**
Sort first, then when the current element equals the previous one, extend only the subsets created in
the previous round.

**What is the difference between subsets and permutations?**
Order does not matter in a subset, so [1,2] and [2,1] are the same. In a permutation they are
different, and the count is n factorial.

**In word search, why is "visited" temporary?**
A cell may not be reused within one path, but a different path may use it. That is why it is unmarked
on the way out.

**What is the practical recursion depth limit in Python?**
About 1,000 frames by default, which a deep tree or a large grid will exceed.

## Language and implementation

See [Python, Java, and JavaScript side by side](python-vs-java-vs-javascript-idioms.md).

**What does `arr.sort()` do in JavaScript with no comparator?**
Sorts as strings, so `[10, 9, 1]` becomes `[1, 10, 9]`. Always pass a comparator.

**How do you make a max-heap in Python?**
Push negated values into `heapq`, and negate again on the way out.

**Which structure does JavaScript lack that costs the most in interviews?**
A heap. It also has no ordered set. Say so out loud rather than pretending otherwise.

**What is `-7 % 3` in Python, Java, and JavaScript?**
2 in Python, and -1 in the other two. Use `((x % k) + k) % k` when grouping by remainder.

**Why write `low + (high - low) / 2` instead of `(low + high) / 2`?**
The sum can overflow a 32-bit integer on a large array, which is the oldest binary search bug there
is.

**What does `Arrays.binarySearch` return when the value is absent?**
`-(insertion point) - 1`, not -1.

**What is wrong with `[[0] * c] * r` in Python?**
It creates r references to one row, so changing one row changes them all.

**Why can comparing tuples in a Python heap throw an exception?**
If the first fields tie, it compares the next field, which may be an object with no ordering. Add an
index as a tie-breaker.

## The interview itself

See [how to talk through a coding interview](interview-communication.md).

**What is the first thing to do after reading the problem?**
Restate it in one sentence and ask about the input: size, sorted, duplicates, negatives, empty.

**Why state the brute force even when you know the fast answer?**
It proves you understood the question, it gives you a baseline to improve, and it is a working answer
if you run out of time.

**Which sentence carries the most weight in the whole round?**
Reading the constraints out loud, ruling out the brute force with arithmetic, and naming the pattern.

**What should you say before writing code?**
The invariant. A candidate who can state it almost always writes correct code.

**Who should find your bug?**
You should. Test your own code out loud on the smallest input, then on the edge cases you named
earlier.

**You have seen the problem before. What do you say?**
Say so, then explain the approach and why it works. Pretending to derive a memorized answer is
obvious.

## Go deeper

- [All 41 patterns, on one page](all-41-patterns.md)
- [How to recognize the pattern in sixty seconds](recognize-the-pattern.md)
- Worked solutions in six languages: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
