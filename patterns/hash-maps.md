# Pattern: Hash Maps

> Remember what you have already seen, keyed by value, and a nested search collapses into a single pass.

## What it is

A hash map stores key to value pairs and looks them up in constant time on average. In interview problems it is almost always used the same way. You walk the input once, and at each element you ask the map about the elements you have already passed.

The map is memory in exchange for time. The nested loop that searches backwards through the array is replaced by one lookup.

## Recognize it when

- The question is "have I seen this before", "how many times does this appear", or "which items share a property".
- The input is unsorted and you are asked for a pair, and sorting is not allowed or would cost too much.
- You need to group things by something computed from them, like anagrams grouped by their sorted letters.
- The brute force is a nested loop where the inner loop only looks backwards.

**Words that give it away:** "duplicate", "unique", "first non-repeating", "how many times", "group by", "anagram", "seen before", "in O(n) time".

## How it works

Walk the input once. At each element, look up what you need, then record the current element for the elements that come after it.

```
two sum, target 9, on [2, 7, 11, 15]

2    is 7 in the map? no.    record 2 -> index 0
7    is 2 in the map? yes.   answer is [0, 1]
```

The order matters. Look up first, then insert. Doing it the other way lets an element match itself.

## The code template

```python
def two_sum(nums, target):
    seen = {}                                  # value -> index
    for i, value in enumerate(nums):
        if target - value in seen:             # look up before inserting
            return [seen[target - value], i]
        seen[value] = i
    return [-1, -1]


def group_anagrams(words):
    groups = {}
    for word in words:
        key = ''.join(sorted(word))            # the computed key is the pattern
        groups.setdefault(key, []).append(word)
    return list(groups.values())
```

## Complexity

| | |
|---|---|
| Time | O(n) on average. The worst case is O(n²) if every key collides, which does not happen in practice with a good hash. |
| Space | O(n) |

## Variations

- Value to index, for pair and complement problems.
- Value to count, for frequency questions. See also the [counting](counting.md) pattern.
- A computed key, for grouping. Sorted letters for anagrams, a canonical form for shape matching.
- A set, when only membership matters and there is no value to store.
- Running total to count, which is what makes [prefix sum](prefix-sum.md) subarray counting linear.
- Original node to cloned node, which is the heart of the [clone](clone.md) pattern.

## Problems that use it

Two Sum, First Non-repeating Character, Contains Duplicate, Longest Palindrome, Ransom Note, Group Anagrams, Isomorphic Strings, Longest Consecutive Sequence, Valid Sudoku.

## Common mistakes

- Claiming O(1) worst case. The guarantee is O(1) on average, and an interviewer may ask you to say so.
- Inserting the current element before looking it up, so it matches itself.
- Reading a key that was never inserted, instead of defaulting to zero. Use `Counter`, `defaultdict`, or `get(key, 0)`.
- Using a hash map when a plain array is enough. If the keys are the 26 lowercase letters or the numbers 0 to 100, an array is faster and simpler.
- Using a mutable object as a key, or a list where a tuple is required.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- The data structure itself, from scratch: [Grokking Data Structures](https://www.designgurus.io/course/grokking-data-structures-for-coding-interviews)
