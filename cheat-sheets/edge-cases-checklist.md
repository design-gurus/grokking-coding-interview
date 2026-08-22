# Edge Cases Checklist

Most failed submissions are not wrong algorithms. They are correct algorithms that never considered
the empty input, the single element, or the two equal values.

Read the list that matches your input type before you write code, and say the relevant ones out loud.
Naming an edge case before you hit it reads very differently from discovering it in a failing test.

## Always ask, whatever the input

- What is the smallest possible input? Empty, or one element?
- Can the input be null, as opposed to empty?
- Are duplicates allowed?
- Can values be negative? Can they be zero?
- Is the input sorted, and can I sort it?
- Am I allowed to modify the input?
- How large can it get? See [what complexity passes](what-complexity-passes.md).
- What should I return when there is no answer?

## Arrays

- Empty array, and an array of one element.
- All elements equal.
- Already sorted, and sorted in reverse. These are the worst cases for quicksort and the best cases for
  insertion sort.
- Negative numbers, which break any window that assumes the sum only grows.
- Zeros, especially in product problems, where one zero changes everything.
- The answer being the whole array, or a single element.
- Integer overflow in a running total or a midpoint.
- Two pointers crossing. Decide whether `left < right` or `left <= right` is correct, and say why.

## Strings

- Empty string, and a string of one character.
- All the same character. This is the slowest input for most string algorithms and it exposes missing
  undo steps.
- Case sensitivity. Is `A` the same as `a`?
- Spaces, punctuation, and non-letter characters. Ask what the alphabet is.
- Unicode, if the interviewer wants to go there. In Java a `char` is 16 bits, so an emoji is two of
  them.
- Building a string in a loop, which is O(n²) in every mainstream language.

## Linked lists

- Empty list, and a single node.
- Two nodes, which is where most pointer rewiring breaks.
- Operating on the head, which is why a dummy node in front of it is worth the two extra lines.
- The tail's next pointer after a reversal. If you do not set it to null, you have made a cycle.
- Checking both `fast` and `fast.next` before taking two steps.
- Even versus odd length, when the question is about the middle.

## Trees

- Null root.
- A single node, which is both the root and a leaf.
- A tree shaped like a list, where the recursion is n deep.
- A node with exactly one child. This is the case that breaks minimum-depth solutions that treat a
  null child as a leaf.
- Duplicate values, especially in a binary search tree, where you must ask which side they go on.
- Negative values, which break any solution seeded with zero as the smallest possible sum.

## Graphs

- A disconnected graph, so one search does not reach everything.
- A node with no edges at all. It still belongs in the answer.
- Self-loops, and duplicate edges between the same pair.
- A cycle, which is either the thing you must detect or the thing that makes you loop forever.
- Directed versus undirected, and whether you built the adjacency list the right way.
- Node labels that are not 0 to n minus 1. A dictionary is safer than an array.

## Grids

- An empty grid, or a grid with zero columns.
- A one by one grid.
- The whole grid being one region, which is the worst case for memory and recursion depth.
- Row and column indices swapped. On a square grid this still runs and quietly gives wrong answers.
- Bounds checked **before** reading the cell, not after.
- Four-way versus eight-way connectivity.

## Numbers and search

- The target sitting at the first index, or the last one.
- The target not being present at all.
- A search range that never shrinks, which is an infinite loop rather than a wrong answer.
- `(low + high) / 2` overflowing in a fixed-width language.
- Division by zero, and the modulo of a negative number, which is negative in Java and JavaScript.
- A single element, where `low` and `high` are the same index.

## Heaps and windows

- k larger than the number of elements.
- k equal to 1, and k equal to n.
- A window larger than the array.
- Recording an answer before the first full window exists.
- Removing an element that has left the window from a heap, which a heap cannot do cheaply. That needs
  lazy deletion or an [ordered set](../patterns/ordered-set.md).

## Recursion

- The base case running before anything else touches a null.
- Undoing the state change on the way back, in [backtracking](../patterns/backtracking.md).
- Appending a copy of the working list, not the list itself.
- Recursion depth against the stack limit, which is about 1,000 frames in Python by default.

## How to use this in the room

Pick three, before you write code:

> "Before I start: the array can be empty, values can be negative so a sliding window will not work
> here, and I will assume duplicates are allowed unless you say otherwise."

Then, after the code is written, walk the smallest input by hand. Almost every off-by-one shows up on
an input of length one.

## Go deeper

- [How to talk through a coding interview](interview-communication.md)
- [How to recognize the pattern in sixty seconds](recognize-the-pattern.md)
