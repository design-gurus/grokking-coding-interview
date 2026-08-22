# Pattern: Fibonacci Numbers (Dynamic Programming)

> Each answer is built from a fixed number of earlier answers, so storing those few turns an exponential recursion into one linear pass.

## What it is

The Fibonacci recurrence, `f(n) = f(n-1) + f(n-2)`, is the simplest shape in dynamic programming. One dimension, a fixed lookback, and no choice to make beyond a small combining rule.

Written as plain recursion it is exponential, because `f(5)` recomputes `f(3)` twice, `f(2)` three times, and so on. Storing each answer the first time it is computed removes all of that. This is the pattern people mean when they say "dynamic programming is just recursion plus a cache".

## Recognize it when

- The answer at position n depends on a fixed number of earlier positions.
- The question counts the ways to reach step n, or to arrange n things under a local rule.
- Each position offers a small set of moves, like taking 1, 2, or 3 steps.
- A plain recursion visibly recomputes the same subproblem, which you can see by drawing two levels of the call tree.

**Words that give it away:** "how many ways", "climb", "steps", "you can jump", "adjacent", "cannot take two in a row", "minimum jumps".

## How it works

```
recursion:               memoized:              bottom-up:
f(5)                     f(5)                   f(0)=0 f(1)=1
├─ f(4)                  ├─ f(4)                f(2)=1 f(3)=2
│  ├─ f(3)               │  ├─ f(3)             f(4)=3 f(5)=5
│  │  └─ ...             │  │  └─ ...
│  └─ f(2)  recomputed   │  └─ f(2)  cached     one pass, no stack
└─ f(3)     recomputed   └─ f(3)     cached
```

Three ways to write it, in increasing order of how much an interviewer likes them: plain recursion, top-down with memoization, bottom-up with a table. Because only the last few entries are ever read, the table then collapses into two or three variables.

House Thief is the same shape with a different combining rule. Instead of adding the two previous answers, you take a maximum. Either rob this house and add the best from two back, or skip it and keep the best from one back.

## The code template

```python
def climb_stairs(n):
    """Bottom-up, then collapsed to two variables."""
    if n <= 2:
        return n
    two_back, one_back = 1, 2
    for _ in range(3, n + 1):
        two_back, one_back = one_back, one_back + two_back
    return one_back


def house_thief(houses):
    """Same shape, but the rule is a maximum instead of a sum."""
    skip, take = 0, 0                       # best ending before here, best including
    for money in houses:
        skip, take = max(skip, take), skip + money
    return max(skip, take)


def min_jumps(jumps):
    """When the lookback is not fixed, the table stays."""
    n = len(jumps)
    best = [float('inf')] * n
    best[0] = 0
    for i in range(n):
        for step in range(1, jumps[i] + 1):
            if i + step < n:
                best[i + step] = min(best[i + step], best[i] + 1)
    return best[n - 1]
```

## Complexity

| | |
|---|---|
| Time | O(n) when the lookback is fixed, O(n × k) when each position has k moves |
| Space | O(n) for the table, or O(1) once collapsed to variables |

## Variations

- Top-down with memoization, which is closest to the recursion you thought of first.
- Bottom-up with a table.
- Rolling variables for constant space.
- A different combining rule: maximum, minimum, or a count instead of a sum.
- A variable lookback, as in minimum jumps or number factors, where the table cannot collapse.

## Problems that use it

Fibonacci Numbers, Staircase, Number Factors, Minimum Jumps to Reach the End, Minimum Jumps with Fee, House Thief, Climbing Stairs, Decode Ways.

## Common mistakes

- Wrong base cases, which shifts every later answer by one. Compute f(0), f(1), and f(2) by hand and check them against your table.
- Collapsing to rolling variables before the recurrence is known to be correct. Get the table right first, then optimize.
- Assigning the two rolling variables in the wrong order, which uses the new value where the old one was needed. Simultaneous assignment avoids this.
- Overflow. Fibonacci numbers pass a 32-bit integer at n equal to 47.
- Recursing without a cache and expecting it to pass. Exponential is exponential.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- Every dynamic programming pattern, in depth: [Grokking Dynamic Programming](https://www.designgurus.io/course/grokking-dynamic-programming)
