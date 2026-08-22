# Pattern: Cyclic Sort

> When the values are a known range like 1 to n, every value already knows which index it belongs at, so you can sort in O(n) with no extra memory.

## What it is

Normal sorting compares elements against each other, and no comparison sort can beat O(n log n). Cyclic Sort does not compare. It uses arithmetic: in an array holding 1 to n, the value 5 belongs at index 4, always. So walk the array and put each value where it belongs by swapping.

Once the array is in that state, any index whose value is wrong points straight at the answer. A missing number, a duplicate, and a corrupt pair are all read off in a second pass.

## Recognize it when

- The array holds integers in a contiguous range, usually 1 to n or 0 to n minus 1, and the range is stated in the constraints.
- The question is about a missing number, a duplicate number, or both at once.
- The problem asks for O(n) time and O(1) space, which rules out both sorting and a hash set.
- The array length and the value range are related. That relationship is the signal.

**Words that give it away:** "contains n numbers taken from the range 1 to n", "one number is missing", "one number appears twice", "without extra space", "in O(n) time".

## How it works

Walk the array. At each index, look at the value. If it is already home, move on. If not, swap it to where it belongs and look again at the same index, because the swap brought a new value here.

```
[3, 1, 5, 4, 2]   index 0 holds 3, which belongs at index 2, swap
[5, 1, 3, 4, 2]   index 0 holds 5, which belongs at index 4, swap
[2, 1, 3, 4, 5]   index 0 holds 2, which belongs at index 1, swap
[1, 2, 3, 4, 5]   index 0 holds 1, correct, move on
```

The pass looks like it could be quadratic because of the inner loop. But every swap puts at least one number in its final home, and a number never moves again once it is home. There are at most n swaps in total, so the whole pass is O(n).

## The code template

```python
def cyclic_sort(nums):
    """Values are 1 to n. After this, nums[i] should be i + 1."""
    i = 0
    while i < len(nums):
        home = nums[i] - 1                          # where nums[i] belongs
        if 0 <= home < len(nums) and nums[i] != nums[home]:
            nums[i], nums[home] = nums[home], nums[i]
        else:
            i += 1                                  # only advance when settled
    return nums


def find_missing_number(nums):
    cyclic_sort(nums)
    for i in range(len(nums)):
        if nums[i] != i + 1:
            return i + 1
    return len(nums) + 1
```

The swap condition compares `nums[i] != nums[home]`, not `i != home`. Comparing values instead of indices is what stops an infinite loop when the array contains duplicates.

## Complexity

| | |
|---|---|
| Time | O(n) |
| Space | O(1) |

## Variations

- Find the one missing number.
- Find all missing numbers in the range.
- Find the duplicate, or all duplicates.
- Find the corrupt pair, which is the one duplicate and the one missing number together.
- Find the smallest missing positive number, where values outside 1 to n are ignored rather than placed.
- First K missing positive numbers.

## Problems that use it

Cyclic Sort, Find the Missing Number, Find all Missing Numbers, Find the Duplicate Number, Find all Duplicate Numbers, Find the Corrupt Pair, Find the Smallest Missing Positive Number, Find the First K Missing Positive Numbers.

## Common mistakes

- Advancing the index after a swap. The swap brought a new value to the current index, and that value has not been placed yet.
- Using `while i != home` as the swap condition, which loops forever when two equal values both want the same home.
- Forgetting the bounds check. In "smallest missing positive", the array can hold negatives and huge values that have no home in the array at all, and they should simply be skipped.
- Using this pattern on arbitrary integers. Without a known dense range, use a [hash map](hash-maps.md) instead.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
