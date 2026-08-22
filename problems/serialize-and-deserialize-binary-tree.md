# Serialize and Deserialize Binary Tree

> Turn a binary tree into a string, and rebuild the identical tree from that string.

**Pattern:** [Serialize and Deserialize](../patterns/serialize-and-deserialize.md) | **Difficulty:** Hard

## The problem

Design two functions. One takes the root of a binary tree and returns a string. The other takes that
string and returns a tree identical in shape and values to the original. The format is yours to choose.

## Why this is a Serialize and Deserialize problem

The words encode, decode, or "send it over a network" name the pattern directly. What makes it hard is
choosing a format, not walking the tree.

Here is the trap. A pre-order walk of the values alone is not enough. The values 1, 2, 3 could have
come from several different trees, so the string does not describe one tree, and the rebuild is
guesswork.

What is missing is the **shape**, and the cheapest way to record shape is to write a marker for every
missing child. Then the string describes the tree exactly, and the rebuild is a straight recursion
over the tokens.

This is also why serializing a **binary search tree** is easier: the ordering rule already tells the
reader where each value belongs, so no markers are needed.

## The approach

To serialize, walk the tree in pre-order. Write the value at each node, and write a marker at every
null child. Join the tokens with a delimiter.

To deserialize, read the tokens as a stream and rebuild in the same order.

1. Take the next token.
2. If it is the marker, return null.
3. Otherwise make a node with that value.
4. Build its left child from the rest of the stream, then its right child.
5. Return the node.

The invariant: each recursive call consumes exactly the tokens belonging to its own subtree, and
leaves the stream positioned at the start of the next one.

```
      1
     / \
    2   3
       / \
      4   5

serialized:  1,2,#,#,3,4,#,#,5,#,#
```

The two walks have to agree. A pre-order writer needs a pre-order reader, and the order of the two
recursive calls in the reader is what reproduces the shape.

## Complexity

| | |
|---|---|
| Time | O(n) in each direction |
| Space | O(n) for the string, plus O(h) for the recursion, where h is the height |

## Edge cases to say out loud

- The empty tree, in both directions.
- A tree shaped like a list, which is the deep-recursion case.
- **Multi-digit and negative values.** The value 12 must not be read as a 1 and a 2, which is why the
  delimiter matters and why a fixed-width format is fragile.
- **A delimiter that can appear in the data.** If node values were strings, splitting on commas would
  break. The robust format writes the length before each value. This is exactly the trap in the
  related Encode and Decode Strings problem.
- Consuming more than one token per call, or the same token twice. Use an iterator or an index that
  only moves forward.

## Related problems

- Serialize and Deserialize BST, where the ordering removes the need for markers.
- Serialize and Deserialize N-ary Tree, where each node also needs a child count, since the number of
  children is no longer fixed.
- Encode and Decode Strings, where the whole problem is the delimiter question above.
- Construct Binary Tree from Preorder and Inorder Traversal, the same rebuild from two walks instead
  of one walk with markers.
- [Clone Graph](clone-graph.md), the same job done through a map rather than through text.

## The full solution

Worked solution in six languages, with runnable tests and an editor to attempt it yourself first:
[Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
