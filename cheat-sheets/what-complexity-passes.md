# What Complexity Passes: Reading the Answer Out of the Constraints

The size of the input is not decoration. It is the interviewer telling you which solutions are
allowed, and most candidates walk straight past it.

A rough rule: a machine does about 10^8 simple operations per second in a compiled language, and
closer to 10^7 in Python. So if the input is 10^5 and your solution is O(n²), that is 10^10
operations, which is not a slow answer, it is a wrong one.

## The table

Find the largest n in the constraints, and read across.

| n is up to | You can afford | Which usually means |
|---|---|---|
| 10 to 12 | O(n!) | Permutations, [backtracking](../patterns/backtracking.md) over orderings |
| 15 to 25 | O(2^n) | [Subsets](../patterns/subsets.md), bitmask over subsets, brute force over every choice |
| 30 to 45 | O(2^(n/2)) | [Meet in the middle](../patterns/meet-in-the-middle.md), split the input in half |
| 100 to 500 | O(n³) | Range [dynamic programming](../patterns/palindromic-subsequence.md), Floyd-Warshall |
| 1,000 to 5,000 | O(n²) | Two-dimensional dynamic programming, pairwise comparison |
| 10^5 to 10^6 | O(n log n) | Sorting, heaps, [binary search](../patterns/modified-binary-search.md), [segment tree](../patterns/segment-tree.md) |
| 10^6 to 10^8 | O(n) | [Sliding window](../patterns/sliding-window.md), [two pointers](../patterns/two-pointers.md), [prefix sum](../patterns/prefix-sum.md), [counting](../patterns/counting.md) |
| 10^9 and above | O(log n) or O(1) | Binary search on the answer, arithmetic, bit tricks |

Read it in both directions. Small n means the intended answer is probably exponential, so stop
looking for a clever formula. Huge n means no scan is allowed at all.

## What each shape costs, in real numbers

| n | O(log n) | O(n) | O(n log n) | O(n²) | O(2^n) |
|---|---|---|---|---|---|
| 10 | 3 | 10 | 33 | 100 | 1,024 |
| 100 | 7 | 100 | 664 | 10,000 | 10^30 |
| 1,000 | 10 | 1,000 | 9,966 | 10^6 | too large |
| 10^5 | 17 | 10^5 | 1.7 × 10^6 | 10^10 | too large |
| 10^6 | 20 | 10^6 | 2 × 10^7 | 10^12 | too large |

The jump from O(n log n) to O(n²) at n equal to 10^5 is the one that decides most interviews. The log
factor costs you 17 times. The square costs you 100,000 times.

## Other numbers hidden in the constraints

| The constraint says | It is telling you |
|---|---|
| "values are between 1 and n" | [Cyclic sort](../patterns/cyclic-sort.md), or use the values as indices |
| "0 <= value <= 1000" | A counting array beats a hash map, and [linear sorting](../patterns/linear-sorting-algorithms.md) is possible |
| "lowercase English letters" | 26 slots, so a fixed array and O(1) space for the alphabet |
| "the answer fits in a 32-bit integer" | Watch for overflow in the intermediate steps, not the answer |
| "in O(1) extra space" | No hash set, no visited array. Think [two pointers](../patterns/two-pointers.md), [fast and slow](../patterns/fast-and-slow-pointers.md), [XOR](../patterns/bitwise-xor.md), or writing into the input |
| "the array is sorted" | [Binary search](../patterns/modified-binary-search.md) or [two pointers](../patterns/two-pointers.md), and it is never mentioned by accident |
| "each element appears twice except one" | [Bitwise XOR](../patterns/bitwise-xor.md) |
| "return the count, not the list" | Counting or dynamic programming, not enumeration |
| "q queries" with q large | Precompute: [prefix sums](../patterns/prefix-sum.md), or a [segment tree](../patterns/segment-tree.md) if the values change |
| No constraints given at all | Ask. "How large can the input be?" is a question interviewers want to hear |

## Binary search on the answer

A specific and often-missed reading. When n is 10^9 but the question asks for the **smallest value
that works**, you are not searching the input, you are searching the range of answers.

The tell is wording like "minimum capacity", "slowest speed", "fewest days", "smallest largest sum".
Write a helper that answers "does this candidate work", check that the answer is monotonic, and binary
search the candidate range. The cost is O(n log range), where the log is over the answer range and not
over the input. See [modified binary search](../patterns/modified-binary-search.md).

## How to use this in the room

Say it out loud, in one sentence, before you write anything:

> "n is up to 10^5, so O(n²) is about 10^10 operations and will not pass. I need something closer to
> O(n log n), which points at sorting or a heap."

That sentence does three things. It proves you read the constraints, it rules out the brute force
without you having to code it, and it usually makes the pattern obvious as you say it.

## Go deeper

- [How to recognize the pattern in sixty seconds](recognize-the-pattern.md)
- [Complexity of every pattern and data structure](complexity-cheat-sheet.md)
- Complexity from the ground up: [Grokking Algorithm Complexity and Big O](https://www.designgurus.io/course/grokking-algorithm-complexity-and-big-o)
