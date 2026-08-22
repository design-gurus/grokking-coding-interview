# Pattern: Palindromic Subsequence (Dynamic Programming)

> The state is a range of the string rather than a position in it, and the answer for a range depends on its two ends plus the range inside them.

## What it is

Most one-dimensional dynamic programming asks "what is the best answer ending at position i". Palindrome problems cannot use that state, because a palindrome is defined by both of its ends at once.

So the state becomes a pair: the range from i to j. If the characters at the two ends match, the answer builds on the range just inside them. If they do not match, you drop one end or the other and take the better result. The same two-index state powers longest common subsequence and edit distance, which is why those problems feel related.

## Recognize it when

- The word palindrome appears, or the string is compared against its own reverse.
- Two strings are compared to each other, character by character, and characters may be skipped.
- The answer is about a range, and the decision at each step is made at the ends of that range.
- The brute force enumerates every substring, which is O(n²) substrings each costing O(n) to check.

**Words that give it away:** "palindrome", "palindromic", "subsequence", "common subsequence", "edit distance", "insertions to make", "minimum deletions".

## How it works

```
"cbbd", longest palindromic subsequence

ends match?      c...d  no  -> max(drop c, drop d)
                 b..b   yes -> 2 + whatever is inside

fill by range length, shortest first:
length 1: every single character is a palindrome, so 1
length 2: "cb" 1, "bb" 2, "bd" 1
length 3: "cbb" 2, "bbd" 2
length 4: "cbbd" 2
```

The fill order matters more than the recurrence. A range of length 4 needs the answer for the range of length 2 inside it, so you must fill by increasing length, not in plain index order.

Subsequence and substring are different problems. A subsequence may skip characters. A substring must be contiguous. Read the word carefully, because the two recurrences differ.

## The code template

```python
def longest_palindromic_subsequence(s):
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = 1                              # one character is a palindrome
    for length in range(2, n + 1):                # fill by range length
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                dp[i][j] = 2 + (dp[i + 1][j - 1] if length > 2 else 0)
            else:
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    return dp[0][n - 1]


def longest_common_subsequence(a, b):
    """The same two-index idea, one index per string."""
    dp = [[0] * (len(b) + 1) for _ in range(len(a) + 1)]
    for i in range(1, len(a) + 1):
        for j in range(1, len(b) + 1):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[len(a)][len(b)]
```

## Complexity

| | |
|---|---|
| Time | O(n²) for one string, O(n × m) for two |
| Space | O(n²), often reducible to O(n) by keeping only the previous row or diagonal |

## Variations

- Longest palindromic subsequence, and longest palindromic substring.
- Count all palindromic substrings.
- Minimum deletions or insertions to make a string a palindrome, which is the length minus the longest palindromic subsequence.
- Palindromic partitioning, the fewest cuts to split a string into palindromes.
- Longest common subsequence, longest common substring, and edit distance.
- The expand-around-center technique, which solves longest palindromic **substring** in O(n²) time and O(1) space, and is usually the better answer for that specific problem.

## Problems that use it

Longest Palindromic Subsequence, Longest Palindromic Substring, Count of Palindromic Substrings, Minimum Deletions in a String to make it a Palindrome, Palindromic Partitioning, Longest Common Subsequence, Edit Distance, Minimum Insertions to Make Palindrome.

## Common mistakes

- Filling the table in plain index order, which reads cells that have not been written yet. Fill by increasing range length, or iterate i downwards.
- Mixing up subsequence and substring. In a substring problem, a mismatch at the ends means the answer for that range is zero, not the maximum of two smaller ranges.
- Forgetting that a range of length 2 has nothing inside it, so `dp[i+1][j-1]` is out of order and must be treated as zero.
- Not seeding the diagonal, where every single character is a palindrome of length 1.
- Reaching for the table on longest palindromic substring when expand-around-center is simpler and uses no extra memory.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- Every dynamic programming pattern, in depth: [Grokking Dynamic Programming](https://www.designgurus.io/course/grokking-dynamic-programming)
