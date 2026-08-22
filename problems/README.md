# Every Problem in the Course, Mapped to Its Pattern

All 302 problems in [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview), grouped by the pattern that solves them, with the difficulty the course assigns.

**Use it backwards.** Reading a problem under its pattern heading teaches you nothing, because the answer is the heading. Pick a problem, cover the section title, and say which pattern you would reach for and which words in the title made you say it. Then check. That gap, between the cue and your answer, is the thing an interview actually measures.

**22 problems have a walkthrough**, linked from the tables below. Each one gives the problem in our own words, the argument for why it belongs to its pattern, the approach and its invariant, the complexity, and the edge cases to say out loud. It stops short of the code. The full worked solutions, in six languages with runnable tests, are in the course.

## Contents

| Pattern | Problems | Pattern page |
|---|---|---|
| [Warmup](#warmup) | 8 | no single pattern, that is the point |
| [Two Pointers](#two-pointers) | 10 | [pattern page](../patterns/two-pointers.md) |
| [Fast and Slow Pointers](#fast-and-slow-pointers) | 7 | [pattern page](../patterns/fast-and-slow-pointers.md) |
| [Sliding Window](#sliding-window) | 12 | [pattern page](../patterns/sliding-window.md) |
| [Merge Intervals](#merge-intervals) | 7 | [pattern page](../patterns/merge-intervals.md) |
| [Cyclic Sort](#cyclic-sort) | 8 | [pattern page](../patterns/cyclic-sort.md) |
| [In-place Reversal of a Linked List](#in-place-reversal-of-a-linked-list) | 5 | [pattern page](../patterns/in-place-reversal-of-a-linked-list.md) |
| [Stacks](#stacks) | 6 | [pattern page](../patterns/stacks.md) |
| [Monotonic Stack](#monotonic-stack) | 7 | [pattern page](../patterns/monotonic-stack.md) |
| [Hash Maps](#hash-maps) | 5 | [pattern page](../patterns/hash-maps.md) |
| [Tree Level Order Traversal](#tree-level-order-traversal) | 14 | [pattern page](../patterns/tree-level-order-traversal.md) |
| [Tree Depth First Search](#tree-depth-first-search) | 7 | [pattern page](../patterns/tree-depth-first-search.md) |
| [Graphs](#graphs) | 5 | [pattern page](../patterns/graphs.md) |
| [Island (Matrix Traversal)](#island-matrix-traversal) | 7 | [pattern page](../patterns/island-matrix-traversal.md) |
| [Two Heaps](#two-heaps) | 4 | [pattern page](../patterns/two-heaps.md) |
| [Subsets](#subsets) | 9 | [pattern page](../patterns/subsets.md) |
| [Modified Binary Search](#modified-binary-search) | 10 | [pattern page](../patterns/modified-binary-search.md) |
| [Bitwise XOR](#bitwise-xor) | 4 | [pattern page](../patterns/bitwise-xor.md) |
| [Top K Elements](#top-k-elements) | 14 | [pattern page](../patterns/top-k-elements.md) |
| [K-way Merge](#k-way-merge) | 5 | [pattern page](../patterns/k-way-merge.md) |
| [Greedy Algorithms](#greedy-algorithms) | 6 | [pattern page](../patterns/greedy-algorithms.md) |
| [0/1 Knapsack](#01-knapsack) | 6 | [pattern page](../patterns/0-1-knapsack.md) |
| [Fibonacci Numbers](#fibonacci-numbers) | 6 | [pattern page](../patterns/fibonacci-numbers.md) |
| [Palindromic Subsequence](#palindromic-subsequence) | 5 | [pattern page](../patterns/palindromic-subsequence.md) |
| [Backtracking](#backtracking) | 5 | [pattern page](../patterns/backtracking.md) |
| [Trie](#trie) | 5 | [pattern page](../patterns/trie.md) |
| [Topological Sort](#topological-sort) | 7 | [pattern page](../patterns/topological-sort.md) |
| [Union Find](#union-find) | 4 | [pattern page](../patterns/union-find.md) |
| [Ordered Set](#ordered-set) | 4 | [pattern page](../patterns/ordered-set.md) |
| [Prefix Sum](#prefix-sum) | 7 | [pattern page](../patterns/prefix-sum.md) |
| [Multi-threaded](#multi-threaded) | 3 | [pattern page](../patterns/multi-threaded.md) |
| [Counting](#counting) | 6 | [pattern page](../patterns/counting.md) |
| [Monotonic Queue](#monotonic-queue) | 5 | [pattern page](../patterns/monotonic-queue.md) |
| [Simulation](#simulation) | 6 | [pattern page](../patterns/simulation.md) |
| [Linear Sorting Algorithms](#linear-sorting-algorithms) | 6 | [pattern page](../patterns/linear-sorting-algorithms.md) |
| [Meet in the Middle](#meet-in-the-middle) | 5 | [pattern page](../patterns/meet-in-the-middle.md) |
| [Mo's Algorithm](#mos-algorithm) | 4 | [pattern page](../patterns/mos-algorithm.md) |
| [Serialize and Deserialize](#serialize-and-deserialize) | 5 | [pattern page](../patterns/serialize-and-deserialize.md) |
| [Clone](#clone) | 4 | [pattern page](../patterns/clone.md) |
| [Articulation Points and Bridges](#articulation-points-and-bridges) | 4 | [pattern page](../patterns/articulation-points-and-bridges.md) |
| [Segment Tree](#segment-tree) | 4 | [pattern page](../patterns/segment-tree.md) |
| [Binary Indexed Tree](#binary-indexed-tree) | 6 | [pattern page](../patterns/binary-indexed-tree.md) |
| [Miscellaneous](#miscellaneous) | 1 | mixed |
| [Test Your Knowledge](#test-your-knowledge) | 34 | unlabelled, deliberately |
| **Total** | **302** | |

## Warmup

These come before any pattern is taught, so nothing tells you what to use. Treat them as a diagnostic: the ones you stall on point at the chapter to read first.

| Problem | Difficulty |
|---|---|
| Contains Duplicate | Easy |
| Pangram | Easy |
| Reverse Vowels | Easy |
| Valid Palindrome | Easy |
| Valid Anagram | Easy |
| Shortest Word Distance | Easy |
| Number of Good Pairs | Easy |
| Sqrt | Medium |

## Two Pointers

10 problems. Pattern page: [Two Pointers](../patterns/two-pointers.md).

| Problem | Difficulty |
|---|---|
| Pair with Target Sum | Easy |
| Find Non-Duplicate Number Instances | Easy |
| Squaring a Sorted Array | Easy |
| Triplet Sum to Zero | Medium |
| Triplet Sum Close to Target | Medium |
| Triplets with Smaller Sum | Medium |
| Dutch National Flag Problem | Medium |
| Quadruple Sum to Target * | Medium |
| Comparing Strings containing Backspaces * | Medium |
| Minimum Window Sort * | Medium |

## Fast and Slow Pointers

7 problems. Pattern page: [Fast and Slow Pointers](../patterns/fast-and-slow-pointers.md).

| Problem | Difficulty |
|---|---|
| [LinkedList Cycle](linkedlist-cycle.md) | Easy |
| Middle of the LinkedList | Easy |
| Start of LinkedList Cycle | Medium |
| Happy Number | Medium |
| Palindrome LinkedList * | Medium |
| Rearrange a LinkedList * | Medium |
| Cycle in a Circular Array * | Hard |

## Sliding Window

12 problems. Pattern page: [Sliding Window](../patterns/sliding-window.md).

| Problem | Difficulty |
|---|---|
| Maximum Sum Subarray of Size K | Easy |
| Smallest Subarray With a Greater Sum | Easy |
| Longest Substring with K Distinct Characters | Medium |
| Fruits into Baskets | Medium |
| Longest Substring with Same Letters after Replacement | Hard |
| Longest Subarray with Ones after Replacement | Hard |
| Permutation in a String * | Hard |
| String Anagrams * | Hard |
| [Smallest Window containing Substring](smallest-window-containing-substring.md) * | Hard |
| Words Concatenation * | Hard |
| Counting Subarrays with Product Less than a Target * | Hard |
| Subarrays with Product Less than a Target * | Hard |

## Merge Intervals

7 problems. Pattern page: [Merge Intervals](../patterns/merge-intervals.md).

| Problem | Difficulty |
|---|---|
| [Merge Intervals](merge-intervals.md) | Medium |
| Insert Interval | Medium |
| Intervals Intersection | Medium |
| Conflicting Appointments | Medium |
| [Minimum Meeting Rooms](minimum-meeting-rooms.md) * | Hard |
| Maximum CPU Load * | Hard |
| Employee Free Time * | Hard |

## Cyclic Sort

8 problems. Pattern page: [Cyclic Sort](../patterns/cyclic-sort.md).

| Problem | Difficulty |
|---|---|
| Cyclic Sort | Easy |
| [Find the Missing Number](find-the-missing-number.md) | Easy |
| Find all Missing Numbers | Easy |
| Find the Duplicate Number | Easy |
| Find all Duplicate Numbers | Easy |
| Find the Corrupt Pair * | Easy |
| Find the Smallest Missing Positive Number * | Medium |
| Find the First K Missing Positive Numbers * | Hard |

## In-place Reversal of a Linked List

5 problems. Pattern page: [In-place Reversal of a Linked List](../patterns/in-place-reversal-of-a-linked-list.md).

| Problem | Difficulty |
|---|---|
| [Reverse a LinkedList](reverse-a-linked-list.md) | Easy |
| Reverse a Sub-list | Medium |
| Reverse every K-element Sub-list | Medium |
| Reverse alternating K-element Sub-list * | Medium |
| Rotate a LinkedList * | Medium |

## Stacks

6 problems. Pattern page: [Stacks](../patterns/stacks.md).

| Problem | Difficulty |
|---|---|
| [Balanced Parentheses](balanced-parentheses.md) | Easy |
| Reverse a String | Easy |
| Decimal to Binary Conversion | Medium |
| Next Greater Element | Easy |
| Sorting a Stack | Easy |
| Simplify Path | Medium |

## Monotonic Stack

7 problems. Pattern page: [Monotonic Stack](../patterns/monotonic-stack.md).

| Problem | Difficulty | Also solvable with |
|---|---|---|
| Remove Nodes From Linked List | Easy |  |
| Remove All Adjacent Duplicates In String | Easy |  |
| Next Greater Element | Easy |  |
| [Daily Temperatures](daily-temperatures.md) | Easy |  |
| Remove All Adjacent Duplicates in String II | Medium |  |
| Sum of Subarray Minimums | Medium | Monotonic Queue |
| Remove K Digits | Hard |  |

## Hash Maps

5 problems. Pattern page: [Hash Maps](../patterns/hash-maps.md).

| Problem | Difficulty |
|---|---|
| First Non-repeating Character | Easy |
| Largest Unique Number | Easy |
| Maximum Number of Balloons | Easy |
| Longest Palindrome | Easy |
| Ransom Note | Easy |

## Tree Level Order Traversal

14 problems. Pattern page: [Tree Level Order Traversal](../patterns/tree-level-order-traversal.md).

| Problem | Difficulty |
|---|---|
| [Binary Tree Level Order Traversal](binary-tree-level-order-traversal.md) | Easy |
| Reverse Level Order Traversal | Easy |
| Zigzag Traversal | Medium |
| Level Averages in a Binary Tree | Easy |
| Find Largest Value in Each Tree Row | Medium |
| Maximum Level Sum of a Binary Tree | Medium |
| Even Odd Tree | Medium |
| Minimum Depth of a Binary Tree | Easy |
| Level Order Successor | Easy |
| Connect Level Order Siblings | Medium |
| Maximum Width of Binary Tree | Medium |
| N-ary Tree Level Order Traversal | Hard |
| Connect All Level Order Siblings * | Medium |
| Right View of a Binary Tree * | Easy |

## Tree Depth First Search

7 problems. Pattern page: [Tree Depth First Search](../patterns/tree-depth-first-search.md).

| Problem | Difficulty |
|---|---|
| Binary Tree Path Sum | Easy |
| All Paths for a Sum | Medium |
| Sum of Path Numbers | Medium |
| Path With Given Sequence | Medium |
| Count Paths for a Sum | Medium |
| Tree Diameter * | Medium |
| Path with Maximum Sum * | Hard |

## Graphs

5 problems. Pattern page: [Graphs](../patterns/graphs.md).

| Problem | Difficulty | Also solvable with |
|---|---|---|
| Find if Path Exists in Graph | Easy |  |
| Number of Provinces | Medium | Union Find |
| Find Eventual Safe States | Medium |  |
| Minimum Number of Vertices to Reach All Nodes | Medium |  |
| Bus Routes | Hard |  |

## Island (Matrix Traversal)

7 problems. Pattern page: [Island (Matrix Traversal)](../patterns/island-matrix-traversal.md).

| Problem | Difficulty |
|---|---|
| [Number of Islands](number-of-islands.md) | Easy |
| Biggest Island | Easy |
| Flood Fill | Easy |
| Number of Closed Islands | Easy |
| Problem Challenge 1 * | Easy |
| Problem Challenge 2 * | Medium |
| Problem Challenge 3 * | Medium |

## Two Heaps

4 problems. Pattern page: [Two Heaps](../patterns/two-heaps.md).

| Problem | Difficulty |
|---|---|
| [Find the Median of a Number Stream](find-the-median-of-a-number-stream.md) | Medium |
| Sliding Window Median | Hard |
| Maximize Capital | Hard |
| Next Interval * | Hard |

## Subsets

9 problems. Pattern page: [Subsets](../patterns/subsets.md).

| Problem | Difficulty |
|---|---|
| [Subsets](subsets.md) | Easy |
| Subsets With Duplicates | Easy |
| Permutations | Medium |
| String Permutations by changing case | Medium |
| [Balanced Parentheses](balanced-parentheses.md) | Hard |
| Unique Generalized Abbreviations | Hard |
| Evaluate Expression * | Hard |
| Structurally Unique Binary Search Trees * | Hard |
| Count of Structurally Unique Binary Search Trees * | Hard |

## Modified Binary Search

10 problems. Pattern page: [Modified Binary Search](../patterns/modified-binary-search.md).

| Problem | Difficulty |
|---|---|
| Order-agnostic Binary Search | Easy |
| Ceiling of a Number | Medium |
| Next Letter | Medium |
| Number Range | Medium |
| Search in a Sorted Infinite Array | Medium |
| Minimum Difference Element | Medium |
| Bitonic Array Maximum | Easy |
| Search Bitonic Array * | Medium |
| [Search in Rotated Array](search-in-rotated-array.md) * | Medium |
| Rotation Count * | Medium |

## Bitwise XOR

4 problems. Pattern page: [Bitwise XOR](../patterns/bitwise-xor.md).

| Problem | Difficulty |
|---|---|
| Single Number | Easy |
| Two Single Numbers | Medium |
| Complement of Base 10 Number | Medium |
| Flip and Invert an Image * | Hard |

## Top K Elements

14 problems. Pattern page: [Top K Elements](../patterns/top-k-elements.md).

| Problem | Difficulty |
|---|---|
| Top 'K' Numbers | Easy |
| Kth Smallest Number | Easy |
| 'K' Closest Points to the Origin | Easy |
| Connect Ropes | Easy |
| Top 'K' Frequent Numbers | Medium |
| Frequency Sort | Medium |
| Kth Largest Number in a Stream | Medium |
| 'K' Closest Numbers | Medium |
| Maximum Distinct Elements | Medium |
| Sum of Elements | Medium |
| Rearrange String | Hard |
| Rearrange String K Distance Apart * | Hard |
| Scheduling Tasks * | Hard |
| Frequency Stack * | Hard |

## K-way Merge

5 problems. Pattern page: [K-way Merge](../patterns/k-way-merge.md).

| Problem | Difficulty |
|---|---|
| [Merge K Sorted Lists](merge-k-sorted-lists.md) | Medium |
| Kth Smallest Number in M Sorted Lists | Medium |
| Kth Smallest Number in a Sorted Matrix | Hard |
| Smallest Number Range | Hard |
| K Pairs with Largest Sums * | Hard |

## Greedy Algorithms

6 problems. Pattern page: [Greedy Algorithms](../patterns/greedy-algorithms.md).

| Problem | Difficulty |
|---|---|
| Valid Palindrome II | Easy |
| Maximum Length of Pair Chain | Medium |
| Minimum Add to Make Parentheses Valid | Medium |
| Remove Duplicate Letters | Medium |
| Largest Palindromic Number | Medium |
| Removing Minimum and Maximum From Array | Medium |

## 0/1 Knapsack

6 problems. Pattern page: [0/1 Knapsack](../patterns/0-1-knapsack.md).

| Problem | Difficulty |
|---|---|
| 0/1 Knapsack | Medium |
| Equal Subset Sum Partition | Medium |
| Subset Sum | Medium |
| Minimum Subset Sum Difference | Hard |
| Count of Subset Sum * | Hard |
| Target Sum * | Hard |

## Fibonacci Numbers

6 problems. Pattern page: [Fibonacci Numbers](../patterns/fibonacci-numbers.md).

| Problem | Difficulty |
|---|---|
| Fibonacci numbers | - |
| Staircase | - |
| Number factors | - |
| Minimum jumps to reach the end | - |
| Minimum jumps with fee | - |
| House thief | - |

## Palindromic Subsequence

5 problems. Pattern page: [Palindromic Subsequence](../patterns/palindromic-subsequence.md).

| Problem | Difficulty |
|---|---|
| Longest Palindromic Subsequence | - |
| Longest Palindromic Substring | - |
| Count of Palindromic Substrings | - |
| Minimum Deletions in a String to make it a Palindrome | - |
| Palindromic Partitioning | - |

## Backtracking

5 problems. Pattern page: [Backtracking](../patterns/backtracking.md).

| Problem | Difficulty |
|---|---|
| Combination Sum | Medium |
| [Word Search](word-search.md) | Medium |
| Factor Combinations | Medium |
| Split a String Into the Max Number of Unique Substrings | Medium |
| Sudoku Solver | Hard |

## Trie

5 problems. Pattern page: [Trie](../patterns/trie.md).

| Problem | Difficulty |
|---|---|
| Implement Trie (Prefix Tree) | Medium |
| Index Pairs of a String | Easy |
| Design Add and Search Words Data Structure | Medium |
| Extra Characters in a String | Medium |
| Search Suggestions System | Medium |

## Topological Sort

7 problems. Pattern page: [Topological Sort](../patterns/topological-sort.md).

| Problem | Difficulty |
|---|---|
| Topological Sort | Medium |
| [Tasks Scheduling](tasks-scheduling.md) | Medium |
| Tasks Scheduling Order | Medium |
| All Tasks Scheduling Orders | Hard |
| Alien Dictionary | Hard |
| Reconstructing a Sequence * | Hard |
| Minimum Height Trees * | Hard |

## Union Find

4 problems. Pattern page: [Union Find](../patterns/union-find.md).

| Problem | Difficulty | Also solvable with |
|---|---|---|
| Redundant Connection | Medium |  |
| Number of Provinces | Medium | Graphs |
| Is Graph Bipartite? | Medium |  |
| Path With Minimum Effort | Medium |  |

## Ordered Set

4 problems. Pattern page: [Ordered Set](../patterns/ordered-set.md).

| Problem | Difficulty |
|---|---|
| Merge Similar Items | Easy |
| 132 Pattern | Medium |
| My Calendar I | Medium |
| Longest Continuous Subarray | Medium |

## Prefix Sum

7 problems. Pattern page: [Prefix Sum](../patterns/prefix-sum.md).

| Problem | Difficulty |
|---|---|
| Find the Middle Index in Array | Easy |
| Left and Right Sum Differences | Easy |
| Maximum Size Subarray Sum Equals k | Medium |
| Binary Subarrays With Sum | Medium |
| Subarray Sums Divisible by K | Medium |
| Sum of Absolute Differences in a Sorted Array | Medium |
| [Subarray Sum Equals K](subarray-sum-equals-k.md) | Medium |

## Multi-threaded

3 problems. Pattern page: [Multi-threaded](../patterns/multi-threaded.md).

| Problem | Difficulty |
|---|---|
| Same Tree | Medium |
| Invert Binary Tree | Medium |
| Binary Search Tree Iterator | Medium |

## Counting

6 problems. Pattern page: [Counting](../patterns/counting.md).

| Problem | Difficulty |
|---|---|
| Count Elements With Maximum Frequency | Easy |
| Maximum Population Year | Easy |
| Minimum Increment to Make Array Unique | Medium |
| Minimum Number of Steps to Make Two Strings Anagram | Medium |
| Least Number of Unique Integers after K Removals | Medium |
| Subarrays with K Different Integers | Hard |

## Monotonic Queue

5 problems. Pattern page: [Monotonic Queue](../patterns/monotonic-queue.md).

| Problem | Difficulty | Also solvable with |
|---|---|---|
| Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit | Medium |  |
| Minimum Number of Coins for Fruits | Medium |  |
| Continuous Subarrays | Medium |  |
| Sum of Subarray Minimums | Medium | Monotonic Stack |
| Max Value of Equation | Hard |  |

## Simulation

6 problems. Pattern page: [Simulation](../patterns/simulation.md).

| Problem | Difficulty |
|---|---|
| Array Transformation | Easy |
| Water Bottles | Easy |
| Spiral Matrix III | Medium |
| Merge Nodes in Between Zeros | Medium |
| Validate Stack Sequences | Medium |
| Removing Stars From a String | Medium |

## Linear Sorting Algorithms

6 problems. Pattern page: [Linear Sorting Algorithms](../patterns/linear-sorting-algorithms.md).

| Problem | Difficulty |
|---|---|
| Relative Sort Array | Easy |
| Height Checker | Easy |
| Array Partition | Easy |
| Top K Frequent Words | Medium |
| Maximum Gap | Medium |
| Sort Characters By Frequency | Easy |

## Meet in the Middle

5 problems. Pattern page: [Meet in the Middle](../patterns/meet-in-the-middle.md).

| Problem | Difficulty |
|---|---|
| Subset Sum Equal to Target | Medium |
| Subsets having Sum between A and B | Hard |
| Closest Subsequence Sum | Hard |
| Split Array With Same Average | Hard |
| Partition Array Into Two Arrays to Minimize Sum Difference | Hard |

## Mo's Algorithm

4 problems. Pattern page: [Mo's Algorithm](../patterns/mos-algorithm.md).

| Problem | Difficulty |
|---|---|
| XOR Queries of a Subarray | Medium |
| Distinct elements in subarray | Medium |
| Minimum Absolute Difference Queries | Medium |
| Range Frequency Queries | Medium |

## Serialize and Deserialize

5 problems. Pattern page: [Serialize and Deserialize](../patterns/serialize-and-deserialize.md).

| Problem | Difficulty |
|---|---|
| Encode and Decode Strings | Medium |
| Serialize and Deserialize BST | Medium |
| [Serialize and Deserialize Binary Tree](serialize-and-deserialize-binary-tree.md) | Hard |
| Verify Preorder Serialization of a Binary Tree | Medium |
| Serialize and Deserialize N-ary Tree | Hard |

## Clone

4 problems. Pattern page: [Clone](../patterns/clone.md).

| Problem | Difficulty |
|---|---|
| Copy List with Random Pointer | Medium |
| Clone Binary Tree With Random Pointer | Medium |
| [Clone Graph](clone-graph.md) | Medium |
| Clone N-ary Tree | Hard |

## Articulation Points and Bridges

4 problems. Pattern page: [Articulation Points and Bridges](../patterns/articulation-points-and-bridges.md).

| Problem | Difficulty |
|---|---|
| Minimum Number of Days to Disconnect Island | Hard |
| Minimize Malware Spread | Hard |
| Minimize Malware Spread II | Hard |
| Critical Connections in a Network | Hard |

## Segment Tree

4 problems. Pattern page: [Segment Tree](../patterns/segment-tree.md).

| Problem | Difficulty |
|---|---|
| Range Minimum Query | Easy |
| Queue Reconstruction by Height | Medium |
| Count Number of Teams | Medium |
| Rectangle Area II | Hard |

## Binary Indexed Tree

6 problems. Pattern page: [Binary Indexed Tree](../patterns/binary-indexed-tree.md).

| Problem | Difficulty |
|---|---|
| Number of Longest Increasing Subsequence | Medium |
| Maximum Profitable Triplets With Increasing Prices I | Medium |
| Queries on a Permutation With Key | Medium |
| Count of Range Sum | Hard |
| Reverse Pairs | Hard |
| Minimum Number of Moves to Make Palindrome | Hard |

## Miscellaneous

The course keeps a small chapter for problems that do not sit under one pattern.

| Problem | Difficulty |
|---|---|
| Kth Smallest Number | Hard |

## Test Your Knowledge

34 problems with no pattern label attached, which is the only honest rehearsal for an interview. Work these last.

**Easy** (6)

| Problem | Difficulty |
|---|---|
| [Two Sum](two-sum.md) | Easy |
| Valid Perfect Square | Easy |
| Best Time to Buy and Sell | Easy |
| Valid Parentheses | Easy |
| Subtree of Another Tree | Easy |
| Design Parking System | Easy |

**Medium** (26)

| Problem | Difficulty |
|---|---|
| [Daily Temperatures](daily-temperatures.md) | Medium |
| Group Anagrams | Medium |
| Decode String | Medium |
| Valid Sudoku | Medium |
| Product of Array Except Self | Medium |
| Maximum Product Subarray | Medium |
| [Container With Most Water](container-with-most-water.md) | Medium |
| Palindromic Substrings | Medium |
| Remove Nth Node From End of List | Medium |
| Find Minimum in Rotated Sorted Array | Medium |
| Pacific Atlantic Water Flow | Medium |
| [Validate Binary Search Tree](validate-binary-search-tree.md) | Medium |
| Construct Binary Tree from Preorder and Inorder Traversal | Medium |
| [Clone Graph](clone-graph.md) | Medium |
| House Robber II | Medium |
| Decode Ways | Medium |
| Unique Paths | Medium |
| Word Break | Medium |
| Lowest Common Ancestor of a Binary Search Tree | Medium |
| Longest Consecutive Sequence | Medium |
| Meeting Rooms II | Medium |
| Encode and Decode Strings | Medium |
| Number of Connected Components in an Undirected Graph | Medium |
| Graph Valid Tree | Medium |
| Implement Trie (Prefix Tree) | Medium |
| Design Add and Search Words Data Structure | Medium |

**Hard** (2)

| Problem | Difficulty |
|---|---|
| Longest Valid Parentheses | Hard |
| [Serialize and Deserialize Binary Tree](serialize-and-deserialize-binary-tree.md) | Hard |

## Notes on this list

- `*` marks the problems the course files as Problem Challenges, which sit at the end of a chapter and are harder than the rest of it.
- 11 titles appear in two sections. Three of those are genuinely two-pattern problems. Number of Provinces works with either a [graph traversal](../patterns/graphs.md) or [union find](../patterns/union-find.md). Sum of Subarray Minimums works with either a [monotonic stack](../patterns/monotonic-stack.md) or a [monotonic queue](../patterns/monotonic-queue.md). Next Greater Element is taught twice, once as a plain [stack](../patterns/stacks.md) exercise and once as the [monotonic stack](../patterns/monotonic-stack.md) template. Most of the rest reappear in Test Your Knowledge, with the pattern label removed on purpose.
- **Balanced Parentheses is two different problems with one name.** Under [Stacks](#stacks) it means checking whether a string is balanced. Under [Subsets](#subsets) it means generating every valid string of n pairs. Read the section heading before you start.
- Difficulty is the course label, not ours. It is a reasonable guide, and it is not the same scale every site uses.
- 11 problems carry no difficulty label in the course, all of them in the two dynamic programming chapters. They are shown with a dash.
- Problem names match the course, so you can find them in the player. The only change is the "Problem 1:" numbering that a few chapters put in front of a title, which is dropped here.

## Go deeper

- The full worked solution for every problem here, in six languages, with an editor to attempt it yourself first: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- Not sure which pattern a problem needs? [How to recognize the pattern in sixty seconds](../cheat-sheets/recognize-the-pattern.md)
