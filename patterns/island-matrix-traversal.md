# Pattern: Island (Matrix Traversal)

> A grid is a graph where every cell is connected to its neighbors, so counting islands is counting connected components.

## What it is

In a grid problem, each cell is a node and its adjacent cells are its edges. You do not build an adjacency list, because the neighbors are computed: row plus or minus one, column plus or minus one.

Scan the grid. Every time you find a cell that belongs to a group and has not been visited, launch a search from it that consumes the whole group. Each launch equals one island.

## Recognize it when

- The input is a 2D grid of land and water, 1s and 0s, colors, or letters.
- The question counts groups, measures the biggest group, fills a region, or asks whether two cells are connected.
- Something spreads across the grid over time: rot, fire, water, infection.
- The problem gives a starting cell and asks how far something can reach.

**Words that give it away:** "grid", "matrix", "island", "region", "connected cells", "flood fill", "surrounded", "rotting", "shortest path in a maze".

## How it works

```
1 1 0 0        scan row by row
1 0 0 1        the first unvisited 1 is at (0,0)
0 0 1 1        a search from it covers (0,0), (0,1), (1,0), which is island 1
               scanning continues and finds (1,3), which starts island 2
               3 islands in total
```

Mark cells as visited the moment you reach them. The cheapest way is to overwrite the cell itself, for example writing a 0 over a 1, if the problem allows changing the input. Otherwise keep a separate visited grid.

When several sources spread at the same time, push all of them into the queue before the loop starts. That is multi-source breadth first search, and it is how "how many minutes until every orange is rotten" is solved.

## The code template

```python
from collections import deque

DIRECTIONS = [(-1, 0), (1, 0), (0, -1), (0, 1)]     # four-way, add diagonals if needed

def count_islands(grid):
    if not grid:
        return 0
    rows, cols = len(grid), len(grid[0])
    islands = 0

    def sink(r, c):
        stack = [(r, c)]
        grid[r][c] = '0'                             # mark on push
        while stack:
            row, col = stack.pop()
            for dr, dc in DIRECTIONS:
                nr, nc = row + dr, col + dc
                if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == '1':
                    grid[nr][nc] = '0'
                    stack.append((nr, nc))

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                islands += 1
                sink(r, c)
    return islands


def minutes_to_rot(grid):
    """Multi-source BFS: every rotten cell starts in the queue."""
    rows, cols = len(grid), len(grid[0])
    queue = deque((r, c) for r in range(rows) for c in range(cols) if grid[r][c] == 2)
    fresh = sum(row.count(1) for row in grid)
    minutes = 0
    while queue and fresh:
        for _ in range(len(queue)):                  # one minute per round
            row, col = queue.popleft()
            for dr, dc in DIRECTIONS:
                nr, nc = row + dr, col + dc
                if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                    grid[nr][nc] = 2
                    fresh -= 1
                    queue.append((nr, nc))
        minutes += 1
    return -1 if fresh else minutes
```

## Complexity

| | |
|---|---|
| Time | O(rows × cols). Every cell is visited once. |
| Space | O(rows × cols) in the worst case, for the stack, the queue, or the visited grid |

## Variations

- Count the islands.
- Measure the area of the largest island.
- Flood fill, which recolors a region.
- Capture the regions that do not touch the border, usually solved by searching inward from the border instead.
- Multi-source spreading, for rotting oranges or fire.
- Shortest path through a maze, which needs breadth first search rather than depth first search.
- Eight-way connectivity instead of four-way, which only changes the direction list.
- [Union find](union-find.md) as an alternative, and the better choice when cells are added over time.

## Problems that use it

Number of Islands, Biggest Island, Flood Fill, Number of Closed Islands, Surrounded Regions, Rotting Oranges, Max Area of Island, Cycle in a Matrix, Shortest Path in Binary Matrix.

## Common mistakes

- Not marking a cell as visited, which loops forever between two neighbors.
- Marking on pop instead of on push, which lets the same cell enter the queue many times.
- Swapping the row and column index, which usually still runs and quietly gives the wrong answer on a non-square grid.
- Checking the bounds after reading the cell rather than before, which throws an index error at the edges.
- Using recursion on a large grid, which can overflow the call stack. A grid of a million cells that is all land recurses a million deep.
- Missing that the problem needs eight-way connectivity, or using eight when it needs four.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
