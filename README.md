# Grokking the Coding Interview

> The free, open companion to the **Grokking the Coding Interview** course by [DesignGurus.io](https://www.designgurus.io/course/grokking-the-coding-interview), created by Arslan Ahmad.

Stop memorizing solutions. Learn the 41 patterns behind them, and a problem you have never seen starts to look familiar.

This repository is the free index, summary, and cheat sheet collection for that method. The full course adds worked solutions in six languages, runnable tests, and more than 300 hand-picked problems.

[![GitHub stars](https://img.shields.io/github/stars/design-gurus/grokking-coding-interview?style=social)](https://github.com/design-gurus/grokking-coding-interview/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/design-gurus/grokking-coding-interview)](https://github.com/design-gurus/grokking-coding-interview/commits/main)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](LICENSE)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

## Contents

- [What is "Grokking the Coding Interview"?](#what-is-grokking-the-coding-interview)
- [Is there a PDF or book?](#is-there-a-grokking-the-coding-interview-pdf-or-book)
- [How to use this repo](#how-to-use-this-repo)
- [How to attack a problem you have never seen](#how-to-attack-a-problem-you-have-never-seen)
- [The patterns](#the-patterns)
- [The problem index](#the-problem-index)
- [Cheat sheets](#cheat-sheets)
- [Glossary](#glossary)
- [Recommended reading](#recommended-reading)
- [What is coming next](#what-is-coming-next)
- [Contributing](#contributing)

## What is "Grokking the Coding Interview"?

"Grok" means to understand something so completely that it becomes intuitive.

Grokking the Coding Interview is the pattern-based approach to coding interviews. Instead of grinding hundreds of problems and hoping the right ones come up, you learn a small set of reusable techniques. The sliding window, the monotonic stack, the topological sort. Then you learn what each one looks like from the outside. Then a new problem is not new. It is a sliding window problem with a frequency map, and you have written that before.

There are 41 of these patterns, and they cover the overwhelming majority of what gets asked. This methodology was created by Arslan Ahmad. The original, fully updated course lives at [DesignGurus.io](https://www.designgurus.io/course/grokking-the-coding-interview).

The part people skip is recognition. Knowing how a sliding window works is easy. Knowing that the problem in front of you is a sliding window problem is the skill that gets tested, and it is what this repo is organized around. Every pattern page leads with a **Recognize it when** section.

## Is there a Grokking the Coding Interview PDF or book?

No. There is no official PDF, ebook, or printed book of the Grokking the Coding Interview course, and there never has been. The PDF files that circulate online are unofficial copies of an old version. They are missing the newer patterns, the newer problems, and every correction made since they were made.

This repository is the official free way to read the material. Every pattern guide and cheat sheet here is free in your browser, with no account needed. The full, current course, with runnable solutions in six languages, is online at [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview).

For more on the series, see [What is the Grokking series?](https://www.designgurus.io/answers/detail/what-is-the-grokking-series)

## How to use this repo

1. Read [how to recognize the pattern](cheat-sheets/recognize-the-pattern.md) once, so you have a routine for the first sixty seconds of any problem.
2. Work through the [patterns](patterns/) in order. For each one, read "Recognize it when" first, then the mechanism.
3. Practice with the [problem index](problems/), which maps every problem in the course to its pattern. Cover the pattern heading, name the pattern yourself, then check.
4. After every problem, write down which pattern it was and which cue told you. That note is worth more than the solution.
5. The night before an interview, reread only the "Recognize it when" sections.
6. Go deeper in the [full course](https://www.designgurus.io/course/grokking-the-coding-interview) when you want worked solutions and a place to run them.

## How to attack a problem you have never seen

A repeatable order beats improvising, and most of it happens before you write any code.

1. **Restate the problem** in one sentence, and confirm it with the interviewer.
2. **Ask about the input.** Sorted? Duplicates? Empty? Negative numbers? How large?
3. **Read the constraints**, and say out loud what complexity they allow. n up to 10^5 rules out O(n²).
4. **State the brute force.** Say what it costs. This is never wasted, and it proves you understood the question.
5. **Name the pattern.** "The window has to grow and shrink, so this is a sliding window with a frequency map."
6. **Walk one example by hand** before coding, so the invariant is settled.
7. **Write it**, saying what each part does as you go.
8. **Test it out loud** on the empty case, the one-element case, and the case that broke your first idea.
9. **State the final complexity**, in time and space, without being asked.

The full breakdown lives in [how to recognize the pattern](cheat-sheets/recognize-the-pattern.md).

## The patterns

All 41, one page each, are in [patterns/](patterns/). The most common twelve:

| Pattern | Recognize it when | Cost |
|---------|-------------------|------|
| [Two Pointers](patterns/two-pointers.md) | Sorted input, looking for a pair or triplet, or an in-place rewrite | O(n) |
| [Sliding Window](patterns/sliding-window.md) | A contiguous subarray or substring, longest or shortest | O(n) |
| [Fast and Slow Pointers](patterns/fast-and-slow-pointers.md) | Linked list, cycle or middle, no extra memory | O(n) |
| [Merge Intervals](patterns/merge-intervals.md) | Ranges that may overlap: meetings, bookings, time slots | O(n log n) |
| [Hash Maps](patterns/hash-maps.md) | Seen before, how many times, or grouped by a computed key | O(n) |
| [Monotonic Stack](patterns/monotonic-stack.md) | Next or previous greater or smaller element | O(n) |
| [Tree Level Order Traversal](patterns/tree-level-order-traversal.md) | An answer per level, or the shallowest match | O(n) |
| [Tree Depth First Search](patterns/tree-depth-first-search.md) | An answer per path, or ancestor to descendant | O(n) |
| [Graphs](patterns/graphs.md) | Nodes and edges, reachability, fewest hops | O(V + E) |
| [Modified Binary Search](patterns/modified-binary-search.md) | Sorted, rotated, or a search over the answer itself | O(log n) |
| [Top K Elements](patterns/top-k-elements.md) | Top K, Kth largest, or K most frequent | O(n log k) |
| [Backtracking](patterns/backtracking.md) | Build a configuration, undo it, prune what cannot work | Exponential |

See the full catalog, including the eleven advanced patterns: [patterns/README.md](patterns/README.md)

## The problem index

Every problem in the course, 302 of them, mapped to the pattern that solves it, with the difficulty the course assigns: [problems/README.md](problems/README.md)

22 of them have a walkthrough: the problem in our own words, the argument for why it belongs to its pattern, the approach and its invariant, the complexity, and the edge cases to say out loud. They stop short of the code, because writing the code is the part you need to practice.

| Problem | Pattern |
|---------|---------|
| [Two Sum](problems/two-sum.md) | [Hash Maps](patterns/hash-maps.md) |
| [Container With Most Water](problems/container-with-most-water.md) | [Two Pointers](patterns/two-pointers.md) |
| [Smallest Window containing Substring](problems/smallest-window-containing-substring.md) | [Sliding Window](patterns/sliding-window.md) |
| [Merge Intervals](problems/merge-intervals.md) | [Merge Intervals](patterns/merge-intervals.md) |
| [Number of Islands](problems/number-of-islands.md) | [Island Traversal](patterns/island-matrix-traversal.md) |
| [Daily Temperatures](problems/daily-temperatures.md) | [Monotonic Stack](patterns/monotonic-stack.md) |
| [Clone Graph](problems/clone-graph.md) | [Clone](patterns/clone.md) |
| [Tasks Scheduling](problems/tasks-scheduling.md) | [Topological Sort](patterns/topological-sort.md) |

Plus fourteen more, all linked from the index.

## Cheat sheets

- [How to recognize the pattern in sixty seconds](cheat-sheets/recognize-the-pattern.md), the constraint table, the trait lookup, the decision tree, and the patterns that get confused with each other.

More are on the way. See [what is coming next](#what-is-coming-next).

## Glossary

Amortized, invariant, subsequence versus substring, pseudo-polynomial, and the rest: [glossary.md](glossary.md)

## Recommended reading

Free guides on the DesignGurus blog and answers, plus every related course: [resources.md](resources.md)

Start with [the coding patterns, explained](https://www.designgurus.io/blog/grokking-the-coding-interview-patterns) and [do not just grind LeetCode](https://www.designgurus.io/blog/dont-just-leetcode).

## What is coming next

This repo is being built in the open. Planned, in order:

- **problems/**, more walkthroughs. The index is complete, and 22 of the 302 problems have a page so far.
- **cheat-sheets/**, more of them: flashcards, complexity tables, comparison sheets like BFS versus DFS and greedy versus dynamic programming, an edge-case checklist.
- **templates/**, the code skeleton for every pattern in Python, Java, and JavaScript.
- **roadmaps/**, study plans for one week, four weeks, from scratch, and for experienced engineers who have not interviewed in years.

Want one of them sooner, or want to write one? Open an issue or a pull request.

## Go deeper: the full course

This repo explains the patterns and how to recognize them. The course teaches each one in depth, with more than 300 problems, worked solutions in six languages, and an editor to run them in.

- Course: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- If the data structures themselves are the gap: [Grokking Data Structures](https://www.designgurus.io/course/grokking-data-structures-for-coding-interviews)
- If dynamic programming is the gap: [Grokking Dynamic Programming](https://www.designgurus.io/course/grokking-dynamic-programming)
- Practice live: [Mock interviews with ex-FAANG engineers](https://www.designgurus.io/mock-interviews)
- Designing systems, not just solving problems: [grokking-system-design](https://github.com/design-gurus/grokking-system-design)

## Newsletter

Coding and system design interview tips, straight to your inbox.

[Subscribe on Substack](https://designgurus.substack.com/)

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md). If this repo helps you, please star it so more engineers can find it.

## License

Content is licensed under [Creative Commons Attribution 4.0 (CC BY 4.0)](LICENSE). The code snippets are additionally under the MIT License, so you can paste them anywhere. You may share and adapt with attribution.

## About

Maintained by [DesignGurus.io](https://www.designgurus.io/), the home of the original Grokking the Coding Interview course by Arslan Ahmad.
