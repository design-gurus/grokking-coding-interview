# Pattern: Bitwise XOR

> XOR cancels equal pairs, so everything that appears twice disappears and only the odd one out is left.

## What it is

XOR has three properties that do all the work: `a ^ a = 0`, `a ^ 0 = a`, and the order of operations does not matter. Put together, XOR-ing a whole array makes every value that appears an even number of times vanish, and leaves behind whatever did not pair up.

That is an O(1) space answer to a question you would otherwise solve with a hash map.

## Recognize it when

- Every value appears twice except one, or except two.
- The problem says O(1) space and the input is integers.
- A number is missing from a complete range, and you want to avoid both sorting and a set.
- The problem is explicitly about bits: flipping, complementing, counting set bits, swapping without a temporary variable.

**Words that give it away:** "appears twice", "single number", "every element appears exactly", "without extra memory", "bitwise", "complement", "flip the bits".

## How it works

```
[4, 1, 2, 1, 2]

4 ^ 1 ^ 2 ^ 1 ^ 2
= 4 ^ (1 ^ 1) ^ (2 ^ 2)     order does not matter
= 4 ^ 0 ^ 0
= 4
```

For two unique numbers, the total XOR is the XOR of just those two. Any bit that is set in that result is a bit where the two numbers differ. Pick one such bit, split the array into the numbers that have it and the numbers that do not, and each group now holds exactly one unique number.

The expression `x & -x` isolates the lowest set bit, which is the standard way to pick that dividing bit.

## The code template

```python
def single_number(nums):
    result = 0
    for num in nums:
        result ^= num          # pairs cancel, the loner survives
    return result


def two_single_numbers(nums):
    total = 0
    for num in nums:
        total ^= num           # this is a ^ b for the two unique values

    dividing_bit = total & -total      # a bit where a and b differ
    first = second = 0
    for num in nums:
        if num & dividing_bit:
            first ^= num
        else:
            second ^= num
    return [first, second]


def missing_number(nums):
    """Values are 0 to n with one missing."""
    result = len(nums)
    for i, num in enumerate(nums):
        result ^= i ^ num      # every present value cancels its own index
    return result
```

## Complexity

| | |
|---|---|
| Time | O(n) |
| Space | O(1) |

## Variations

- One single number among pairs.
- Two single numbers among pairs, using a dividing bit.
- One single number where the others appear three times, which XOR alone cannot do. Count the set bits at each position and take the count modulo 3.
- Missing number, by XOR-ing the values against the full range.
- Complement of a base-10 number, which flips only the bits the number actually uses.
- Flipping and inverting an image, which is XOR with 1 per pixel.

## Problems that use it

Single Number, Two Single Numbers, Single Number II, Complement of Base 10 Number, Flip and Invert an Image, Missing Number, Find the Difference.

## Common mistakes

- Applying XOR when values appear three times. The cancellation rule needs pairs. Three of a kind needs bit counting instead.
- Flipping all 32 bits for a complement, when the problem only wants the bits the number occupies. Build a mask of ones up to the highest set bit first.
- Forgetting how the language handles negative numbers and integer width. Python integers are unbounded, Java has a 32-bit signed int, and the difference shows up in shift and complement problems.
- Reaching for bit tricks when a hash map is clearer and the constraints allow it. Say the O(1) space version exists, then write whichever one you can get right.
- Not tracing a small example on paper. Bit problems are hard to debug by staring at them.

## Go deeper

- This pattern's introduction in the course: [Introduction to Bitwise XOR Pattern](https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-bitwise-xor-pattern)
- The problems that use it, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
