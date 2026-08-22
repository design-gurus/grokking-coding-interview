# Python vs Java vs JavaScript: The Same Twenty Operations

Knowing the pattern is not enough if you lose four minutes remembering how to build a max-heap. This
is the same operation written three ways, plus the traps each language sets.

## Collections

| Operation | Python | Java | JavaScript |
|---|---|---|---|
| Sort ascending | `arr.sort()` | `Arrays.sort(arr)` | `arr.sort((a, b) => a - b)` |
| Sort by a key | `arr.sort(key=lambda x: x[1])` | `list.sort(Comparator.comparingInt(x -> x[1]))` | `arr.sort((a, b) => a[1] - b[1])` |
| Sort descending | `arr.sort(reverse=True)` | `Arrays.sort(arr, Collections.reverseOrder())` | `arr.sort((a, b) => b - a)` |
| Reverse | `arr.reverse()` | `Collections.reverse(list)` | `arr.reverse()` |
| Copy a list | `arr[:]` or `list(arr)` | `arr.clone()` | `[...arr]` |
| Last element | `arr[-1]` | `arr[arr.length - 1]` | `arr[arr.length - 1]` |
| 2D array of zeros | `[[0] * c for _ in range(r)]` | `new int[r][c]` | `Array.from({length: r}, () => new Array(c).fill(0))` |

## Maps, sets, and counting

| Operation | Python | Java | JavaScript |
|---|---|---|---|
| Empty map | `d = {}` | `Map<K,V> m = new HashMap<>()` | `const m = new Map()` |
| Get with a default | `d.get(k, 0)` | `m.getOrDefault(k, 0)` | `m.get(k) ?? 0` |
| Increment a count | `d[k] = d.get(k, 0) + 1` | `m.merge(k, 1, Integer::sum)` | `m.set(k, (m.get(k) ?? 0) + 1)` |
| Count everything at once | `Counter(arr)` | stream, or a loop | a loop |
| Auto-defaulting map | `defaultdict(int)`, `defaultdict(list)` | `m.computeIfAbsent(k, x -> new ArrayList<>())` | a loop |
| Empty set | `s = set()` | `Set<T> s = new HashSet<>()` | `const s = new Set()` |
| Is it there | `k in s` | `s.contains(k)` | `s.has(k)` |
| Iterate keys | `for k in d:` | `for (K k : m.keySet())` | `for (const k of m.keys())` |
| Iterate pairs | `for k, v in d.items():` | `for (var e : m.entrySet())` | `for (const [k, v] of m)` |

## Heaps, deques, and ordered structures

| Operation | Python | Java | JavaScript |
|---|---|---|---|
| Min-heap | `heapq.heappush(h, x)`, `heapq.heappop(h)` | `PriorityQueue<Integer> pq = new PriorityQueue<>()` | **no built-in**, write one or use a sorted array |
| Max-heap | push `-x`, negate on the way out | `new PriorityQueue<>(Collections.reverseOrder())` | **no built-in** |
| Heap by a key | push tuples, `(cost, item)` | `new PriorityQueue<>((a, b) -> a[0] - b[0])` | **no built-in** |
| Build a heap from a list | `heapq.heapify(arr)`, O(n) | `new PriorityQueue<>(list)` | n/a |
| Queue | `collections.deque`, `popleft()` | `ArrayDeque`, `poll()` | array with `push`, `shift` (`shift` is O(n)) |
| Deque, both ends | `deque`, `appendleft` and `pop` | `ArrayDeque`, `offerFirst` and `pollLast` | array, or write a ring buffer |
| Ordered set or map | **no built-in**, `sortedcontainers` | `TreeMap`, `TreeSet` | **no built-in** |
| Nearest key at or below | `SortedList` plus `bisect` | `tm.floorKey(x)` | n/a |
| Binary search a sorted list | `bisect_left(arr, x)` | `Arrays.binarySearch(arr, x)` | write the loop |

**The two gaps worth naming out loud.** JavaScript has no heap and no ordered set. Python has no
ordered set. If a problem needs one, say so and describe what you would use, because an interviewer
who knows the language is watching for exactly that.

Note that `Arrays.binarySearch` in Java returns `-(insertion point) - 1` when the value is absent, not
-1. Python's `bisect_left` returns the insertion point directly.

## Strings

| Operation | Python | Java | JavaScript |
|---|---|---|---|
| Build a string in a loop | append to a list, then `''.join(parts)` | `StringBuilder sb; sb.append(c)` | push to an array, then `parts.join('')` |
| Character at a position | `s[i]` | `s.charAt(i)` | `s[i]` or `s.charAt(i)` |
| Substring | `s[a:b]` | `s.substring(a, b)` | `s.slice(a, b)` |
| Split | `s.split(',')` | `s.split(",")` | `s.split(',')` |
| Letter to 0-25 index | `ord(c) - ord('a')` | `c - 'a'` | `c.charCodeAt(0) - 97` |
| Reverse a string | `s[::-1]` | `new StringBuilder(s).reverse().toString()` | `[...s].reverse().join('')` |
| Sorted characters | `''.join(sorted(s))` | `char[] a = s.toCharArray(); Arrays.sort(a)` | `[...s].sort().join('')` |

Strings are immutable in all three. Concatenating in a loop is O(n²) in every one of them. Use the
builder in the first row.

## Numbers, and where they bite

| | Python | Java | JavaScript |
|---|---|---|---|
| Largest useful value | `float('inf')` | `Integer.MAX_VALUE` | `Infinity` |
| Smallest useful value | `float('-inf')` | `Integer.MIN_VALUE` | `-Infinity` |
| Integer division | `7 // 2` is 3, `-7 // 2` is **-4** | `-7 / 2` is **-3** | `Math.trunc(-7 / 2)` is -3, `Math.floor` is -4 |
| Modulo of a negative | `-7 % 3` is **2** | `-7 % 3` is **-1** | `-7 % 3` is **-1** |
| Overflow | none, integers are unbounded | 32-bit `int` overflows silently | safe to 2^53, **bitwise operations truncate to 32 bits** |
| Midpoint without overflow | `(lo + hi) // 2` is safe | `lo + (hi - lo) / 2` | `lo + Math.trunc((hi - lo) / 2)` |

Two of these decide real answers.

**The modulo sign.** Grouping by remainder, as in "subarrays divisible by k", breaks in Java and
JavaScript on negative numbers. The fix is `((x % k) + k) % k`. Python needs nothing.

**Java overflow.** `(lo + hi) / 2` on a large array overflows and goes negative, which is the oldest
binary search bug there is. Write `lo + (hi - lo) / 2`.

## Traps per language

**Python**

- The recursion limit is about 1,000 frames. A deep tree or a large grid needs
  `sys.setrecursionlimit`, or an iterative rewrite.
- `[[0] * c] * r` makes r references to **one** row. Changing one changes them all. Use the list
  comprehension in the table.
- `heapq` is min-only. Negate for a max-heap, and negate consistently on the way out.
- Comparing tuples falls through to the next field, so a heap of `(count, someObject)` throws when two
  counts tie. Add an index as a tie-breaker.

**Java**

- `==` on `Integer` compares references, not values, outside the cached range of -128 to 127. Use
  `.equals` or unbox to `int`.
- `int[]` cannot go into a `List<Integer>` without boxing, and `Arrays.asList(intArray)` produces a
  list with one element.
- `Arrays.sort` on primitives is a dual-pivot quicksort: not stable, O(n²) worst case. On objects it
  is Timsort: stable, O(n log n) worst case.
- A comparator written as `(a, b) -> a - b` overflows on extreme values. Use `Integer.compare(a, b)`.

**JavaScript**

- `arr.sort()` with no comparator sorts **as strings**, so `[10, 9, 1]` becomes `[1, 10, 9]`. Always
  pass a comparator.
- `shift()` and `unshift()` are O(n). An array is a fine stack and a bad queue.
- Plain objects turn every key into a string, so `1` and `"1"` collide. `Map` keeps the type.
- Bitwise operators convert to 32-bit signed integers, so they silently break above 2^31.

## Which language to use

Use the one you will actually interview in. If you have a real choice, Python is the shortest for
most patterns and has the heap and deque built in. Java is the most explicit about types, which helps
in tree and graph problems, and it has `TreeMap`, which is the one structure the other two lack.
JavaScript is fine for everything except heaps and ordered sets, and those two gaps cost real minutes.

## Go deeper

- [The 41 patterns](../patterns/), each with a Python template
- [Complexity of every structure](complexity-cheat-sheet.md)
- Worked solutions in six languages, side by side: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
