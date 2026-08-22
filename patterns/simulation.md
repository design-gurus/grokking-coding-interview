# Pattern: Simulation

> Some problems have no shortcut. Model the state, apply the rules exactly as written, and step forward until it stops.

## What it is

A simulation problem describes a process, and the answer is whatever the process produces. There is no clever insight to find, and looking for one wastes the interview.

What is being tested is whether you can turn a wordy description into clean, correct code: name the state, write one step, and get every boundary right.

## Recognize it when

- The problem describes a process rather than a property: a robot moving, a game running, a queue being served, water being poured.
- The constraints are small, which is the strongest signal. A limit of a few thousand steps means the intended answer is to run them.
- The statement is long and full of rules, and none of the rules interact in a way that suggests a formula.
- You have looked for a pattern and found none. That absence is itself information.

**Words that give it away:** "simulate", "repeat until", "each second", "the robot moves", "the game ends when", "spiral", "in order, until nothing changes".

## How it works

Direction arrays keep grid movement short and readable. Instead of four copies of the same code, keep the offsets in a list and index into it.

```
directions: right (0,1), down (1,0), left (0,-1), up (-1,0)

turning right is just:  d = (d + 1) % 4
```

For time-based problems, a priority queue keyed by event time is usually better than stepping one tick at a time, because it skips the ticks where nothing happens.

Watch for repetition. If the state can return to something it has already been, the process loops forever. The answer is then found by detecting the cycle, not by running the loop.

## The code template

```python
def spiral_order(matrix):
    """Four shrinking boundaries, which is cleaner than tracking direction."""
    if not matrix:
        return []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    result = []
    while top <= bottom and left <= right:
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1
        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1
        if top <= bottom:                       # re-check, the row may be gone
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1
        if left <= right:                       # re-check, the column may be gone
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1
    return result


DIRECTIONS = [(0, 1), (1, 0), (0, -1), (-1, 0)]     # right, down, left, up

def walk(instructions):
    row = col = 0
    facing = 0
    for step in instructions:
        if step == 'R':
            facing = (facing + 1) % 4
        elif step == 'L':
            facing = (facing - 1) % 4
        else:
            dr, dc = DIRECTIONS[facing]
            row, col = row + dr, col + dc
    return row, col
```

## Complexity

| | |
|---|---|
| Time | O(steps × work per step). The constraints tell you what fits. |
| Space | O(state) |

## Variations

- A pointer or robot moving on a grid.
- Spiral and diagonal traversals.
- Game state evolving by a rule, as in Game of Life.
- Queue or stream processing, where requests arrive and are served.
- Time-based simulation with a priority queue of events.
- Iterating until the state stops changing, which needs a rule for when to stop.
- Cycle detection when the state can repeat, which lets you skip ahead by a huge number of steps.

## Problems that use it

Array Transformation, Water Bottles, Spiral Matrix, Spiral Matrix III, Robot Bounded in Circle, Game of Life, Pour Water, Walking Robot Simulation, Design Underground System.

## Common mistakes

- Looking for a clever formula when the constraints clearly permit brute force. Check the limits first.
- Updating the state in place when every update must see the original state. Game of Life is the classic trap: use a second grid, or encode both the old and new values in one cell.
- Off-by-one in boundaries, especially in the spiral traversal, where a single leftover row is walked twice without the re-check.
- Not detecting a repeating state, so the loop never ends.
- Writing four copies of the movement code instead of using a direction array, which triples the chance of a typo.
- Misreading a rule. Read the statement twice before writing anything, because a simulation is only as correct as your reading of it.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
