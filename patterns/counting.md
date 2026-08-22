# Pattern: Counting

> Count how many times each value appears, then answer the real question from the counts rather than from the input.

## What it is

A large family of problems reduces to one frequency table plus a little logic on top of it. Two strings are anagrams if their letter counts match. The majority element is the value whose count passes half the length. A group of anagrams is a bucket of words sharing a count signature.

Counting is close to the [hash map](hash-maps.md) pattern, and it is really a specialization of it. The key is the value, and the stored thing is always a number.

## Recognize it when

- The question is about frequency, occurrences, duplicates, or how many distinct values there are.
- Anagrams, permutations of a string, or "can these be rearranged into" appear.
- The answer depends on the counts and not on the positions.
- The values come from a small fixed set, like lowercase letters, digits, or a bounded range.

**Words that give it away:** "frequency", "occurs", "how many times", "anagram", "majority", "duplicate", "distinct", "most common".

## How it works

```
"aabbbc"
counts: a=2 b=3 c=1

most frequent    -> b
distinct values  -> 3
is it an anagram of "abcbab"?  count that too and compare the two tables
```

When the value range is small and known, use a plain array instead of a map. Twenty-six slots for lowercase letters, or 128 for ASCII, is faster than hashing and easier to reason about.

Inside a [sliding window](sliding-window.md), the counts change as the window moves: increment on the way in, decrement on the way out. Remember to delete keys that fall to zero, or the count of distinct values will be wrong.

## The code template

```python
from collections import Counter

def is_anagram(a, b):
    return Counter(a) == Counter(b)


def majority_element(nums):
    """Boyer-Moore vote: the majority element in O(1) space, no counts stored."""
    candidate, votes = None, 0
    for num in nums:
        if votes == 0:
            candidate = num
        votes += 1 if num == candidate else -1
    return candidate            # only valid if a majority is guaranteed to exist


def count_with_array(word):
    """When the alphabet is small and known, an array beats a hash map."""
    counts = [0] * 26
    for char in word:
        counts[ord(char) - ord('a')] += 1
    return counts
```

## Complexity

| | |
|---|---|
| Time | O(n) to count, plus whatever the logic on top costs |
| Space | O(k), where k is the number of distinct values. O(1) when the alphabet is fixed. |

## Variations

- A frequency map, or a fixed-size array when the value range is small.
- Boyer-Moore majority vote, which finds the majority element in O(1) space.
- A count signature used as a grouping key, which is how anagrams are grouped.
- Counting inside a sliding window, incrementing and decrementing as it moves.
- Counting sort as a step toward an answer, which belongs to the [linear sorting](linear-sorting-algorithms.md) pattern.
- Counting pairs by remainder, or by some other computed property, rather than by value.

## Problems that use it

Count Elements With Maximum Frequency, Maximum Population Year, Least Number of Unique Integers after K Removals, Majority Element, Group Anagrams, Top K Frequent Elements, Valid Anagram, Find All Anagrams in a String, Sort Characters By Frequency.

## Common mistakes

- Using a hash map where a 26-slot array is enough, which is slower and harder to compare.
- Leaving zero entries in the map, so the count of distinct keys includes values that are no longer present.
- Boyer-Moore without the guarantee. If a majority element is not certain to exist, the algorithm returns a candidate that must be verified with a second pass.
- Ignoring case, whitespace, or non-letter characters in string counting problems. Ask what the input can contain.
- Sorting to compare two strings when counting is O(n) and sorting is O(n log n). Sorting is fine, but say that you know the difference.

## Go deeper

- This pattern's introduction in the course: [Introduction to Counting Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-counting-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
