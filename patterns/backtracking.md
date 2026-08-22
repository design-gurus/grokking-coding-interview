# Pattern: Backtracking

> Make a choice, recurse, then undo the choice. Cutting off branches that cannot lead anywhere is what separates this from brute force.

## What it is

Backtracking walks a tree of decisions. At each node you pick one of the available options and recurse into the smaller problem that remains. Then you put things back exactly as they were, and try the next option.

That undo step is the pattern's name and its most common bug. Without it, a choice made on one branch leaks into every branch that follows.

The other half is pruning. Testing a partial answer against the constraints and abandoning it early is what makes backtracking usable on problems where the full tree has billions of nodes.

## Recognize it when

- The problem asks for all valid configurations, or for any one configuration meeting strict rules.
- The answer is built up piece by piece, and a partial answer can be judged invalid before it is finished.
- It is a puzzle: a board, a maze, a placement, a partition, a schedule.
- The input is small, usually under about 20 elements, because the tree is exponential.

**Words that give it away:** "all possible", "place n queens", "solve the sudoku", "valid combinations", "word search", "partition the string", "generate all".

## How it works

```
generate parentheses, n = 2

            ""
            |
            "("                     ")" is pruned: nothing to close
          /     \
      "(("       "()"
        |          |
      "(()"      "()("
        |          |
     "(())"      "()()"
```

The two branches that would start with a closing parenthesis are never explored. That is pruning, and it is why this runs in far less than the full 2^(2n) time.

Every backtracking function has the same four parts. A base case that records a finished answer, a loop over the available choices, a validity test, and the make, recurse, undo sequence.

## The code template

```python
def solve(state, choices, result):
    if is_complete(state):
        result.append(list(state))       # copy, or every answer aliases
        return
    for choice in choices:
        if not is_valid(state, choice):
            continue                     # prune before recursing
        state.append(choice)             # make the choice
        solve(state, remaining(choices, choice), result)
        state.pop()                      # undo it


def generate_parentheses(n):
    result = []

    def build(current, opened, closed):
        if len(current) == 2 * n:
            result.append(''.join(current))
            return
        if opened < n:                   # still allowed to open
            current.append('(')
            build(current, opened + 1, closed)
            current.pop()
        if closed < opened:              # only close what is open
            current.append(')')
            build(current, opened, closed + 1)
            current.pop()

    build([], 0, 0)
    return result
```

## Complexity

| | |
|---|---|
| Time | Exponential in the worst case, often O(b^d) for branching factor b and depth d. Pruning changes the real runtime enormously and the worst case not at all. |
| Space | O(d) for the recursion stack, plus the size of the output |

## Variations

- Enumerate everything, which is the same job as the [subsets](subsets.md) pattern written recursively.
- Constraint satisfaction: N-Queens, Sudoku, graph coloring.
- Search on a grid, where the undo step restores the cell you marked as used.
- Partitioning a string into pieces that each satisfy a rule.
- Combination sum, where the same number may be reused, which changes what the recursive call gets passed.
- Adding memoization on top, which turns some backtracking problems into dynamic programming.

## Problems that use it

Sudoku Solver, N-Queens, Generate Parentheses, Word Search, Letter Combinations of a Phone Number, Combination Sum, Palindrome Partitioning, Permutations, Restore IP Addresses, Rat in a Maze.

## Common mistakes

- Forgetting the undo, which corrupts every later branch. If your answers contain elements that should not be there, look here first.
- Recording a reference to the working list instead of a copy of it. Every recorded answer then points at the same list, which is empty by the end.
- Pruning too little, which times out, or pruning too much, which drops valid answers. Test the pruning rule on a small case by hand.
- Passing the wrong starting index in combination problems, which either produces duplicates or wrongly forbids reuse.
- Not sorting first when the input has duplicates and the answers must be distinct.
- Recursing deeper than the stack allows on a large input.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- Recursion and backtracking in depth: [Grokking the Art of Recursion](https://www.designgurus.io/course/grokking-recursion-for-coding-interview)
