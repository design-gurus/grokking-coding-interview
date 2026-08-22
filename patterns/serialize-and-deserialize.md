# Pattern: Serialize and Deserialize

> Flatten a structure into a string, and rebuild it from that string. The format has to record enough shape that the rebuild is unambiguous.

## What it is

Serializing a tree means writing it out as text. Deserializing means reading that text back into the same tree. The tricky part is not the traversal, it is choosing a format where only one tree could have produced the output.

A pre-order walk of a binary tree is not enough on its own. The values 1, 2, 3 could describe several different trees. Adding a marker for every missing child fixes that, because the markers record the shape.

## Recognize it when

- The problem says encode, decode, serialize, deserialize, or "send this over a network".
- You are asked to design a codec, or to store a structure and restore it later.
- A tree has to be rebuilt from one or two traversals.
- Two structures have to be compared, and turning each into a canonical string is the simplest way to do it.

**Words that give it away:** "serialize", "encode and decode", "reconstruct the tree", "from preorder and inorder", "codec", "save and restore".

## How it works

```
tree:        1
            / \
           2   3
              / \
             4   5

pre-order with null markers:
1,2,#,#,3,4,#,#,5,#,#

reading back: take 1, build its left from the stream, then its right.
The # markers tell the reader exactly where a branch stops.
```

Deserializing is a recursion over a stream. Each call takes the next token. If it is a marker, return null. Otherwise make a node, build its left child from the rest of the stream, then its right child. The order in which the recursive calls consume the stream is what rebuilds the shape.

A binary search tree needs no markers, because the values themselves say where each node belongs. A general graph needs identifiers instead, since a node can be reached from several places.

## The code template

```python
def serialize(root):
    parts = []

    def walk(node):
        if not node:
            parts.append('#')            # the marker that records the shape
            return
        parts.append(str(node.val))
        walk(node.left)
        walk(node.right)

    walk(root)
    return ','.join(parts)


def deserialize(data):
    tokens = iter(data.split(','))

    def build():
        token = next(tokens)             # consume exactly one token per call
        if token == '#':
            return None
        node = TreeNode(int(token))
        node.left = build()
        node.right = build()
        return node

    return build()
```

## Complexity

| | |
|---|---|
| Time | O(n) in each direction |
| Space | O(n) for the string, plus O(h) for the recursion |

## Variations

- Pre-order with null markers, which is the standard answer.
- Level order with markers, using a queue, which matches how these strings are usually printed.
- A binary search tree, where the ordering removes the need for markers.
- Trees where a node can have any number of children, which need a child count or a closing marker.
- Graphs, where each node gets an identifier and the edges are written as pairs.
- Encode and decode a list of strings, where the trap is that any delimiter can appear inside the data, so you write the length before each string instead.

## Problems that use it

Serialize and Deserialize Binary Tree, Serialize and Deserialize BST, Serialize and Deserialize N-ary Tree, Encode and Decode Strings, Verify Preorder Serialization of a Binary Tree, Construct Binary Tree from Preorder and Inorder Traversal, Find Duplicate Subtrees.

## Common mistakes

- Leaving out the null markers, which makes the shape ambiguous and the rebuild wrong.
- Consuming more than one token per recursive call, or consuming the same token twice. Use an iterator or an index that only ever moves forward.
- Picking a delimiter that can appear in the data. If a node can hold a comma, splitting on commas breaks. Length-prefixing is the safe format.
- Not handling the empty tree in both directions.
- In a graph, forgetting the visited map, which recurses forever on a cycle. That overlaps with the [clone](clone.md) pattern.
- Assuming values are single digits. The value 12 must not be read as a 1 and a 2.

## Go deeper

- This pattern's introduction in the course: [Introduction to Serialize and Deserialize Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-serialize-and-deserialize-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
