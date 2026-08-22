# Tasks Scheduling

> Given tasks and their prerequisites, decide whether all of them can be finished.

**Pattern:** [Topological Sort](../patterns/topological-sort.md) | **Difficulty:** Medium

## The problem

You are given a number of tasks and a list of prerequisite pairs. Each pair says that one task must be
finished before another can start. Return whether it is possible to finish every task.

This problem is also known as Course Schedule.

## Why this is a Topological Sort problem

The words "prerequisite" and "before" describe **directed** edges between tasks, so the input is a
directed graph written as a list of pairs.

The question sounds like it asks for an order, but read it again: it asks whether an order **exists**.
Those are the same question. A valid order exists exactly when the graph has no cycle, because a cycle
means a group of tasks that each wait on another one in the group, and none of them can ever start.

Kahn's algorithm answers both at once. It builds an order by repeatedly taking a task with nothing
left blocking it. If it places every task, an order exists. If it runs out of unblocked tasks early,
whatever is left forms a cycle.

## The approach

1. Build an adjacency list, from each task to the tasks that depend on it.
2. Count in-degrees: for each task, how many prerequisites it still has.
3. Put every task with an in-degree of zero into a queue. Those are the tasks that can start now.
4. While the queue is not empty:
   - Pop a task and count it as finished.
   - For each task that depended on it, decrease that task's in-degree by one. If it reaches zero,
     push it.
5. Return whether the number finished equals the total number of tasks.

The invariant: a task's in-degree is the number of its prerequisites that have not been finished yet.

## Complexity

| | |
|---|---|
| Time | O(V + E), where V is the number of tasks and E the number of prerequisite pairs |
| Space | O(V + E) for the graph, the in-degree array, and the queue |

## Edge cases to say out loud

- **Edge direction.** "To take course A you must first take course B" means the edge runs from B to A.
  Getting this backwards produces a confident wrong answer, not a crash, so state your direction out
  loud before coding.
- Tasks with no prerequisites at all, and tasks nothing depends on. Both belong in the answer, and the
  first group is what seeds the queue.
- No prerequisites given, where every task is immediately ready.
- A task that is its own prerequisite, which is a cycle of length one.
- Duplicate pairs, which can inflate an in-degree and leave a task blocked forever. Deduplicate if the
  input allows repeats.
- Building the graph undirected by mistake, which makes every in-degree at least one, so the queue
  starts empty and nothing is ever scheduled.

## Related problems

- Tasks Scheduling Order, which returns the order instead of a yes or no.
- All Tasks Scheduling Orders, which uses [backtracking](../patterns/backtracking.md) whenever more
  than one task is unblocked. The number of orders can be exponential.
- Alien Dictionary, where the edges have to be derived first by comparing adjacent words.
- Minimum Height Trees, which peels leaves the same way from the outside in.
- Sequence Reconstruction, which asks whether the valid order is unique, meaning the queue never holds
  more than one task at a time.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
