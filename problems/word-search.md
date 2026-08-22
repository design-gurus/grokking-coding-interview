# Word Search

> Decide whether a word can be spelled out by walking through neighboring cells of a grid.

**Pattern:** [Backtracking](../patterns/backtracking.md) | **Difficulty:** Medium

## The problem

You are given a grid of letters and a word. Return whether the word can be built from letters in
adjacent cells, moving up, down, left, or right. A cell may not be used twice within the same path.

## Why this is a Backtracking problem

Two details together make this backtracking rather than a plain grid search.

First, **a cell may not be reused inside one path**. That means "visited" is not a permanent fact about
a cell, the way it is when counting
[islands](../patterns/island-matrix-traversal.md). A cell used by a failed path must become available
again for a different path. Marking and then **unmarking** is the definition of backtracking.

Second, a partial path can be judged hopeless early. The moment the letter under you does not match the
next letter of the word, that branch is dead and everything below it can be skipped. That pruning is
what keeps the search usable, because the tree of all paths is enormous.

## The approach

1. For every cell in the grid, start a search there.
2. The search takes a cell and a position in the word:
   - If the position is past the end of the word, the whole word matched, so return true.
   - If the cell is out of bounds, or its letter does not match the letter at that position, return
     false.
   - Mark the cell as in use.
   - Try all four neighbors with the next position. If any returns true, return true.
   - **Unmark the cell** and return false.

The invariant: the marked cells are exactly those on the path currently being tried, and nothing else.

The unmark on the way out is the line that people forget. Without it, the cells used by the first
failed path stay marked, so every later path is blocked, and the answer comes back false on grids
where the word clearly exists.

A common trick is to mark by overwriting the cell with a character that cannot appear in the word, then
writing the original letter back afterwards. That avoids a separate visited grid.

## Complexity

| | |
|---|---|
| Time | O(rows × cols × 3^L), where L is the length of the word. Each start cell begins a search, and after the first step there are three directions to try rather than four, because you never go straight back. |
| Space | O(L) for the recursion stack |

Pruning changes the real runtime enormously and the worst case not at all. Say both.

## Edge cases to say out loud

- A word longer than the number of cells, which can never fit.
- A one-letter word, where any matching cell is the answer.
- An empty grid or an empty word. Ask what an empty word should return.
- Repeated letters in the word, as in a grid of all `A` and the word `AAAAA`. This is the case that
  exposes a missing unmark, and it is also the slowest input.
- Whether a cell may be reused across different paths. It may, and only within a single path it may
  not.

## Related problems

- Word Search II, which searches for many words at once and is where a
  [trie](../patterns/trie.md) of the word list turns a slow solution into a fast one.
- Sudoku Solver and N-Queens, the same make-recurse-undo loop on a constraint puzzle.
- Combination Sum, backtracking where the choice is a number rather than a direction.
- [Number of Islands](number-of-islands.md), the grid search where visited is permanent, which is the
  contrast worth being able to state.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
