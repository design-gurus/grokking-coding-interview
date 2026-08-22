# Pattern: Greedy Algorithms

> Take the best-looking option at every step and never look back. The sort you choose first is what makes that safe.

## What it is

A greedy algorithm makes the locally best choice at each step and never reconsiders it. That is fast and simple, and it is wrong more often than people expect.

What makes a greedy algorithm correct is an argument, not a feeling. The usual argument is an exchange argument: show that any optimal answer can be rewritten to include your greedy choice without getting worse. In interviews that argument almost always comes from sorting the input by the right key.

## Recognize it when

- The problem asks for the maximum number of items you can take, or the minimum number of moves.
- Sorting by one obvious quantity makes each decision look inevitable: by end time, by size, by ratio, by deadline.
- The problem is about scheduling, assignment, or handing out limited resources.
- A dynamic programming solution would work but the constraints are far too large for a table.

**Words that give it away:** "maximum number of", "minimum number of", "non-overlapping", "assign", "as many as possible", "least effort".

## How it works

Interval scheduling is the clearest case. To fit the most meetings into a room, sort by **end** time, not start time and not duration.

```
sorted by end:  [1,3]  [2,5]  [4,7]  [6,8]

take [1,3]                  it ends earliest, so it blocks the least
skip [2,5]                  starts before 3
take [4,7]
skip [6,8]                  starts before 7
answer: 2 meetings
```

Finishing earliest leaves the most room for everything that follows. That sentence is the exchange argument, and it is the kind of sentence an interviewer wants to hear before you write code.

## The code template

```python
def max_non_overlapping(intervals):
    """The most intervals you can take with no overlap."""
    intervals.sort(key=lambda pair: pair[1])       # by END time, deliberately
    taken, last_end = 0, float('-inf')
    for start, end in intervals:
        if start >= last_end:                     # compatible with what we have
            taken += 1
            last_end = end
    return taken


def assign_cookies(children, cookies):
    """Two sorted lists walked with two pointers, the other common greedy shape."""
    children.sort()
    cookies.sort()
    child = cookie = satisfied = 0
    while child < len(children) and cookie < len(cookies):
        if cookies[cookie] >= children[child]:    # smallest cookie that works
            satisfied += 1
            child += 1
        cookie += 1
    return satisfied
```

## Complexity

| | |
|---|---|
| Time | O(n log n), dominated by the sort |
| Space | O(1) beyond the sort |

## Variations

- Interval scheduling, sorted by end time.
- Two sorted lists matched against each other with [two pointers](two-pointers.md).
- A heap when the best next choice changes as you go, so the ordering cannot be fixed up front.
- Building an answer character by character with a [monotonic stack](monotonic-stack.md), as in remove k digits.
- Sorting by a ratio, as in fractional knapsack. Note that the 0/1 version cannot be solved greedily, which is exactly why [0/1 knapsack](0-1-knapsack.md) is a separate pattern.

## Problems that use it

Maximum Length of Pair Chain, Non-overlapping Intervals, Minimum Add to Make Parentheses Valid, Remove Duplicate Letters, Largest Palindromic Number, Assign Cookies, Jump Game, Gas Station, Task Scheduler.

## Common mistakes

- Not justifying the greedy choice. This is the single most common failure in this pattern, and interviewers ask about it directly. Be ready to say why the local choice is safe.
- Sorting by the wrong key. Sorting intervals by start time, or by duration, gives a confident wrong answer on the scheduling problem.
- Using greedy where an early choice can cost you later. If taking the best item now can block two better items, you need [dynamic programming](0-1-knapsack.md).
- Assuming that because greedy passes the examples, it is correct. Try to build a counterexample before committing.
- Forgetting the tie-breaking rule, which changes the answer in some assignment problems.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
