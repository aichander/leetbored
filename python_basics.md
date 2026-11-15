# Python 

A practical reference for Python syntax, data structures, common
operations and basic Big-O. 

------------------------------------------------------------------------

## 1. Basic syntax and operators

### Boolean operators

-   `and` --- both conditions must be true.
-   `or` --- at least one condition must be true.
-   `not` --- reverses a Boolean value.
-   Python uses `and` / `or`, **not** `&&` / `||`.

``` python
if x > 0 and x < 10:
    print(x)

if x == 0 or x == 1:
    print(x)
```

### Comparisons

``` python
==  # equal
!=  # not equal
>   # greater than
<   # less than
>=  # greater than or equal
<=  # less than or equal
```

`is` checks whether two references point to the same object; use `==` to
compare values.

``` python
x == y   # same value?
x is y   # same object?
x is None  # idiomatic None check
```

### `range()`

`range(start, stop, step)` includes `start` but excludes `stop`.
Defaults: `start=0`, `step=1`.

``` python
range(5)          # 0, 1, 2, 3, 4
range(1, 5)       # 1, 2, 3, 4
range(1, 5, 2)    # 1, 3
range(5, 0, -1)   # 5, 4, 3, 2, 1

for i in range(1, 5, 2):
    print(i)
```

### Math operators and helpers

``` python
7 / 2       # 3.5 — true division
7 // 2      # 3   — floor division
7 % 2       # 1   — remainder
2 ** 3      # 8   — exponent
abs(-5)     # 5
round(3.6)  # 4
```

`//` floors toward negative infinity, which matters for negative
numbers: `-7 // 2 == -4`.

``` python
import math

math.floor(3.8)  # 3
math.ceil(3.2)   # 4
math.sqrt(16)    # 4.0
math.pow(2, 3)   # 8.0
math.fmod(7, 2)  # 1.0
```

-   `%` is usually the modulo operator.
-   `math.fmod()` follows C-style remainder behavior, which differs from
    `%` for some negative values.
-   `**` is often preferable to `math.pow()` when working with integers.

### Infinity

``` python
smallest = float("inf")
largest = float("-inf")
```

Useful for initializing min/max tracking. Python integers have arbitrary
precision, so they do not have a fixed maximum integer like many other
languages.

### Truthiness

These are falsey: `False`, `None`, `0`, `0.0`, `""`, `[]`, `{}`,
`set()`.

``` python
if not arr:       # empty list?
    return []

if arr:           # non-empty?
    print(arr)
```

------------------------------------------------------------------------

## 2. Lists (dynamic arrays)

``` python
arr = []
arr = [1, 2, 3, 4]
```

Lists are mutable and can grow or shrink.

### Common operations and complexity

  Operation            Meaning                        Time
  -------------------- ------------------ ----------------
  `arr[i]`             Access by index                O(1)
  `arr[i] = x`         Reassign value                 O(1)
  `arr.append(x)`      Add to end           O(1) amortized
  `arr.pop()`          Remove last item               O(1)
  `arr.insert(i, x)`   Insert at index                O(n)
  `arr.pop(i)`         Remove at index                O(n)
  `x in arr`           Membership check               O(n)
  `len(arr)`           Number of items                O(1)
  `arr.sort()`         Sort in place            O(n log n)

### Stack (LIFO: Last In, First Out)

Use `append()` to push and `pop()` to remove the top.

``` python
stack = []
stack.append(1)
stack.append(2)
stack.append(3)

top = stack[-1]  # Peek: 3; does not remove it
value = stack.pop()  # Removes and returns 3
```

### Indexing and slicing

``` python
arr[-1]       # Last element
arr[-2]       # Second-to-last

arr[1:3]      # Index 1 up to, but excluding, index 3
arr[:3]       # First three elements
arr[2:]       # From index 2 to the end
arr[:]        # Shallow copy
arr[::-1]     # Reversed copy
arr[::2]      # Every second element
```

Slicing creates a new list; it does not modify the original.

### Unpacking

``` python
a, b, c = [1, 2, 3]

first, *middle, last = [1, 2, 3, 4]
# first = 1; middle = [2, 3]; last = 4
```

Without `*`, the number of variables must match the number of values.

### Iteration

``` python
# Values
for num in arr:
    print(num)

# Indices
for i in range(len(arr)):
    print(i)

# Index and value — very common
for i, num in enumerate(arr):
    print(i, num)
```

### `zip()`

Combines corresponding values from iterables into tuples. Stops at the
shortest iterable by default.

``` python
nums = [1, 2, 3]
letters = ["a", "b", "c"]

for num, letter in zip(nums, letters):
    print(num, letter)

pairs = list(zip(nums, letters))
```

### Reversing

``` python
arr.reverse()  # Reverse in place; returns None
arr[::-1]      # New reversed list
reversed(arr)  # Iterator; use list(reversed(arr)) to materialize
```

### Sorting

``` python
arr.sort()                    # Ascending, modifies original
arr.sort(reverse=True)        # Descending, modifies original
new_arr = sorted(arr)         # New sorted list
new_arr = sorted(arr, reverse=True)
```

Custom sorting uses `key=` and often a `lambda`:

``` python
words = ["apple", "kiwi", "banana"]

words.sort(key=lambda x: len(x))  # By length
sorted_pairs = sorted(pairs, key=lambda x: x[1])  # By second item
sorted(nums, key=lambda x: abs(x))  # By absolute value
sorted(items, key=lambda x: (x[0], x[1]))  # Multiple sort criteria
```

A `lambda` is a small anonymous function. It is also used with `map()`,
`filter()`, and heap/sort keys.

### List comprehensions

``` python
[i for i in range(5)]
[x * 2 for x in arr]
[x for x in arr if x > 0]
[x if x > 0 else 0 for x in arr]
```

Pattern: `[expression for item in iterable if condition]`.

------------------------------------------------------------------------

## 3. Strings

Strings are **immutable**: you cannot change a character in place.

``` python
s = "hello"
# s[0] = "H"  # TypeError

s = "H" + s[1:]  # Create a new string
```

### Common operations

``` python
len(s)
s.lower()
s.upper()
s.strip()
s.split()
s.replace("h", "H")
s.startswith("he")
s.endswith("lo")
s.count("l")
s.find("e")       # -1 if not found
```

### Joining strings

``` python
words = ["hello", "world"]

" ".join(words)  # "hello world"
"".join(words)   # "helloworld"
```

For repeated string building, append pieces to a list and use
`"".join(parts)` rather than repeatedly concatenating in a loop.

### Character conversion

``` python
ord("a")  # Unicode code point (97 for "a")
chr(97)   # "a"
```

### Useful patterns

``` python
s[::-1]                # Reverse
s[left:right]          # Slice; right is excluded
for i, char in enumerate(s):
    print(i, char)
```

### Complexity reminders

-   Indexing: O(1)
-   `len(s)`: O(1)
-   Searching (`char in s`): O(n)
-   Slicing: O(k), where k is the slice length
-   Concatenation: O(n) in the resulting string length

------------------------------------------------------------------------

## 4. Queues and deques

Use `collections.deque` for efficient queue operations.

``` python
from collections import deque

q = deque()
```

### Queue (FIFO: First In, First Out)

``` python
q.append(1)
q.append(2)
q.append(3)

first = q.popleft()  # Removes and returns 1
```

### Deque operations

``` python
q.append(x)       # Add right
q.appendleft(x)   # Add left
q.pop()           # Remove right
q.popleft()       # Remove left
q[0]              # First item
q[-1]             # Last item
```

Adding/removing at either end is O(1).

**Common uses:** BFS, level-order tree traversal, sliding windows, and
simulations.

Avoid `list.pop(0)` for queues: it is O(n). Use `deque.popleft()`.

------------------------------------------------------------------------

## 5. Hash sets

A set stores **unique, hashable values**. Membership, add, and remove
are O(1) on average.

``` python
myset = set()
myset = {1, 2, 3}

empty_dict = {}  # {} is an empty dictionary, NOT an empty set
```

### Operations

``` python
myset.add(4)
myset.remove(2)    # Raises KeyError if absent
myset.discard(5)   # No error if absent

2 in myset         # True or False
len(myset)
```

### Create a set

``` python
myset = set([1, 2, 3, 3])  # {1, 2, 3}
```

### Set operations

``` python
a = {1, 2, 3}
b = {3, 4, 5}

a | b  # Union: {1, 2, 3, 4, 5}
a & b  # Intersection: {3}
a - b  # In a but not b: {1, 2}
a ^ b  # In either, but not both
```

### Complexity (average case)

-   `x in myset`: O(1)
-   `add(x)`: O(1)
-   `remove(x)` / `discard(x)`: O(1)
-   Iteration: O(n)

Sets do not preserve a meaningful sorted order. Use a set when you need
fast membership or uniqueness, not positional indexing.

------------------------------------------------------------------------

## 6. Hash maps / dictionaries

A dictionary stores key-value pairs.

``` python
counts = {}
counts["apple"] = 3
```

-   Keys are unique; assigning an existing key overwrites its value.
-   Keys must be hashable (e.g. strings, numbers, tuples of hashable
    values).
-   Values can be any type.

### Common operations

``` python
counts[key] = value
counts[key]          # Get value; KeyError if key is missing
counts.get(key)      # None if missing
counts.get(key, 0)   # Default to 0 if missing
key in counts        # Checks whether key exists
len(counts)          # Number of keys
counts.pop(key)      # Remove key and return its value
```

**Important:** `key in counts` checks for a key, not a value.

``` python
if key in counts:
    print(counts[key])
```

### Add pairs

``` python
counts = {"a": 1, "b": 2}

keys = ["a", "b"]
values = [1, 2]
counts = dict(zip(keys, values))
```

### Dictionary comprehension

``` python
squares = {i: i * i for i in range(3)}
```

### Looping through dictionaries

``` python
for key in counts:           # Keys
    print(key)

for value in counts.values():  # Values
    print(value)

for key, value in counts.items():  # Both
    print(key, value)
```

### Frequency counter pattern

``` python
counts = {}

for num in nums:
    counts[num] = counts.get(num, 0) + 1
```

Or use `Counter`:

``` python
from collections import Counter

counts = Counter(nums)
counts[num]  # Count; returns 0 if missing
```

### Complexity (average case)

-   Insert/update: O(1)
-   Lookup: O(1)
-   Delete: O(1)
-   Iterate: O(n)

------------------------------------------------------------------------

## 7. Tuples

Tuples are ordered and immutable.

``` python
t = (1, 2, 3)
t[0]       # 1
# t[0] = 5  # TypeError
```

Useful for: - Returning multiple values. - Representing coordinate pairs
such as `(row, col)`. - Using immutable groups of values as dictionary
keys or set elements.

A list cannot be a dictionary key because it is mutable and unhashable.
A tuple can be a key if all its elements are hashable.

``` python
visited = set()
visited.add((row, col))
```

A one-item tuple needs a comma: `(5,)`.

------------------------------------------------------------------------

## 8. Heaps (priority queues)

Python's `heapq` is a **min-heap by default**: the smallest item is at
index `0`.

``` python
import heapq

heap = []

heapq.heappush(heap, 5)
heapq.heappush(heap, 2)
heapq.heappush(heap, 8)

smallest = heap[0]            # Peek: 2
smallest = heapq.heappop(heap)  # Remove and return 2
```

### Build a heap

``` python
nums = [5, 2, 8, 1]
heapq.heapify(nums)  # In-place; O(n)
```

### Max-heap using negative values

Negate values to simulate a max-heap:

``` python
max_heap = []

heapq.heappush(max_heap, -5)
heapq.heappush(max_heap, -2)
heapq.heappush(max_heap, -8)

largest = -max_heap[0]                 # Peek: 8
largest = -heapq.heappop(max_heap)     # Pop: 8
```

### Heap complexity

-   Peek at min (`heap[0]`): O(1)
-   Push: O(log n)
-   Pop: O(log n)
-   Heapify: O(n)

A heap is **not fully sorted**. Only the root is guaranteed to be the
smallest (or largest, using the negative-value trick).

### Common use cases

-   Top K elements.
-   Kth largest/smallest element.
-   Scheduling / priority queues.
-   Merging sorted lists.
-   Repeatedly retrieving the next smallest/largest item.

------------------------------------------------------------------------

## 9. Useful `collections` tools

``` python
from collections import Counter, defaultdict, deque
```

### `Counter`

Counts occurrences:

``` python
counts = Counter("banana")
counts["a"]       # 3
counts.most_common(2)  # Two most frequent items
```

### `defaultdict`

Automatically creates a default value for missing keys:

``` python
groups = defaultdict(list)
groups["fruit"].append("apple")

graph = defaultdict(list)
graph[1].append(2)
```

For counts:

``` python
counts = defaultdict(int)
counts["a"] += 1
```

### `deque`

See the Queues and Deques section.

------------------------------------------------------------------------

## 10. Functions and scope

Use `def` to define a function.

``` python
def add(a, b):
    return a + b
```

### Nested functions

An inner function can read variables from the outer function:

``` python
def outer(a, b):
    def inner():
        return a + b
    return inner()
```

To reassign an enclosing function's variable from a nested function, use
`nonlocal`:

``` python
def outer():
    count = 0

    def inner():
        nonlocal count
        count += 1

    inner()
    return count
```

`global` is for module-level variables; avoid relying on it when a local variable or return value is clearer.

### Multiple return values

``` python
def get_min_max(nums):
    return min(nums), max(nums)

smallest, largest = get_min_max([3, 1, 5])
```

### Common built-ins

``` python
len(nums)
sum(nums)
min(nums)
max(nums)
sorted(nums)
any(condition for x in nums)
all(condition for x in nums)
```

------------------------------------------------------------------------

## 11. Classes and objects

Commonly used for linked lists, binary trees, and design problems.

``` python
class MyClass:
    def __init__(self, nums):
        self.nums = nums
        self.size = len(nums)

    def get_size(self):
        return self.size

obj = MyClass([1, 2, 3])
print(obj.nums)
print(obj.get_size())
```

-   `class` defines a class.
-   `__init__` is the constructor/initializer.
-   `self` refers to the current instance.
-   `self.variable` stores an instance attribute.
-   `self` is passed automatically when calling an instance method.

### Class pattern

Implement methods inside classes:

``` python
class Solution:
    def someMethod(self, nums: list[int]) -> int:
        return sum(nums)
```

Do not add your own input-reading code unless the platform asks for it.

------------------------------------------------------------------------

## 12. Linked lists

A linked list is made of nodes. Each node stores a value and a reference
to the next node.

``` python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

Unlike a Python list, a linked list does not support indexing or
slicing.

``` python
node.val    # Current node's value
node.next   # Reference to next node
```

Traverse with a pointer:

``` python
current = head

while current:
    print(current.val)
    current = current.next
```

### Dummy node pattern

Useful when building a new linked list:

``` python
dummy = ListNode()
current = dummy

# Attach nodes using current.next = ...
# Move current forward using current = current.next

return dummy.next
```

The dummy node avoids special handling for the head.

### Fast and slow pointers

``` python
slow = head
fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
```

Common uses: - Find the middle of a linked list. - Detect a cycle. -
Find where two halves meet.

------------------------------------------------------------------------

## 13. Trees and graphs: common structures

### Binary tree node

``` python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

### DFS (Depth-First Search)

Recursive tree traversal:

``` python
def dfs(node):
    if not node:
        return

    # Process node
    dfs(node.left)
    dfs(node.right)
```

The order above is preorder: node, left, right.

Other orders: - Inorder: left, node, right. - Postorder: left, right,
node.

### BFS (Breadth-First Search)

Use a queue:

``` python
from collections import deque

q = deque([root])

while q:
    node = q.popleft()

    if node.left:
        q.append(node.left)
    if node.right:
        q.append(node.right)
```

### Graph adjacency list

``` python
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0],
    3: [1]
}
```

Or build one:

``` python
from collections import defaultdict

graph = defaultdict(list)

for a, b in edges:
    graph[a].append(b)
    graph[b].append(a)  # For an undirected graph
```

### Visited set

Prevents revisiting nodes and getting stuck in cycles:

``` python
visited = set()
visited.add(start)
```

Mark nodes visited when adding them to the queue/stack, not only when
removing them, to avoid adding duplicates.

------------------------------------------------------------------------

## 14. Common algorithm patterns

Recognize the pattern before coding. Ask: what information do I need to
remember as I scan the input?

### Two pointers

Use two indices, often moving inward or in the same direction.

``` python
left, right = 0, len(nums) - 1

while left < right:
    # Process nums[left] and nums[right]
    left += 1
    right -= 1
```

Common for sorted arrays, pair sums, reversing, and palindromes.

### Sliding window

Maintain a window over a contiguous section of an array/string.

Fixed-size window:

``` python
window_sum = sum(nums[:k])
best = window_sum

for right in range(k, len(nums)):
    window_sum += nums[right] - nums[right - k]
    best = max(best, window_sum)
```

Variable-size window:

``` python
left = 0

for right in range(len(nums)):
    # Add nums[right] to window

    while window_is_invalid:
        # Remove nums[left] from window
        left += 1

    # Update answer using current window
```

### Hash map lookup

Use a dictionary to remember values you've already seen.

``` python
seen = {}

for i, num in enumerate(nums):
    complement = target - num

    if complement in seen:
        return [seen[complement], i]

    seen[num] = i
```

### Binary search

Works on sorted data or a monotonic search space.

``` python
left, right = 0, len(nums) - 1

while left <= right:
    mid = left + (right - left) // 2

    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        left = mid + 1
    else:
        right = mid - 1

return -1
```

### Backtracking

Explore a choice, recurse, then undo the choice.

``` python
def backtrack(path):
    if is_complete(path):
        result.append(path[:])
        return

    for choice in choices:
        path.append(choice)
        backtrack(path)
        path.pop()  # Undo choice
```

### BFS vs DFS

-   BFS uses a queue (`deque`) and explores level by level.
-   DFS uses recursion or a stack and explores a path deeply before
    backtracking.
-   BFS is useful for shortest paths in unweighted graphs.
-   Both commonly take O(V + E) on a graph with V vertices and E edges.

------------------------------------------------------------------------

## 15. Big-O quick reference

  Operation                                     Typical time
  ----------------------------------------- ----------------
  List index access                                     O(1)
  List append / pop at end                    O(1) amortized
  List search (`in`)                                    O(n)
  List insert / delete at front or middle               O(n)
  Set membership / add / remove                 O(1) average
  Dictionary lookup / insert / delete           O(1) average
  Sorting                                         O(n log n)
  Heap push / pop                                   O(log n)
  Heap peek                                             O(1)
  Binary search                                     O(log n)
  BFS / DFS graph traversal                         O(V + E)
  String search                                         O(n)
  Build a heap with `heapify`                           O(n)

Big-O describes how runtime or memory grows as input size grows. State
whether you're discussing time or space complexity.

------------------------------------------------------------------------

## 16. Common gotchas

1.  **`sort()` returns `None`.** Use `arr.sort()` to modify in place, or
    `sorted(arr)` to get a new list.
2.  **`reverse()` returns `None`.** Use `arr.reverse()` or `arr[::-1]`.
3.  **`pop()` removes and returns an item.** `arr.pop()` removes the
    last item; `arr.pop(i)` removes index `i`.
4.  **`key in dict` checks keys.** Use `value in dict.values()` to check
    values.
5.  **`{}` is an empty dictionary.** Use `set()` for an empty set.
6.  **Strings and tuples are immutable.** Lists, sets, and dictionaries
    are mutable.
7.  **Lists are not hashable.** You cannot use a list as a dictionary
    key or set element.
8.  **`list.pop(0)` is O(n).** Use `deque.popleft()` for queues.
9.  **`heapq` is a min-heap by default.** `heapq.heappop()` removes the
    smallest item.
10. **Slicing excludes the stop index.** `arr[1:3]` includes indices 1
    and 2.
11. **`range()` excludes the stop value.**
12. **Use `==` for value comparison and `is None` for None checks.**
13. **Avoid modifying a list while iterating over it** unless you are
    deliberately managing indices.
14. **For a 2D grid, check boundaries** before accessing neighboring
    cells.
15. **For recursive DFS, define a base case** to stop at `None`, an
    invalid state, or a completed path.
16. **For BFS/DFS, track visited nodes** when cycles or repeated paths
    are possible.
17. **Watch variable shadowing.** Avoid naming a variable `list`, `sum`,
    `max`, `set`, or `dict`, because these hide Python built-ins.

------------------------------------------------------------------------

## 17. Quick imports to remember

``` python
from collections import Counter, defaultdict, deque
import heapq
import math
```