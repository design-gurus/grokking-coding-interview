# Merge K Sorted Lists

> Combine K sorted linked lists into one sorted list.

**Pattern:** [K-way Merge](../patterns/k-way-merge.md) | **Difficulty:** Medium

## The problem

You are given K linked lists, each already sorted in ascending order. Merge them into a single sorted
linked list and return its head. Some of the lists may be empty.

## Why this is a K-way Merge problem

Merging **two** sorted lists is a two pointer walk: compare the two heads and take the smaller. The
cost is linear and there is nothing to think about.

With K lists, the same idea needs the smallest of K heads at every step, and scanning all K heads each
time costs O(K) per element, so the total is O(N × K).

The fix is to stop scanning. A min-heap holding one candidate from each list surfaces the smallest in
O(1) and repairs itself in O(log K). That is the pattern: the heap holds exactly one element per list,
never more.

The cue is the input shape. "Several sorted things, merge them" is this pattern, whether the things
are lists, arrays, streams, or the rows of a sorted matrix.

## The approach

1. Push the head of every non-empty list onto a min-heap.
2. Pop the smallest. Append it to the output.
3. If the node you popped has a next node, push that node.
4. Repeat until the heap is empty.

The invariant: the heap holds the smallest not-yet-taken node from each list that still has one, so
its top is the smallest remaining node overall.

Each heap entry has to carry enough to find its successor. With linked lists the node itself is
enough. With arrays you need the list index and the position, or you cannot push the replacement.

A dummy head node in front of the output removes the special case for the first node appended.

## Complexity

| | |
|---|---|
| Time | O(N log K), where N is the total number of nodes across all lists |
| Space | O(K) for the heap |

Two alternatives are worth naming. Concatenating everything and sorting is O(N log N), which is worse
and throws away the sortedness you were given. Merging pairs of lists repeatedly, tournament style, is
also O(N log K) and uses O(1) extra space, and some interviewers prefer it.

## Edge cases to say out loud

- An empty input list of lists, and lists that are individually empty. Pushing the head of an empty
  list is the most common crash here.
- K equal to 1, where the answer is the input.
- Duplicate values across lists, which are fine but mean your heap comparison must not throw when two
  values tie. In Python, comparing tuples falls through to the second field, and nodes are not
  comparable, so include an index as a tie-breaker.
- Very long lists with K small, and very short lists with K large. Both are the same cost, and saying
  so shows you understand where the log comes from.

## Related problems

- Kth Smallest Number in M Sorted Lists, which stops after k pops instead of draining the heap.
- Kth Smallest Element in a Sorted Matrix, where each row is one of the lists.
- Smallest Number Range, which tracks the running maximum alongside the heap and stops when any list
  runs out.
- Merge Two Sorted Lists, the two-list case that needs no heap at all.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
