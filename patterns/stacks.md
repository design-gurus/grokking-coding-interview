# Pattern: Stacks

> A stack always hands back the most recent unfinished item, which is exactly what matching, nesting, and undo problems ask for.

## What it is

A stack stores items in last-in, first-out order. You push items on and pop them off the same end, so the item you get back is always the newest one still waiting.

That property is the pattern. Whenever the answer depends on "the most recent thing I have not handled yet", a stack holds that thing at its top, ready to read in O(1).

## Recognize it when

- The input has nesting or matching: brackets, tags, folders, function calls, expressions.
- The problem mentions undo, history, back, or "the most recent".
- You need to process items in the reverse of the order you met them.
- You are about to write recursion and the interviewer asks for an iterative version. A stack is what the language itself uses to implement recursion.

**Words that give it away:** "valid parentheses", "nested", "matching", "undo", "evaluate the expression", "simplify the path", "reverse", "most recent".

## How it works

Push each item as you meet it. When the current item resolves whatever is on top, pop it and handle the pair. Anything still on the stack at the end was never resolved, which is usually itself the answer to "is this input valid".

```
input: ( [ ] )

(     push (        stack: (
[     push [        stack: ( [
]     top is [, matching, pop      stack: (
)     top is (, matching, pop      stack: empty
end   stack empty, so the input is balanced
```

## The code template

```python
def is_balanced(s):
    pairs = {')': '(', ']': '[', '}': '{'}
    stack = []
    for char in s:
        if char in '([{':
            stack.append(char)
        elif char in pairs:
            if not stack or stack.pop() != pairs[char]:
                return False        # closed the wrong thing, or nothing at all
    return not stack                # anything left over was never closed


def evaluate_postfix(tokens):
    stack = []
    for token in tokens:
        if token.lstrip('-').isdigit():
            stack.append(int(token))
        else:
            right = stack.pop()
            left = stack.pop()      # order matters for minus and divide
            stack.append(apply(token, left, right))
    return stack.pop()
```

## Complexity

| | |
|---|---|
| Time | O(n). Each item is pushed once and popped at most once. |
| Space | O(n) |

## Variations

- Bracket and tag matching.
- Expression evaluation, in postfix or with two stacks for infix.
- Path simplification, where `..` pops the previous folder.
- Undo and redo history, using two stacks.
- Two stacks used to imitate a queue, or two queues used to imitate a stack.
- A stack that also tracks its own minimum, by pushing pairs instead of values.
- A stack replacing recursion in an iterative depth first search.
- Keeping the stack ordered turns it into a [monotonic stack](monotonic-stack.md), which answers next-greater questions.

## Problems that use it

Balanced Parentheses, Reverse a String, Decimal to Binary Conversion, Simplify Path, Remove All Adjacent Duplicates, Min Stack, Evaluate Reverse Polish Notation, Implement Queue using Stacks, Basic Calculator.

## Common mistakes

- Popping without checking that the stack is not empty, which crashes on input like `)))`.
- Forgetting the final emptiness check, which is what catches unclosed brackets like `(((`.
- Popping the operands of a minus or a divide in the wrong order.
- Reaching for a stack when the problem wants the oldest item rather than the newest. That is a queue.
- Using a list as a stack in Python is correct and fast, since append and pop both work at the end. Using a list as a queue is not, because removing from the front is O(n).

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- The data structure itself, from scratch: [Grokking Data Structures](https://www.designgurus.io/course/grokking-data-structures-for-coding-interviews)
