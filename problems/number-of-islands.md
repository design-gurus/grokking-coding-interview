# Number of Islands

> Count the groups of connected land cells in a grid of land and water.

**Pattern:** [Island (Matrix Traversal)](../patterns/island-matrix-traversal.md) | **Difficulty:** Easy in the course, Medium elsewhere

## The problem

You are given a 2D grid where each cell is land or water. An island is a group of land cells connected
horizontally or vertically. Return how many islands the grid contains. Assume the grid is surrounded
by water on all sides.

## Why this is an Island (Matrix Traversal) problem

A grid where a cell relates to its neighbors **is** a graph. The cells are the nodes and the edges are
computed rather than given: row plus or minus one, column plus or minus one.

Once you see that, the question renames itself. "How many islands" is "how many connected components",
and the standard way to count components is to scan for an unvisited node, run one search that consumes
its whole component, and count the searches.

The words "connected", "group", and "region" on a grid all land here.

## The approach

1. Walk every cell of the grid.
2. When you find a land cell that has not been visited, add one to the count and start a search from
   it.
3. The search consumes the whole island: from each cell, move to any neighboring land cell that has
   not been visited, marking cells as visited the moment you reach them.
4. When the search finishes, continue scanning for the next unvisited land cell.

The invariant: every land cell already visited belongs to an island that has already been counted.

Mark on the way **in**, not on the way out. If you mark a cell only when you pop it, the same cell can
enter the queue many times before it is ever popped, and the search stops being linear.

The cheapest way to mark is to overwrite the land cell with water, if changing the input is allowed.
Ask. If not, keep a separate visited grid.

## Complexity

| | |
|---|---|
| Time | O(rows × cols), every cell is visited once |
| Space | O(rows × cols) in the worst case, for the stack, the queue, or the visited grid |

## Edge cases to say out loud

- An empty grid, or a grid with no land.
- A grid that is entirely land, which is one island and the worst case for memory.
- **Recursion depth.** A depth first search written recursively on a large all-land grid recurses once
  per cell, which can overflow the stack. An iterative version with an explicit stack, or a breadth
  first search with a queue, avoids it.
- Whether diagonals count. This problem says they do not. Some variants say they do, and it changes
  only the direction list.
- Whether you may modify the input.

## Related problems

- Biggest Island, the same scan returning the largest count instead of the number of searches.
- Flood Fill, one search from a given cell rather than a scan for all of them.
- Surrounded Regions, which searches inward from the border, because the cells that touch the border
  are the ones that survive.
- Rotting Oranges, a multi-source search where every rotten cell starts in the queue and each round is
  one minute.
- Number of Islands II, where land is added over time, which is where
  [union find](../patterns/union-find.md) beats re-running the search.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
