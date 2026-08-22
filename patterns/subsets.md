# Pattern: Subsets

> Every existing partial answer produces new partial answers when the next element is considered, which builds the whole power set without any recursion.

## What it is

There are 2^n subsets of a set of n elements, because each element is either in or out. The Subsets pattern builds them all by starting with the empty subset. For each new element, copy every subset built so far and add the element to the copies.

The same idea generates permutations, with a different rule: insert the new element at every position of every permutation built so far.

## Recognize it when

- The problem asks for all subsets, all permutations, all combinations, or the power set.
- The answer is a list of lists, not a single number.
- The input is small. Anything over about 20 elements cannot be enumerated, so if n is large the problem wants something else, usually [dynamic programming](0-1-knapsack.md).
- The constraints mention that the input may contain duplicates, which is the hint that the deduplication rule matters.

**Words that give it away:** "all possible", "generate every", "power set", "combinations", "permutations", "distinct subsets".

## How it works

```
start:            [[]]
add 1:            [[], [1]]
add 2:            [[], [1], [2], [1,2]]
add 3:            [[], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]]
```

The list doubles with every element, which is where 2^n comes from.

Duplicates are the only hard part. Sort the input first, so equal values sit next to each other. When the current element equals the previous one, extend only the subsets that were created in the previous round. Extending all of them would produce the same subset twice.

## The code template

```python
def subsets(nums):
    result = [[]]
    for num in nums:
        result += [current + [num] for current in result]
    return result


def subsets_with_duplicates(nums):
    nums.sort()                                   # equal values must be adjacent
    result = [[]]
    start, end = 0, 0
    for i, num in enumerate(nums):
        start = 0
        if i > 0 and nums[i] == nums[i - 1]:
            start = end                           # only extend the newest ones
        end = len(result)
        for j in range(start, end):
            result.append(result[j] + [num])
    return result


def permutations(nums):
    result = [[]]
    for num in nums:
        next_round = []
        for partial in result:
            for position in range(len(partial) + 1):
                next_round.append(partial[:position] + [num] + partial[position:])
        result = next_round
    return result
```

## Complexity

| | |
|---|---|
| Time | O(n × 2^n) for subsets, O(n × n!) for permutations. The n factor is the cost of copying each result. |
| Space | Same as time, because the output is the space |

## Variations

- Subsets of a set with no duplicates.
- Subsets with duplicates, using the sort-and-skip rule.
- Permutations, and permutations with duplicates.
- Combinations of exactly K elements, which is a subset walk that stops at depth K.
- Letter case permutations, where each letter is either upper or lower.
- Balanced parentheses and generalized abbreviations, which are subset walks with a validity rule.
- The recursive form of all of these is [backtracking](backtracking.md). The choice between the two is style, not complexity, until pruning enters the picture, and then backtracking wins.

## Problems that use it

Subsets, Subsets With Duplicates, Permutations, String Permutations by changing case, Balanced Parentheses, Unique Generalized Abbreviations, Combination Sum, Letter Combinations of a Phone Number.

## Common mistakes

- Not sorting before deduplicating. The skip rule only works when equal values are adjacent.
- Extending every existing subset when the current element is a duplicate, which produces the same subset twice.
- Appending the working list instead of a copy, so every entry in the result points at the same list.
- Confusing subsets with permutations. In a subset, order does not matter, and [1,2] and [2,1] are the same answer. In a permutation they are different.
- Trying to enumerate when n is 40. Read the constraints before choosing this pattern.

## Go deeper

- This pattern's introduction in the course: [Introduction to Subsets Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-subsets-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- Recursion and backtracking in depth: [Grokking the Art of Recursion](https://www.designgurus.io/course/grokking-recursion-for-coding-interview)
