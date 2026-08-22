# Pattern: Mo's Algorithm

> Answer many range queries by reordering them, so each query's range is close to the last one and the window only has to shuffle a little between answers.

## What it is

You are given a fixed array and many queries of the form "tell me something about the range from L to R". The obvious approach walks each range, which costs O(n) per query.

Mo's algorithm keeps one window and moves its two ends to match each query in turn. Moving an end by one position costs O(1), because you add or remove a single element from a running aggregate. Sorting the queries so that consecutive ones have similar ranges keeps the total movement to O((n + q) × √n).

This is a competitive programming technique. It is rare in interviews, and it is included here for completeness and for contest preparation.

## Recognize it when

- The array never changes, and there are many range queries, both up to about 10^5 or 10^6.
- Every query is known in advance, so they can be reordered. This is called offline.
- The aggregate can be updated by adding or removing one element at a time, but cannot be subtracted, which rules out [prefix sums](prefix-sum.md).
- The typical question: how many distinct values are in this range, or the most frequent value in this range.

**Words that give it away:** "q queries", "given all queries in advance", "distinct elements in a range", "no updates", with n and q both large.

## How it works

```
block size = sqrt(n)

sort queries by (block of L, then R)
  within one block of L, R only ever moves forward
  L moves at most one block width between queries

move the current window one step at a time:
  while curL > L: curL -= 1; add(curL)
  while curR < R: curR += 1; add(curR)
  while curL < L: remove(curL); curL += 1
  while curR > R: remove(curR); curR -= 1
```

The two `add` and `remove` functions carry all the problem-specific logic. For counting distinct values, `add` increments a frequency and bumps the distinct count when it goes from zero to one, and `remove` does the reverse.

Sorting R in alternating directions per block, ascending in one block and descending in the next, avoids R sweeping all the way back at every block boundary. It is a constant-factor improvement worth knowing.

## The code template

```python
def mos_algorithm(nums, queries):
    """queries are (left, right) index pairs, inclusive. Returns one answer each."""
    import math
    block = max(1, int(math.sqrt(len(nums))))

    order = sorted(range(len(queries)),
                   key=lambda i: (queries[i][0] // block,
                                  queries[i][1] if (queries[i][0] // block) % 2 == 0
                                  else -queries[i][1]))

    counts = {}
    state = {'distinct': 0}

    def add(index):
        value = nums[index]
        counts[value] = counts.get(value, 0) + 1
        if counts[value] == 1:
            state['distinct'] += 1

    def remove(index):
        value = nums[index]
        counts[value] -= 1
        if counts[value] == 0:
            state['distinct'] -= 1

    answers = [0] * len(queries)
    cur_left, cur_right = 0, -1                 # an empty window
    for i in order:
        left, right = queries[i]
        while cur_left > left:
            cur_left -= 1
            add(cur_left)
        while cur_right < right:
            cur_right += 1
            add(cur_right)
        while cur_left < left:
            remove(cur_left)
            cur_left += 1
        while cur_right > right:
            remove(cur_right)
            cur_right -= 1
        answers[i] = state['distinct']
    return answers
```

## Complexity

| | |
|---|---|
| Time | O((n + q) × √n) |
| Space | O(n) for the aggregate, plus O(q) for the sorted query order |

## Variations

- Mo's on subarrays, which is the standard version.
- Mo's on trees, using an Euler tour to turn subtree ranges into array ranges.
- Mo's with updates, which adds a third dimension to the sort and gets considerably harder.
- Offline queries answered in a different sorted order, which is a broader idea worth recognizing even when Mo's itself is not the answer.

## Problems that use it

XOR Queries of a Subarray, Distinct Elements in a Subarray, Minimum Absolute Difference Queries, Powerful Array, Range Frequency Queries.

## Common mistakes

- Using it when the queries are online, meaning each answer must be produced before the next query is revealed. Mo's requires reordering, so it cannot be used.
- Using it when the aggregate can be subtracted. If prefix sums work, use prefix sums, because they are O(1) per query.
- Getting the expand and shrink order wrong. Expand before you shrink, or the window can pass through an invalid state where the right end is left of the left end.
- Forgetting to restore the original query order before returning the answers.
- Choosing a block size other than about √n, which changes the complexity.
- Reaching for it in an interview. If the array changes, use a [segment tree](segment-tree.md) or a [binary indexed tree](binary-indexed-tree.md), which is what an interviewer is far more likely to be looking for.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
