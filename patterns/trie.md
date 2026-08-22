# Pattern: Trie

> Store words in a tree where each edge is one character, so shared prefixes are stored once and prefix questions cost only the length of the prefix.

## What it is

A trie, also called a prefix tree, holds a set of strings. Each node has a child per possible next character, and the path from the root to a node spells a prefix. Words that begin the same way share the same path until they diverge.

That sharing is the point. Searching a hash set for every word starting with "car" means checking every word in the set. In a trie you walk three nodes and everything below you is an answer.

## Recognize it when

- The input is a dictionary, a word list, or a set of strings, and it is searched many times.
- The questions involve prefixes: autocomplete, suggestions, "starts with", spell check.
- A search may contain a wildcard that matches any single character.
- You are matching many words against one long text, rather than one word against many texts.

**Words that give it away:** "prefix", "starts with", "autocomplete", "dictionary", "word search", "suggestions", "implement a data structure that".

## How it works

```
words: car, card, cat

        root
         |
         c
         |
         a
        / \
       r*  t*
       |
       d*

* marks the end of a word
```

Insert by walking down and creating any missing child. Search by walking down and failing if a child is missing.

The end-of-word marker is what separates the two searches. A prefix search only needs the walk to succeed. A whole-word search needs the walk to succeed **and** the final node to be marked. Without the marker, "ca" would look like a stored word.

## The code template

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_word = False        # the end-of-word marker


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_word = True

    def _walk(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return None         # never create nodes during a search
            node = node.children[char]
        return node

    def search(self, word):
        node = self._walk(word)
        return node is not None and node.is_word

    def starts_with(self, prefix):
        return self._walk(prefix) is not None
```

For a wildcard search, the walk becomes a recursion: on a `.` character, try every child and return true if any branch succeeds.

## Complexity

| | |
|---|---|
| Time | O(L) to insert or search a word of length L, whatever the size of the dictionary |
| Space | O(total characters), and much less when words share prefixes |

## Variations

- Prefix search, and whole-word search using the marker.
- Wildcard search, which tries every child and backtracks.
- Collecting every word below a node, for suggestions, using a depth first walk.
- Storing a value at the end node, turning the trie into a map rather than a set.
- Scanning a text by starting a fresh walk at each position, which is how word-break and word-search-on-a-grid problems use it.
- A bitwise trie over the binary digits of numbers, used for maximum XOR problems.

## Problems that use it

Implement Trie, Design Add and Search Words Data Structure, Search Suggestions System, Word Search II, Index Pairs of a String, Extra Characters in a String, Replace Words, Longest Common Prefix.

## Common mistakes

- Leaving out the end-of-word marker, which makes every prefix of a stored word look like a stored word.
- Creating nodes during a search. A failed lookup should not grow the trie.
- Assuming lowercase English when using a 26-slot array. A dictionary keyed by character is slower but always correct.
- Claiming a trie beats a hash set for exact membership. It does not. Its advantage is prefixes, ordering, and sharing memory across similar words.
- Building a trie of the dictionary when the problem needs a trie of the text, or the reverse.
- Forgetting to undo the marker on delete, or deleting nodes that another word still needs.

## Go deeper

- This pattern's introduction in the course: [Introduction to Trie](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-trie)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- The data structure itself, from scratch: [Grokking Data Structures](https://www.designgurus.io/course/grokking-data-structures-for-coding-interviews)
