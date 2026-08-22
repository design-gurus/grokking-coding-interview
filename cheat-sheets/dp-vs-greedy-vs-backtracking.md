# Dynamic Programming vs Greedy vs Backtracking

All three explore a set of choices. They differ in how much of that set they actually visit, and
picking the wrong one is the most expensive mistake in an interview: greedy where you needed dynamic
programming gives a confident wrong answer, and backtracking where you needed dynamic programming
times out.

## The one-line rule

- **Greedy**: take the best choice now and never reconsider. Correct only when you can argue it.
- **Dynamic programming**: try every choice, but remember each subproblem so you solve it once.
- **Backtracking**: try every choice, and abandon a branch as soon as it cannot work.

## Side by side

| | Greedy | Dynamic Programming | Backtracking |
|---|---|---|---|
| Explores | One path | Every subproblem, once each | Every branch that is not pruned |
| Typical cost | O(n log n) | O(states × choices) | Exponential |
| Output | One answer | A count, or the best value | Every valid configuration |
| Needs | A proof, usually from a sort | Overlapping subproblems | A rule that kills branches early |
| Fails when | An early choice blocks a better later one | The state space is too large | Nothing prunes, so the tree is complete |

## How to choose, in order

**1. What is the output?**

If it is a **list of every valid answer**, you are enumerating, and that is backtracking or the
[subsets](../patterns/subsets.md) build. No amount of caching turns "list them all" into something
cheaper, because the list itself is exponential.

If it is **a count** or **one best value**, enumeration is wasteful. Look at dynamic programming.

**2. Does one sort make every choice obvious?**

Interval scheduling sorted by end time. Two lists matched against each other in order. Handing out
the smallest thing that fits. If yes, greedy, and be ready to say **why** the local choice is safe.

**3. Can an early choice cost you later?**

This is the question that separates greedy from dynamic programming, and it is worth trying to build
a counterexample before you commit. The classic one:

```
coins [1, 3, 4], target 6

greedy, largest first:  4 + 1 + 1 = 3 coins
optimal:                3 + 3     = 2 coins
```

Taking the 4 looked best and blocked the better answer. That is exactly when greedy is wrong and
dynamic programming is right.

**4. Are the same subproblems being recomputed?**

Draw two levels of the recursion tree. If the same arguments turn up twice, cache them, and you have
converted backtracking into dynamic programming. If every branch is unique, caching buys nothing and
the answer is pruning instead.

## The same problem, three ways

Take "choose items to reach a target sum".

| Question | Method | Why |
|---|---|---|
| List every subset that reaches it | [Backtracking](../patterns/backtracking.md) | The output is exponential, so the work must be |
| How many subsets reach it | [Dynamic programming](../patterns/0-1-knapsack.md) | Count without listing, one entry per state |
| Reach it with the fewest items, sizes divide evenly | [Greedy](../patterns/greedy-algorithms.md) | The divisibility is what makes the local choice safe |
| Reach it, n is 40 and the values are huge | [Meet in the middle](../patterns/meet-in-the-middle.md) | Too big to enumerate, too large a range to tabulate |

Same input, four methods. The wording of the question picks one.

## Greedy is only correct with an argument

Two kinds of argument are worth being able to state.

**The exchange argument.** Take any optimal answer. Show you can swap one of its choices for your
greedy choice without making it worse. Repeat, and you have turned the optimal answer into yours.
Interval scheduling: any optimal schedule can be rewritten to start with the interval that ends
earliest, because that interval leaves at least as much room as any other.

**The stays-ahead argument.** Show that after every step, your partial answer is at least as good as
any other partial answer of the same length.

If you cannot produce either one in about a minute, assume greedy is wrong and reach for dynamic
programming.

## Turning a recursion into dynamic programming

The path from a brute-force recursion to a table, which is the safest route under time pressure:

1. Write the plain recursion, however slow. It defines the subproblem.
2. Name the arguments that change. Those are the state.
3. Add a cache keyed by that state. It is now top-down dynamic programming, and usually fast enough.
4. If asked, turn the cache into a table filled in dependency order, which removes the recursion.
5. If only the last row or two is ever read, collapse the table to that.

Do not start at step 5. People who write the rolling variables first get the recurrence wrong and
have nothing to debug against.

## Common wrong turns

- Greedy on a knapsack with arbitrary weights. The value-to-weight ratio is right for the fractional
  version and wrong for the [0/1 version](../patterns/0-1-knapsack.md).
- Dynamic programming with a capacity of 10^9. The table is pseudo-polynomial, so it depends on the
  size of the number, not just the count of items.
- Backtracking with no pruning on n equal to 30. The tree is a billion nodes.
- Caching a state that does not capture everything the answer depends on. If two different situations
  share a cache key, the cache returns a wrong answer instead of a slow one.

## Go deeper

- [Greedy Algorithms](../patterns/greedy-algorithms.md), [0/1 Knapsack](../patterns/0-1-knapsack.md), [Fibonacci Numbers](../patterns/fibonacci-numbers.md), [Palindromic Subsequence](../patterns/palindromic-subsequence.md), [Backtracking](../patterns/backtracking.md), [Subsets](../patterns/subsets.md)
- Every dynamic programming pattern in depth: [Grokking Dynamic Programming](https://www.designgurus.io/course/grokking-dynamic-programming)
- Recursion and backtracking in depth: [Grokking the Art of Recursion](https://www.designgurus.io/course/grokking-recursion-for-coding-interview)
