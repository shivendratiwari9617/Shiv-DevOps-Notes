# Python DSA ? Senior Interview Preparation
### Product-Based Company Edition

> **How to use:** every section opens with the mental model, then each sub-topic gives a 2-line technical explanation, a Pythonic example, complexity, and the product-company insight interviewers actually probe. All code is runnable on Python 3.10+.

---

## 1. Complexity Analysis and Problem-Solving Foundations

Complexity analysis is the language interviewers use to decide whether your solution scales. At senior level you are expected to state complexity **before** coding, justify the bound, and recognise when constant factors or Python-specific overheads dominate the asymptotics. Getting this wrong is the fastest way to fail a product-company loop.

### Time complexity ? best, average, worst case
Worst case is the default contract; average case matters when the structure amortises (hash tables); best case is rarely interesting except to show early-exit behaviour.
```python
def find(nums, target):          # best O(1) - first element
    for i, n in enumerate(nums): # average O(n/2) -> O(n)
        if n == target:          # worst O(n) - absent or last
            return i
    return -1
```
**Complexity:** time O(n), space O(1).
**Insight:** say "worst case O(n)" unless asked otherwise. Quoting the best case unprompted reads as evasive.

### Big-O, Big-Theta, Big-Omega
Big-O is an upper bound, Big-Omega a lower bound, Big-Theta a tight bound. Interviews say "O" but almost always mean Theta.
```python
# binary search: O(log n) upper, Omega(1) best, Theta(log n) worst-case tight
```
**Insight:** you don't need the formalism, but knowing O(n^2) is *technically* true for an O(n) algorithm shows rigour if a follow-up probes it.

### Space complexity ? auxiliary space and recursion stack
Auxiliary space excludes the input. Recursion costs O(depth) stack frames even when no data structure is allocated ? a point candidates routinely miss.
```python
def rev_iter(s):  return s[::-1]          # O(n) aux (new string)
def rev_rec(s):   return s if len(s) <= 1 else rev_rec(s[1:]) + s[0]
# rev_rec: O(n) stack AND O(n^2) time - slicing copies each call
```
**Insight:** in Python, recursion depth is capped near 1000. Always state "this recurses O(n) deep, so for n > ~1000 I'd convert to iterative."

### Amortised complexity basics
An expensive operation spread over many cheap ones. `list.append` is O(1) amortised: the list over-allocates, so occasional O(n) resizes average out.
```python
xs = []
for i in range(10**6):
    xs.append(i)      # amortised O(1); a few resizes are O(n)
```
**Insight:** `append` is amortised O(1) but `insert(0, x)` is always O(n). Knowing the difference is a genuine signal.

### Input size reasoning and constraint-based approach selection
Constraints tell you the target complexity before you design. Map n to the admissible bound.

| n | Admissible | Typical approach |
|---|---|---|
| <= 10 | O(n!) / O(2^n) | permutations, brute-force backtracking |
| <= 20 | O(2^n) | bitmask DP, subsets |
| <= 500 | O(n^3) | Floyd-Warshall, 3 nested loops |
| <= 5,000 | O(n^2) | 2-D DP, pairwise |
| <= 10^6 | O(n log n) | sort, heap, binary search |
| > 10^7 | O(n) / O(log n) | hashing, two pointers, math |

**Insight:** "n is up to 10^5, so I need O(n log n) or better ? that rules out the nested loop" is exactly the sentence interviewers listen for.

### Dry run and edge-case thinking
Walk one small input by hand before coding, then enumerate edge cases explicitly.
```python
# Standard edge-case checklist:
# []  [x]  [x, x]  already sorted  reverse sorted
# all duplicates  negatives/zero  n == target boundary  overflow (N/A in Python)
```
**Insight:** naming edge cases *before* writing code is a senior marker; discovering them only when the interviewer supplies a failing input is not.

### Pattern recognition and brute force to optimised
Most interview problems map to ~15 patterns. State the brute force, give its complexity, then optimise ? never jump straight to the clever answer without establishing the baseline.
```python
# "contiguous subarray" + "sum/max"      -> sliding window / Kadane
# "sorted array" + "pair"                -> two pointers
# "count occurrences"                    -> dict / Counter
# "top k"                                -> heap
# "next greater"                         -> monotonic stack
# "shortest path, unweighted"            -> BFS
# "all combinations"                     -> backtracking
# "overlapping subproblems"              -> DP
```
**Insight:** verbalising the pattern ("this is a variable-size sliding window") demonstrates transferable skill, not memorisation of one problem.

### Trade-offs ? time vs space, readability vs micro-optimisation
Hash maps buy time with memory; in-place algorithms save memory and destroy the input. Readability wins in interviews unless asked to optimise.
```python
nums = [5, 3, 5, 1]
seen = set(nums)          # time-optimised: O(n) time, O(n) space
nums.sort()               # space-optimised: O(n log n) time, O(1) extra
print(seen, nums)
```
**Insight:** say which you chose **and** the condition that would flip the decision ? that is the 4-level answer.

### Writing interview-friendly solutions
Clear names, small helpers, early returns, explicit edge handling, and a stated complexity. Product companies grade code quality, not just correctness.
```python
def max_subarray_sum(nums: list[int]) -> int:
    if not nums:
        raise ValueError("nums must be non-empty")
    best = current = nums[0]
    for n in nums[1:]:
        current = max(n, current + n)
        best = max(best, current)
    return best
# Time O(n), Space O(1)
```
**Insight:** a correct solution with cryptic names scores lower than a clean one. Reviewability is part of the bar.

---

## 2. Arrays, Lists, and String Fundamentals

Python's `list` is a dynamic array of pointers, not a contiguous block of values ? which drives every performance characteristic that matters. Strings are immutable, so any "modification" allocates. Most array/string interview failures in Python are hidden quadratic behaviour from slicing or concatenation.

### Arrays vs Python lists
A Python list stores pointers to objects, so it is heterogeneous and memory-heavy versus a C array; `array.array` and `bytearray` give compact typed storage.
```python
import sys, array
xs = [1, 2, 3]                       # pointers -> int objects
ar = array.array("i", [1, 2, 3])     # packed 32-bit ints
print(sys.getsizeof(xs), sys.getsizeof(ar))
```
**Complexity:** index O(1), append amortised O(1), insert/delete at i is O(n).
**Insight:** mention that lists over-allocate ? that is *why* append is amortised O(1).

### List operations ? indexing, slicing, append, pop, insert, remove
`pop()` from the end is O(1); `pop(0)`, `insert(0, x)` and `remove(v)` are O(n) because elements shift.
```python
xs = [1, 2, 3, 4]
xs.append(5)      # O(1) amortised
xs.pop()          # O(1)
xs.pop(0)         # O(n)  <-- use deque if frequent
xs.insert(0, 0)   # O(n)
xs.remove(3)      # O(n) - searches then shifts
sub = xs[1:3]     # O(k) - COPIES
```
**Insight:** `arr[i:]` inside a loop is the single most common hidden-quadratic bug in Python interviews. Pass indices instead.

### String operations ? indexing, slicing, concatenation, join, split
Strings are immutable: `s += x` in a loop is O(n^2). Build a list and `join` once for O(n).
```python
words = ["a", "b", "c"]
out = ""
for w in words:       # BAD - O(n^2)
    out += w
out = "".join(words)  # GOOD - O(n)
print(out)
print("a,b,,c".split(","))   # ['a','b','','c']
print("a  b".split())        # ['a','b'] - whitespace runs, empties dropped
```
**Insight:** CPython has a refcount-1 optimisation that sometimes hides the quadratic cost. Say you don't rely on an implementation detail.

### Traversal patterns
Forward, reverse, index+value, and pairwise traversal each have an idiomatic form; avoid `range(len(x))` when `enumerate` works.
```python
from itertools import pairwise
xs = [1, 2, 3]
print([(i, v) for i, v in enumerate(xs)])
print(list(reversed(xs)))
print(list(zip(xs, xs[1:])))      # pairs - xs[1:] COPIES
print(list(pairwise(xs)))         # 3.10+, no copy
print([i for i in range(len(xs) - 1, -1, -1)])
```
**Insight:** `zip(xs, xs[1:])` copies; `pairwise` doesn't. Small detail, real signal.

### Frequency counting
`Counter` is the idiomatic frequency map and gives `most_common` for free.
```python
from collections import Counter
freq = Counter("mississippi")
print(freq.most_common(2))     # [('i', 4), ('s', 4)]
```
**Complexity:** O(n) build, O(1) lookup.
**Insight:** `Counter` missing-key reads return 0 **without inserting** ? unlike `defaultdict(int)`.

### In-place modification and reversal patterns
In-place means O(1) auxiliary space, usually via two pointers swapping inward.
```python
def reverse_in_place(xs):
    i, j = 0, len(xs) - 1
    while i < j:
        xs[i], xs[j] = xs[j], xs[i]   # tuple swap, no temp
        i += 1; j -= 1
```
**Complexity:** O(n) time, O(1) space.
**Insight:** `xs[::-1]` is a copy (O(n) space); `xs.reverse()` is in-place. Interviewers ask which you used and why.

### Prefix and suffix thinking
Precompute cumulative values so range queries become O(1) subtractions.
```python
from itertools import accumulate
nums = [2, 4, 6, 8]
pre = list(accumulate(nums, initial=0))   # [0,2,6,12,20]
range_sum = pre[3] - pre[1]               # sum(nums[1:3]) == 10
```
**Complexity:** O(n) build, O(1) per query.
**Insight:** `initial=0` removes every off-by-one in the index arithmetic ? use it.

### Common themes ? duplicates, missing values, rotations, palindromes, anagrams, subarrays
Each maps to a known tool: set for duplicates, math/XOR for missing, reversal or slicing for rotation, two pointers for palindrome, Counter for anagram, window/prefix for subarray.
```python
from collections import Counter
nums, k, s, a, b = [1, 2, 4], 1, "racecar", "listen", "silent"
print(len(set(nums)) < len(nums))                      # duplicates? O(n)
print(len(nums) * (len(nums) + 1) // 2 - sum(nums))    # missing in 1..n, O(1) space
print(nums[k:] + nums[:k])                             # rotate, O(n)
print(s == s[::-1])                                    # palindrome, O(n)
print(Counter(a) == Counter(b))                        # anagram, O(n)
```
**Insight:** for "missing number", offering both the sum formula and the XOR trick (overflow-safe in other languages, irrelevant in Python due to big ints) shows breadth.

---

## 3. Hashing, Sets, and Dictionary-Based Thinking

Hash-based lookup is the single highest-leverage optimisation in interviews: it converts O(n) scans into O(1) checks and turns most O(n^2) brute forces into O(n). In Python, `dict` and `set` are extremely well optimised, so "use a dict" is correct far more often than any clever alternative.

### Hash map and hash set concepts
Both store entries in buckets indexed by `hash(key)`; average O(1) lookup, worst O(n) under pathological collisions. Keys must be hashable and hash-stable.
```python
seen = set()        # membership only
index = {}          # key -> value
print(hash((1, 2)))        # tuples hashable
# hash([1, 2])             # TypeError: unhashable type: 'list'
```
**Insight:** the real requirement is a **stable** hash for the key's lifetime, not immutability ? mutating a hashed field corrupts lookup silently.

### Python dict ? insertion, lookup, deletion, iteration
All average O(1); iteration follows **insertion order** (guaranteed since 3.7).
```python
d = {}
d["a"] = 1            # insert O(1)
d.get("b", 0)         # lookup with default, no insertion
d.setdefault("c", []) # insert-if-absent, returns value
del d["a"]            # O(1)
for k, v in d.items(): ...
```
**Insight:** two dicts compare equal regardless of insertion order; order is preserved, not significant.

### Python set ? membership, uniqueness, set operations
Set algebra replaces nested loops and reads better.
```python
a, b = {1, 2, 3}, {3, 4}
a & b    # {3}  intersection
a | b    # union
a - b    # difference
a ^ b    # symmetric difference
```
**Complexity:** each O(min(len(a), len(b))) to O(len(a)+len(b)).
**Insight:** `x in some_list` is O(n); `x in some_set` is O(1). Converting is the most common single-line optimisation in interviews.

### Frequency maps, index mapping, seen/unseen patterns
Three canonical uses: count occurrences, remember where you saw something, and detect repeats in one pass.
```python
# index mapping - Two Sum in one pass
def two_sum(nums, target):
    pos = {}
    for i, n in enumerate(nums):
        if target - n in pos:
            return [pos[target - n], i]
        pos[n] = i
    return []
# Time O(n), Space O(n)
```
**Insight:** check *before* inserting, or `x + x == target` falsely matches a single element.

### Grouping by keys, counting and bucket ideas
`defaultdict(list)` groups in one pass; bucketing by a derived key can beat sorting.
```python
from collections import defaultdict
def group_anagrams(words):
    groups = defaultdict(list)
    for w in words:
        groups[tuple(sorted(w))].append(w)   # tuple = hashable key
    return list(groups.values())
# Time O(n * k log k), Space O(n * k)
```
**Insight:** a 26-length count tuple as the key drops it to O(n*k). Mentioning that unprompted is a strong signal.

### Hashing-based optimisation and collisions
Collisions are resolved by open addressing in CPython; average O(1) holds because the table resizes at ~2/3 load.
```python
# bucket sort by frequency: O(n) top-k, beating O(n log n) sort
def top_k(nums, k):
    freq = Counter(nums)
    buckets = [[] for _ in range(len(nums) + 1)]
    for val, c in freq.items():
        buckets[c].append(val)
    out = []
    for c in range(len(buckets) - 1, 0, -1):
        for v in buckets[c]:
            out.append(v)
            if len(out) == k:
                return out
    return out
```
**Insight:** offer three solutions for top-k ? `Counter.most_common` (O(n log n)), `heapq.nlargest` (O(n log k)), bucket sort (O(n)) ? and justify the pick.

### Common applications
Two-sum style, duplicate detection, longest consecutive sequence, character frequency.
```python
def longest_consecutive(nums):
    s = set(nums)
    best = 0
    for n in s:
        if n - 1 not in s:              # only start at a sequence head
            length = 1
            while n + length in s:
                length += 1
            best = max(best, length)
    return best
# Time O(n), Space O(n)
```
**Insight:** without the `n - 1 not in s` guard this is O(n^2) while *looking* linear ? a classic senior trap.

---

## 4. Two Pointers, Sliding Window, Prefix Sum, and Interval Patterns

These four patterns cover a large share of array and string interview questions. They all exploit structure ? sortedness, contiguity, or monotonicity ? to replace nested loops with a single pass. Recognising which applies is worth more than memorising any individual solution.

### Two pointers ? inward, fast/slow, merge-style
Inward pointers work on sorted data; fast/slow detect cycles and midpoints; merge-style walks two sorted sequences.
```python
def two_sum_sorted(nums, target):        # inward
    i, j = 0, len(nums) - 1
    while i < j:
        s = nums[i] + nums[j]
        if s == target: return [i, j]
        if s < target:  i += 1
        else:           j -= 1
    return []
# Time O(n), Space O(1)
```
**Insight:** be able to *prove* the move is safe ? if the sum is too small, no pair using `nums[i]` can work, so advancing `i` cannot skip the answer.

### Sliding window ? fixed and variable size
Fixed window slides by adding one and removing one; variable window grows on the right and shrinks on the left while a constraint is violated.
```python
def longest_unique(s: str) -> int:       # variable window
    last, left, best = {}, 0, 0
    for right, ch in enumerate(s):
        if ch in last and last[ch] >= left:
            left = last[ch] + 1          # max() guard via the >= check
        last[ch] = right
        best = max(best, right - left + 1)
    return best
# Time O(n), Space O(min(n, charset))
```
**Insight:** forgetting the `last[ch] >= left` guard lets stale indices move the window backwards ? the defining bug of this pattern.

### Prefix sum ? 1-D, range queries, prefix frequency
Precompute cumulative sums so any range sum is one subtraction; combine with a hash map for "count subarrays with sum k".
```python
def subarray_sum_k(nums, k):
    counts = {0: 1}          # empty prefix seeds the count
    total = found = 0
    for n in nums:
        total += n
        found += counts.get(total - k, 0)
        counts[total] = counts.get(total, 0) + 1
    return found
# Time O(n), Space O(n)
```
**Insight:** the `{0: 1}` seed is what makes subarrays starting at index 0 count. Omitting it is the standard off-by-one here.

### Difference array basics
Record deltas at range boundaries, then prefix-sum once to materialise all range updates in O(n) total.
```python
def apply_ranges(n, updates):            # updates: (l, r, val) inclusive
    diff = [0] * (n + 1)
    for l, r, val in updates:
        diff[l] += val
        diff[r + 1] -= val
    return list(accumulate(diff[:n]))
# Time O(n + m), Space O(n)
```
**Insight:** turns m range updates from O(n*m) into O(n+m) ? the right answer for "add val to every element in range, q times".

### Kadane-style subarray thinking
At each index decide: extend the running subarray or restart from the current element.
```python
def max_subarray(nums):
    best = cur = nums[0]
    for n in nums[1:]:
        cur = max(n, cur + n)
        best = max(best, cur)
    return best
# Time O(n), Space O(1)
```
**Insight:** handle the all-negative case ? initialising `best = 0` silently returns 0 instead of the largest negative.

### Interval problems ? merging, overlapping, meeting rooms
Sort by start to merge; use a min-heap of end times (or a sweep line) to count concurrent intervals.
```python
def merge(intervals):
    intervals.sort(key=lambda x: x[0])
    out = []
    for s, e in intervals:
        if out and s <= out[-1][1]:          # touching counts as overlap
            out[-1][1] = max(out[-1][1], e)
        else:
            out.append([s, e])
    return out
# Time O(n log n), Space O(n)

import heapq
def min_meeting_rooms(intervals):
    intervals.sort(key=lambda x: x[0])
    ends = []
    for s, e in intervals:
        if ends and ends[0] <= s:
            heapq.heappop(ends)              # a room freed up
        heapq.heappush(ends, e)
    return len(ends)
# Time O(n log n), Space O(n)
```
**Insight:** always ask whether `[1,2]` and `[2,3]` count as overlapping. Stating the assumption beats guessing.

### Sweep line basics
Convert intervals into +1/-1 events, sort, and scan to track a running count.
```python
def max_concurrent(intervals):
    events = []
    for s, e in intervals:
        events.append((s, 1))
        events.append((e, -1))     # end before start at equal time
    events.sort()
    cur = best = 0
    for _, delta in events:
        cur += delta
        best = max(best, cur)
    return best
# Time O(n log n), Space O(n)
```
**Insight:** the tie-break at equal timestamps decides whether touching intervals overlap ? `-1` sorts before `+1`, treating them as non-overlapping.

### Pattern recognition cues
Sorted input suggests two pointers or binary search; "contiguous" suggests a window or prefix sum; "subarray sum equals k" suggests prefix + hash map; "substring with constraint" suggests a variable window.

---

## 5. Stack, Queue, Deque, and Monotonic Patterns

Stacks and queues turn nested-loop scans into linear passes by remembering just enough history. In Python, use `list` as a stack and **`collections.deque` as a queue** ? using a list as a queue is the classic O(n) mistake. Monotonic structures are the pattern behind nearly every "next greater / previous smaller / window extreme" question.

### Stack basics ? push, pop, peek
LIFO; a Python list gives O(1) `append`/`pop` at the end.
```python
st = []
st.append(1)      # push O(1)
top = st[-1]      # peek O(1)
st.pop()          # pop  O(1)
```
**Insight:** always guard `st[-1]` with `if st:` ? an empty-stack peek is the most common crash in parentheses problems.

### Queue basics ? FIFO behaviour
FIFO; `deque.popleft()` is O(1) while `list.pop(0)` is O(n).
```python
from collections import deque
q = deque([1, 2, 3])
q.append(4)       # enqueue O(1)
q.popleft()       # dequeue O(1)
```
**Insight:** `queue.Queue` is for thread-safe producer/consumer, not algorithms ? using it in a BFS signals confusion.

### Deque basics ? append/pop both ends
Double-ended queue with O(1) operations at both ends and an optional `maxlen` ring buffer.
```python
from collections import deque
d = deque(maxlen=3)
for x in [1, 2, 3, 4]:
    d.append(x)       # auto-evicts from the left
print(list(d))        # [2, 3, 4]
d.appendleft(0); d.pop(); d.popleft()
print(list(d))
```
**Complexity:** O(1) at both ends; O(n) random access.
**Insight:** `maxlen` gives you a last-N buffer for free ? a neat answer to "track the last k elements".

### Python tools ? list as stack, collections.deque
Use `list` when you only touch one end; `deque` when you touch both.
```python
from collections import deque
stack = []                 # list: O(1) at the end
queue = deque()            # deque: O(1) at both ends
stack.append(1); queue.append(1); queue.appendleft(0)
print(stack, list(queue))
```
**Insight:** quantify it ? for n = 10^5, `list.pop(0)` in a loop is ~10^10 operations; `deque.popleft()` is ~10^5.

### Parentheses and expression problems
Push openers, pop and match on closers, and require an empty stack at the end.
```python
def is_valid(s: str) -> bool:
    pairs = {")": "(", "]": "[", "}": "{"}
    st = []
    for ch in s:
        if ch in pairs:
            if not st or st.pop() != pairs[ch]:
                return False
        else:
            st.append(ch)
    return not st
# Time O(n), Space O(n)
```
**Insight:** the final `return not st` catches unclosed openers ? omitting it passes `"(("`.

### Next greater / next smaller element
Maintain a stack of indices whose answer is still pending; pop when the current element resolves them.
```python
def next_greater(nums):
    res = [-1] * len(nums)
    st = []                               # indices, values decreasing
    for i, n in enumerate(nums):
        while st and nums[st[-1]] < n:
            res[st.pop()] = n
        st.append(i)
    return res
# Time O(n) amortised - each index pushed and popped once
```
**Insight:** justify the O(n): the inner `while` looks quadratic but each index enters and leaves the stack exactly once.

### Monotonic stack
A stack kept sorted (increasing or decreasing) so the top is always the nearest useful neighbour.
```python
def daily_temperatures(temps):
    res = [0] * len(temps)
    st = []
    for i, t in enumerate(temps):
        while st and temps[st[-1]] < t:
            j = st.pop()
            res[j] = i - j
        st.append(i)
    return res
# Time O(n), Space O(n)
```
**Insight:** histogram area, trapping rain water, stock span and next-greater are all this one pattern.

### Monotonic queue and sliding window maximum
A deque holding indices in decreasing value order gives the window max in O(1) per step.
```python
def max_sliding_window(nums, k):
    dq, out = deque(), []
    for i, n in enumerate(nums):
        while dq and nums[dq[-1]] <= n:
            dq.pop()                       # drop dominated values
        dq.append(i)
        if dq[0] <= i - k:
            dq.popleft()                   # drop out-of-window index
        if i >= k - 1:
            out.append(nums[dq[0]])
    return out
# Time O(n), Space O(k)
```
**Insight:** the heap alternative is O(n log n). Offering both and choosing the deque is the senior answer.

### Span problems and simulation
Span = how far back a condition holds; simulation = replay operations with a stack/queue.
```python
def stock_span(prices):
    res, st = [], []                      # st: indices, decreasing prices
    for i, p in enumerate(prices):
        while st and prices[st[-1]] <= p:
            st.pop()
        res.append(i - st[-1] if st else i + 1)
        st.append(i)
    return res
# Time O(n), Space O(n)
```

### BFS queue usage basics
BFS always uses a `deque`; snapshot `len(queue)` to process level by level.
```python
def bfs_levels(root):
    if not root: return []
    q, out = deque([root]), []
    while q:
        level = []
        for _ in range(len(q)):           # snapshot = one level
            node = q.popleft()
            level.append(node.val)
            q.extend(c for c in (node.left, node.right) if c)
        out.append(level)
    return out
# Time O(n), Space O(width)
```

---

## 6. Linked Lists

Linked lists are rare in production Python but common in interviews because they test pointer discipline and in-place reasoning. Python has no built-in linked list, so you define the node yourself. The dummy-head and fast/slow-pointer idioms solve most variants.

### Node structure, singly and doubly linked lists
A singly linked node holds a value and `next`; doubly also holds `prev`, enabling O(1) deletion given the node.
```python
class Node:
    __slots__ = ("val", "next")           # memory win for many nodes
    def __init__(self, val, nxt=None):
        self.val, self.next = val, nxt
```
**Insight:** mentioning `__slots__` here is a genuine Python-specific optimisation for millions of nodes.

### Traversal, insertion, deletion
Traversal is O(n); insert/delete is O(1) **given the previous node**, otherwise O(n) to find it.
```python
def traverse(head):
    cur = head
    while cur:
        yield cur.val
        cur = cur.next
```
**Insight:** a generator keeps it lazy and composable ? a Pythonic touch over building a list.

### Reversal
Three-pointer walk, rebinding `next` as you go; Python's tuple swap makes it one line.
```python
def reverse(head):
    prev, cur = None, head
    while cur:
        cur.next, prev, cur = prev, cur, cur.next   # evaluated right-to-left
    return prev
# Time O(n), Space O(1)
```
**Insight:** the recursive version is O(n) stack and blows up past ~1000 nodes ? say so.

### Middle of linked list and cycle detection
Fast/slow pointers: fast moves two steps, slow one; they meet at the middle, or inside a cycle.
```python
def middle(head):
    slow = fast = head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
    return slow

def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow is fast:
            return True
    return False
# Time O(n), Space O(1)
```
**Insight:** offer the O(n)-space `set` of visited nodes first ? it is more Pythonic and often the better production answer ? then give Floyd's as the O(1)-space optimisation.

### Merge two linked lists and intersection
Merge with a dummy head; find intersection by equalising lengths or by the two-pass pointer swap.
```python
def merge(a, b):
    dummy = tail = Node(0)
    while a and b:
        if a.val <= b.val: tail.next, a = a, a.next
        else:              tail.next, b = b, b.next
        tail = tail.next
    tail.next = a or b
    return dummy.next
# Time O(n+m), Space O(1)
```

### Dummy node pattern and in-place manipulation
A dummy head removes every "is this the first node?" special case.
```python
def remove_nth_from_end(head, n):
    dummy = Node(0, head)
    fast = slow = dummy
    for _ in range(n): fast = fast.next
    while fast.next:
        fast, slow = fast.next, slow.next
    slow.next = slow.next.next            # unlink
    return dummy.next
# Time O(n) one pass, Space O(1)
```
**Insight:** the dummy is what makes deleting the head identical to deleting any other node.

---

## 7. Recursion and Backtracking

Recursion expresses tree-shaped search naturally; backtracking is recursion plus undoing state. Python caps recursion near 1000 frames and has no tail-call optimisation, so depth awareness is mandatory ? converting to an explicit stack is a senior-level move interviewers look for.

### Recursion fundamentals ? base case, recursive case, call stack
Every recursion needs a base case that terminates and a step that strictly reduces the problem.
```python
import sys
print(sys.getrecursionlimit())            # 1000 by default

def fact(n):
    if n <= 1: return 1                   # base case
    return n * fact(n - 1)                # recursive case
# Time O(n), Space O(n) stack
```
**Insight:** "this recurses O(n) deep so it fails past ~1000; I'd make it iterative" is the sentence that separates levels.

### Recursive tree of calls
Drawing the call tree reveals both complexity and repeated subproblems ? the trigger for memoisation.
```python
# fib(5) -> fib(4) + fib(3); fib(3) computed twice -> O(2^n)
# overlapping subproblems => memoise => O(n)
```

### Backtracking framework ? choose, explore, unchoose
Add a choice, recurse, then remove it so the next branch starts clean.
```python
def subsets(nums):
    out, path = [], []
    def dfs(i):
        if i == len(nums):
            out.append(path[:])           # COPY - path is mutated
            return
        dfs(i + 1)                        # exclude
        path.append(nums[i])              # choose
        dfs(i + 1)                        # explore
        path.pop()                        # unchoose
    dfs(0)
    return out
# Time O(n * 2^n), Space O(n) recursion
```
**Insight:** `path[:]` is mandatory ? appending `path` itself stores one aliased list mutated to empty by the end. Classic bug.

### Subsets, permutations, combinations
Subsets branch include/exclude; permutations track used elements; combinations fix a start index to avoid duplicates.
```python
from itertools import permutations, combinations
list(permutations([1,2,3], 2))    # ordered pairs
list(combinations([1,2,3], 2))    # unordered pairs

def combine(n, k):
    out, path = [], []
    def dfs(start):
        if len(path) == k:
            out.append(path[:]); return
        for v in range(start, n + 1):
            path.append(v)
            dfs(v + 1)                    # start = v+1 prevents reuse/dupes
            path.pop()
    dfs(1)
    return out
```
**Insight:** knowing `itertools` has these is good; being able to hand-roll the backtracking is what is being tested.

### Combination sum and N-Queens style search
Allow reuse by recursing with the same index; prune with sorted input and early exit.
```python
def combination_sum(nums, target):
    nums.sort()
    out, path = [], []
    def dfs(start, remain):
        if remain == 0:
            out.append(path[:]); return
        for i in range(start, len(nums)):
            if nums[i] > remain: break    # pruning - sorted
            path.append(nums[i])
            dfs(i, remain - nums[i])      # i, not i+1 -> reuse allowed
            path.pop()
    dfs(0, target)
    return out
```

### Grid path exploration and visited-state handling
Mark visited before recursing and either restore it (path search) or leave it (component counting).
```python
def num_islands(grid):
    if not grid: return 0
    rows, cols, count = len(grid), len(grid[0]), 0
    def sink(r, c):
        stack = [(r, c)]                  # explicit stack - avoids depth limit
        while stack:
            i, j = stack.pop()
            if 0 <= i < rows and 0 <= j < cols and grid[i][j] == "1":
                grid[i][j] = "0"          # mark visited in place
                stack.extend([(i+1,j), (i-1,j), (i,j+1), (i,j-1)])
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1":
                count += 1; sink(r, c)
    return count
# Time O(rows*cols), Space O(rows*cols) worst
```
**Insight:** a 1000x1000 grid blows the stack with recursive DFS. Using an explicit stack unprompted is a strong signal.

### Pruning and Python recursion considerations
Prune impossible branches early; prefer iteration when depth can exceed ~1000. Raising the limit risks a hard interpreter crash.
```python
import sys
print(sys.getrecursionlimit())        # 1000
# sys.setrecursionlimit(10000)        # last resort - crash risk, not a real fix
```
**Insight:** say that raising the limit treats the symptom; converting to an explicit stack is the fix.

---

## 8. Trees and Binary Search Trees

Trees test recursive decomposition: solve for children, combine upward. BSTs add an ordering invariant that turns O(n) search into O(h). Most tree questions are a traversal plus a small amount of state returned from children.

### Tree terminology and binary tree structure
Root has no parent, leaves have no children, height is the longest root-to-leaf edge count, depth is distance from the root.
```python
class TreeNode:
    __slots__ = ("val", "left", "right")
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right
```

### Tree traversals ? preorder, inorder, postorder, level order
Preorder = node, left, right (copy/serialise); inorder = left, node, right (**sorted for a BST**); postorder = left, right, node (delete/aggregate); level order = BFS.
```python
def inorder(root):                        # generator - lazy, early exit
    if root:
        yield from inorder(root.left)
        yield root.val
        yield from inorder(root.right)
# Time O(n), Space O(h)
```
**Insight:** a generator traversal lets you short-circuit (e.g. k-th smallest) without visiting the whole tree.

### DFS vs BFS on trees
DFS uses O(h) space and suits path/depth questions; BFS uses O(width) and suits shortest-path or level questions.
```python
# DFS iterative preorder
def preorder_iter(root):
    out, st = [], [root] if root else []
    while st:
        node = st.pop()
        out.append(node.val)
        if node.right: st.append(node.right)   # right first -> left pops first
        if node.left:  st.append(node.left)
    return out
```
**Insight:** for a skewed tree DFS is O(n) space; for a complete tree BFS is O(n/2) space. Pick by shape, not habit.

### Tree height and diameter
Height recurses to children and adds one; diameter is the max of (left height + right height) across all nodes.
```python
def diameter(root):
    best = 0
    def height(node):
        nonlocal best
        if not node: return 0
        l, r = height(node.left), height(node.right)
        best = max(best, l + r)           # path through this node
        return 1 + max(l, r)
    height(root)
    return best
# Time O(n), Space O(h)
```
**Insight:** returning height while updating a `nonlocal` best is the standard "state returned from children" idiom.

### Binary Search Tree ? property, search, insert, delete
Left subtree < node < right subtree, **recursively**. Operations are O(h): O(log n) balanced, O(n) degenerate.
```python
def search(root, target):
    while root:
        if root.val == target: return root
        root = root.left if target < root.val else root.right
    return None
# Time O(h), Space O(1) iterative
```

### Validate BST
Carry (low, high) bounds down ? checking only immediate children is the classic wrong answer.
```python
def is_valid_bst(root, low=float("-inf"), high=float("inf")):
    if not root: return True
    if not (low < root.val < high): return False
    return (is_valid_bst(root.left,  low, root.val) and
            is_valid_bst(root.right, root.val, high))
# Time O(n), Space O(h)
```
**Insight:** the alternative proof is that an inorder traversal must be strictly increasing ? a generator makes that elegant and early-exiting.

### Lowest common ancestor
In a BST, descend while both targets are on the same side; in a general tree, recurse and return the node where the two sides split.
```python
def lca_bst(root, p, q):
    while root:
        if p.val < root.val > q.val:   root = root.left
        elif p.val > root.val < q.val: root = root.right
        else: return root
# Time O(h), Space O(1)

def lca(root, p, q):
    if not root or root is p or root is q: return root
    l, r = lca(root.left, p, q), lca(root.right, p, q)
    return root if l and r else (l or r)
# Time O(n), Space O(h)
```

### Path sum and tree construction from traversals
Path sums carry a running total down; construction uses preorder/postorder for the root and inorder for the split.
```python
def build(preorder, inorder):
    idx = {v: i for i, v in enumerate(inorder)}   # O(1) root lookup
    it = iter(preorder)
    def helper(lo, hi):
        if lo > hi: return None
        val = next(it)
        node = TreeNode(val)
        mid = idx[val]
        node.left  = helper(lo, mid - 1)
        node.right = helper(mid + 1, hi)
        return node
    return helper(0, len(inorder) - 1)
# Time O(n), Space O(n)
```
**Insight:** the index map turns an O(n^2) build into O(n) ? a frequent follow-up.

### Serialize and deserialize basics
Preorder with explicit null markers round-trips any binary tree.
```python
def serialize(root):
    out = []
    def dfs(n):
        if not n: out.append("#"); return
        out.append(str(n.val)); dfs(n.left); dfs(n.right)
    dfs(root)
    return ",".join(out)

def deserialize(data):
    it = iter(data.split(","))
    def dfs():
        v = next(it)
        if v == "#": return None
        node = TreeNode(int(v))
        node.left, node.right = dfs(), dfs()
        return node
    return dfs()
# Time O(n), Space O(n)
```
**Insight:** if asked "why not `pickle`?" ? security on untrusted input, version coupling, and no cross-language interop.

---

## 9. Heap, Priority Queue, Trie, and Advanced Core Structures

Heaps answer "top k" and "running extreme" in O(log n) per operation. Tries index strings by prefix. Union-Find tracks connectivity incrementally. All three appear regularly in product-company rounds and all three have Python-specific gotchas.

### Heap basics ? min-heap, max-heap concept
A binary heap keeps the smallest (min-heap) at index 0 with O(log n) push/pop and O(1) peek. **Python only has a min-heap** ? negate values for a max-heap.
```python
import heapq
h = []
heapq.heappush(h, 3); heapq.heappush(h, 1)
heapq.heappop(h)          # 1  - smallest
# max-heap via negation
mx = [-x for x in [3, 1, 4]]
heapq.heapify(mx)         # O(n)
-heapq.heappop(mx)        # 4
```
**Insight:** `heapify` is O(n), not O(n log n) ? worth stating.

### Python heapq ? push, pop, heapify, nlargest, merge
`nlargest`/`nsmallest` accept a `key`; `merge` lazily merges sorted iterables.
```python
import heapq
nums, pts = [5, 1, 8, 3], [(3, 3), (1, 1), (2, 2)]
print(heapq.nlargest(3, nums))                                      # O(n log k)
print(heapq.nsmallest(2, pts, key=lambda p: p[0]**2 + p[1]**2))
print(list(heapq.merge([1, 4], [2, 3])))                            # lazy k-way merge
```
**Insight:** for top-k, `nlargest` is O(n log k) versus `sorted(...)[:k]` at O(n log n). For k close to n, just sort.

### Top K problems
Keep a size-k min-heap: push, and pop when it exceeds k.
```python
def top_k_frequent(nums, k):
    freq = Counter(nums)
    return [v for v, _ in heapq.nlargest(k, freq.items(), key=lambda kv: kv[1])]
# Time O(n log k), Space O(n)
```
**Insight:** for mixed sort directions (count desc, word asc) you cannot negate a string ? use `(-count, word)` in a min-heap.

### K-way merge and running median
Merge k sorted lists with a heap of heads; track a running median with two heaps.
```python
def merge_k(lists):
    h = [(l[0], i, 0) for i, l in enumerate(lists) if l]
    heapq.heapify(h)
    out = []
    while h:
        val, li, idx = heapq.heappop(h)
        out.append(val)
        if idx + 1 < len(lists[li]):
            heapq.heappush(h, (lists[li][idx + 1], li, idx + 1))
    return out
# Time O(n log k), Space O(k)

class MedianFinder:
    def __init__(self):
        self.lo, self.hi = [], []          # lo = max-heap (negated), hi = min-heap
    def add(self, num):
        heapq.heappush(self.lo, -num)
        heapq.heappush(self.hi, -heapq.heappop(self.lo))   # balance
        if len(self.hi) > len(self.lo):
            heapq.heappush(self.lo, -heapq.heappop(self.hi))
    def median(self):
        if len(self.lo) > len(self.hi): return -self.lo[0]
        return (-self.lo[0] + self.hi[0]) / 2
# add O(log n), median O(1)
```
**Insight:** the `(val, index, ...)` tuple prevents Python from comparing non-orderable payloads on ties ? a real `TypeError` source.

### Priority-based scheduling
Push `(priority, counter, task)` with a monotonic counter for stable FIFO ordering within a priority.
```python
import heapq, itertools
counter = itertools.count()
pq = []
heapq.heappush(pq, (2, next(counter), "low"))
heapq.heappush(pq, (1, next(counter), "high"))
print(heapq.heappop(pq)[2])      # 'high'
```
**Insight:** the counter guarantees stability **and** guarantees the payload is never compared.

### Trie basics ? insert, search, prefix search
A prefix tree where each node maps a character to a child; operations are O(len(word)) regardless of dictionary size.
```python
class Trie:
    def __init__(self):
        self.root = {}
    def insert(self, word):
        node = self.root
        for ch in word:
            node = node.setdefault(ch, {})
        node["$"] = True                   # terminal marker
    def search(self, word):
        node = self._walk(word)
        return node is not None and "$" in node
    def starts_with(self, prefix):
        return self._walk(prefix) is not None
    def _walk(self, s):
        node = self.root
        for ch in s:
            if ch not in node: return None
            node = node[ch]
        return node
# insert/search O(L), Space O(total chars)
```
**Insight:** nested dicts are the Pythonic trie ? simpler than a node class and usually faster.

### Trie use cases ? dictionary search, autocomplete
Prefix search collects all terminal words under a node via DFS.
```python
def autocomplete(trie, prefix):
    node = trie._walk(prefix)
    if node is None: return []
    out = []
    def dfs(n, path):
        if "$" in n: out.append(prefix + path)
        for ch, child in n.items():
            if ch != "$": dfs(child, path + ch)
    dfs(node, "")
    return out
```
**Insight:** a hash set answers exact lookup in O(L) too ? the trie wins **only** for prefix queries. Say so.

### Disjoint Set Union / Union-Find
Tracks connected components with near-O(1) amortised `find`/`union` using path compression and union by rank.
```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]   # path compression
            x = self.parent[x]
        return x
    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb: return False                          # already connected
        if self.rank[ra] < self.rank[rb]: ra, rb = rb, ra
        self.parent[rb] = ra
        if self.rank[ra] == self.rank[rb]: self.rank[ra] += 1
        return True
# Amortised O(alpha(n)) ~ O(1), Space O(n)
```
**Insight:** `union` returning `False` for an already-connected pair is exactly the cycle-detection primitive ? and the core of Kruskal's MST.

### Use cases ? connected components, cycle detection
Count components by counting distinct roots; detect a cycle when `union` returns False.
```python
def count_components(n, edges):
    dsu = DSU(n)
    comps = n
    for a, b in edges:
        if dsu.union(a, b):
            comps -= 1
    return comps
# Time O(E * alpha(n)), Space O(n)
```
**Insight:** DSU handles **incremental** connectivity (edges arriving over time); BFS/DFS requires the full graph up front. That is the deciding factor.

---

## 10. Graphs and Graph Traversal Patterns

Graphs generalise trees: any node may connect to any other, so cycles are possible and **visited tracking is mandatory**. Most interview graph questions are BFS, DFS, or topological sort in disguise ? including grid problems, which are implicit graphs. Choosing the representation correctly is the first decision.

### Graph representation ? adjacency list vs matrix
An adjacency list is O(V+E) space and suits sparse graphs; a matrix is O(V^2) but gives O(1) edge lookup.
```python
from collections import defaultdict, deque
edges, n = [(0, 1), (1, 2)], 3
graph = defaultdict(list)
for u, v in edges:
    graph[u].append(v)
    graph[v].append(u)        # omit for directed
matrix = [[0] * n for _ in range(n)]   # NOT [[0]*n]*n - that aliases rows
print(dict(graph), matrix)
```
**Insight:** `[[0]*n]*n` creates n references to **one** row. This is the single most common grid bug in Python.

### Directed vs undirected, weighted vs unweighted
Undirected edges are added both ways; weights change the shortest-path algorithm from BFS to Dijkstra.
```python
from collections import defaultdict
graph = defaultdict(list)
u, v, w = 0, 1, 5
graph[u].append((v, w))       # weighted adjacency
print(dict(graph))
```
**Insight:** unweighted shortest path = BFS, O(V+E). Reaching for Dijkstra there is over-engineering and interviewers notice.

### BFS and DFS
BFS explores level by level with a queue (shortest path in unweighted graphs); DFS goes deep with a stack or recursion.
```python
def bfs(graph, start):
    visited, q, order = {start}, deque([start]), []
    while q:
        node = q.popleft()
        order.append(node)
        for nxt in graph[node]:
            if nxt not in visited:
                visited.add(nxt)      # mark on ENQUEUE, not dequeue
                q.append(nxt)
    return order

def dfs(graph, start):
    visited, st, order = set(), [start], []
    while st:
        node = st.pop()
        if node in visited: continue
        visited.add(node); order.append(node)
        st.extend(graph[node])
    return order
# Both: Time O(V+E), Space O(V)
```
**Insight:** marking visited on dequeue instead of enqueue lets duplicates into the queue ? a subtle performance regression.

### Visited tracking and connected components
A set of visited nodes prevents infinite loops; each unvisited start yields one component.
```python
def components(graph, nodes):
    seen, count = set(), 0
    for n in nodes:
        if n not in seen:
            count += 1
            stack = [n]
            while stack:
                cur = stack.pop()
                if cur in seen: continue
                seen.add(cur)
                stack.extend(graph[cur])
    return count
# Time O(V+E), Space O(V)
```

### Cycle detection
In an undirected graph, a visited neighbour that is not the parent implies a cycle; in a directed graph, use three colours (white/grey/black) or DSU.
```python
def has_cycle_directed(graph, nodes):
    WHITE, GREY, BLACK = 0, 1, 2
    color = {n: WHITE for n in nodes}
    def dfs(u):
        color[u] = GREY
        for v in graph[u]:
            if color[v] == GREY: return True      # back edge
            if color[v] == WHITE and dfs(v): return True
        color[u] = BLACK
        return False
    return any(dfs(n) for n in nodes if color[n] == WHITE)
# Time O(V+E), Space O(V)
```
**Insight:** grey = "on the current recursion path". A black node is finished and safe ? confusing the two is the usual bug.

### Topological sort and the in-degree method
Linear ordering of a DAG where every edge points forward. Kahn's BFS repeatedly removes in-degree-0 nodes.
```python
def topo_sort(n, prerequisites):
    graph = defaultdict(list)
    indeg = [0] * n
    for course, pre in prerequisites:
        graph[pre].append(course)
        indeg[course] += 1
    q = deque(i for i in range(n) if indeg[i] == 0)
    order = []
    while q:
        node = q.popleft()
        order.append(node)
        for nxt in graph[node]:
            indeg[nxt] -= 1
            if indeg[nxt] == 0:
                q.append(nxt)
    return order if len(order) == n else []       # short => cycle
# Time O(V+E), Space O(V+E)
```
**Insight:** `len(order) != n` **is** the cycle check ? you get detection free, no second pass.

### Shortest path ? BFS unweighted, Dijkstra basics
BFS gives shortest hops; Dijkstra uses a min-heap for non-negative weights.
```python
import heapq
def dijkstra(graph, start):
    dist = {start: 0}
    pq = [(0, start)]
    while pq:
        d, node = heapq.heappop(pq)
        if d > dist.get(node, float("inf")): continue   # stale entry
        for nxt, w in graph[node]:
            nd = d + w
            if nd < dist.get(nxt, float("inf")):
                dist[nxt] = nd
                heapq.heappush(pq, (nd, nxt))
    return dist
# Time O((V+E) log V), Space O(V)
```
**Insight:** the stale-entry skip matters because `heapq` has no decrease-key; without it you reprocess nodes. Dijkstra also fails on negative weights ? that needs Bellman-Ford.

### Grid as graph and multi-source BFS
A grid cell is a node with up to 4 neighbours; seeding the queue with all sources computes all distances in one pass.
```python
def rotting_oranges(grid):
    rows, cols = len(grid), len(grid[0])
    q = deque()
    fresh = 0
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2: q.append((r, c, 0))
            elif grid[r][c] == 1: fresh += 1
    minutes = 0
    while q:
        r, c, t = q.popleft()
        minutes = max(minutes, t)
        for dr, dc in ((1,0), (-1,0), (0,1), (0,-1)):
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                grid[nr][nc] = 2
                fresh -= 1
                q.append((nr, nc, t + 1))
    return minutes if fresh == 0 else -1
# Time O(rows*cols), Space O(rows*cols)
```
**Insight:** multi-source BFS replaces "run BFS from every source" with one O(V+E) pass.

### Graph interview patterns
Traversal, reachability, component counting, dependency ordering. Cues: "prerequisites/build order" = topological sort; "islands/regions" = grid DFS/BFS; "minimum steps" = BFS; "network connectivity over time" = DSU.

---

## 11. Searching and Sorting

Sorting is often the enabling step rather than the answer: it unlocks two pointers, binary search, greedy, and interval merging. Python's `sorted` is Timsort ? stable, adaptive, O(n log n) ? and understanding `key=` and stability is far more valuable in interviews than reciting quicksort.

### Linear search and binary search
Linear is O(n) on any data; binary is O(log n) but **requires sorted input**.
```python
def binary_search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2          # no overflow - Python ints are arbitrary precision
        if nums[mid] == target: return mid
        if nums[mid] < target:  lo = mid + 1
        else:                   hi = mid - 1
    return -1
# Time O(log n), Space O(1)
```
**Insight:** mention that `(lo+hi)//2` overflows in Java/C++ but not Python ? shows you know why the `lo + (hi-lo)//2` idiom exists.

### Lower bound / upper bound thinking with bisect
`bisect_left` gives the first insertion point, `bisect_right` the last; together they bound the range of equal elements.
```python
import bisect
a = [1, 2, 2, 2, 3]
bisect.bisect_left(a, 2)      # 1  - first index of 2
bisect.bisect_right(a, 2)     # 4  - one past last 2
count_of_2 = bisect.bisect_right(a, 2) - bisect.bisect_left(a, 2)   # 3
bisect.insort(a, 2)           # O(n) insert - the search is O(log n)
```
**Insight:** reaching for `bisect` instead of hand-rolling avoids the off-by-one bug farm. The `key=` parameter arrived in 3.10.

### Binary search on answer
When the answer is monotonic ("is X feasible?"), binary search the answer space rather than an array.
```python
def min_capacity(weights, days):
    def feasible(cap):
        need, cur = 1, 0
        for w in weights:
            if cur + w > cap:
                need += 1; cur = 0
            cur += w
        return need <= days
    lo, hi = max(weights), sum(weights)
    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid): hi = mid
        else:             lo = mid + 1
    return lo
# Time O(n log(sum)), Space O(1)
```
**Insight:** the cue is "minimise the maximum" or "maximise the minimum" ? that phrasing almost always means binary search on the answer.

### Sorting fundamentals ? stable vs unstable, in-place vs extra space
A stable sort preserves the relative order of equal keys; `list.sort()` is in-place, `sorted()` returns a new list.
```python
data = [("b", 2), ("a", 1), ("c", 2)]
data.sort(key=lambda x: x[1])      # stable: ('b',2) stays before ('c',2)
```
**Insight:** stability enables multi-key sorting by sorting on the **least** significant key first ? a genuinely senior technique.

### Common sorting algorithms

| Algorithm | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble | O(n) | O(n^2) | O(n^2) | O(1) | yes |
| Selection | O(n^2) | O(n^2) | O(n^2) | O(1) | no |
| Insertion | O(n) | O(n^2) | O(n^2) | O(1) | yes |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | yes |
| Quick | O(n log n) | O(n log n) | O(n^2) | O(log n) | no |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | no |
| **Timsort** | **O(n)** | **O(n log n)** | **O(n log n)** | **O(n)** | **yes** |

**Insight:** Python uses Timsort, which is O(n) on already-sorted or reverse-sorted runs ? that adaptivity is why it wins on real data.

### Python sorting ? sorted, sort, key, reverse, tuple keys
`key` is computed once per element (O(n) calls); `cmp` was removed in Python 3.
```python
import operator
words, data, nums = ["ccc", "a", "bb"], [("b", 2), ("a", 2)], [3, 1, 2]
print(sorted(words, key=len))
print(sorted(data, key=lambda t: (-t[1], t[0])))     # value desc, name asc
print(sorted(data, key=operator.itemgetter(1, 0)))   # C-speed, multi-field
print(sorted(nums, reverse=True))
```
**Insight:** you cannot negate a string for a descending text sort ? use multi-pass (exploiting stability) or `functools.cmp_to_key`.

### Custom sorting strategy
When the comparison is relational rather than a computable key, `cmp_to_key` is the escape hatch.
```python
import functools
def largest_number(nums):
    strs = list(map(str, nums))
    strs.sort(key=functools.cmp_to_key(
        lambda a, b: (a + b < b + a) - (a + b > b + a)))
    return "".join(strs).lstrip("0") or "0"
# Time O(n log n * L), Space O(n)
```
**Insight:** `cmp_to_key` is slower (pairwise Python calls) ? use it only when no key function exists.

### Partitioning logic and quickselect
Partition splits around a pivot; quickselect finds the k-th element in O(n) average without a full sort.
```python
import random
def quickselect(nums, k):                  # k-th smallest, 0-indexed
    lo, hi = 0, len(nums) - 1
    while True:
        p = random.randint(lo, hi)
        nums[p], nums[hi] = nums[hi], nums[p]
        store = lo
        for i in range(lo, hi):
            if nums[i] < nums[hi]:
                nums[store], nums[i] = nums[i], nums[store]
                store += 1
        nums[store], nums[hi] = nums[hi], nums[store]
        if store == k: return nums[store]
        if store < k:  lo = store + 1
        else:          hi = store - 1
# Average O(n), Worst O(n^2), Space O(1)
```
**Insight:** random pivot keeps the O(n^2) worst case out of reach. For top-k in an interview, `heapq.nlargest` at O(n log k) is usually the better trade.

---

## 12. Greedy, Bit Manipulation, Math, and Combinatorics Basics

Greedy makes the locally optimal choice and needs a correctness argument. Bit manipulation gives O(1) set operations on small universes. Python's arbitrary-precision integers remove the overflow concerns that dominate these topics in C++/Java ? mention that, it is a real differentiator.

### Greedy strategy and correctness intuition
Greedy works when a local optimum provably leads to a global one ? usually proven by an exchange argument.
```python
def max_non_overlapping(intervals):
    intervals.sort(key=lambda x: x[1])       # sort by END
    count, end = 0, float("-inf")
    for s, e in intervals:
        if s >= end:
            count += 1; end = e
    return count
# Time O(n log n), Space O(1)
```
**Insight:** be ready for "why sort by end?" ? finishing earliest leaves the most room for the rest. Without that argument it is a guess.

### Sorting-based and interval greedy
Most greedy problems reduce to "sort by the right key, then sweep once".
```python
def can_jump(nums):
    reach = 0
    for i, n in enumerate(nums):
        if i > reach: return False
        reach = max(reach, i + n)
    return True
# Time O(n), Space O(1)
```

### Bit manipulation ? AND, OR, XOR, NOT, shifts
Bitwise ops are O(1) on machine words; XOR is self-inverse, which powers the classic tricks.
```python
a, b = 0b1100, 0b1010
a & b      # 0b1000  AND
a | b      # 0b1110  OR
a ^ b      # 0b0110  XOR
~a         # -13     two's complement
a << 1     # 24      multiply by 2
a >> 1     # 6       floor divide by 2
```
**Insight:** Python ints are arbitrary precision and `~` yields a negative ? there is no fixed width, so masking with `& 0xFFFFFFFF` is needed to emulate 32-bit behaviour.

### Common bit tricks
```python
n = 12
is_even      = n & 1 == 0                  # True
is_power_two = n > 0 and n & (n - 1) == 0  # False (12 = 0b1100)
popcount     = bin(n).count("1")           # 2
popcount_fast = n.bit_count()              # 2   - 3.10+, no string built
clear_lowest = n & (n - 1)                 # 8   - 0b1000
lowest_bit   = n & -n                      # 4   - 0b0100

def single_number(nums):         # every element twice except one
    out = 0
    for x in nums:
        out ^= x                 # x^x == 0, x^0 == x
    return out
# Time O(n), Space O(1)
```
**Insight:** `n.bit_count()` (3.10+) beats `bin(n).count("1")` ? it avoids building a string.

### Math basics ? gcd, lcm, primes, sieve, modular arithmetic
Use the stdlib; a hand-rolled gcd is a chance to introduce a bug.
```python
import math
math.gcd(12, 18)            # 6
math.lcm(4, 6)              # 12  (3.9+)
math.isqrt(17)              # 4   exact integer sqrt

def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, math.isqrt(n) + 1):
        if is_prime[i]:
            is_prime[i*i::i] = [False] * len(is_prime[i*i::i])
    return [i for i, p in enumerate(is_prime) if p]
# Time O(n log log n), Space O(n)

pow(2, 10, 1000)            # modular exponentiation, O(log e)
```
**Insight:** slice assignment in the sieve is markedly faster than an inner Python loop ? a genuine Python-specific optimisation.

### Combinatorics basics
```python
import math
from itertools import product
print(math.factorial(5))              # 120
print(math.comb(5, 2))                # 10   C(n,k)
print(math.perm(5, 2))                # 20   P(n,k)
print(len(list(product([0, 1], repeat=3))))   # 8 - all 3-bit patterns
```
**Insight:** `math.comb` avoids building and counting the actual combinations.

---

## 13. Dynamic Programming

DP applies when a problem has overlapping subproblems and optimal substructure. The senior skill is not memorising recurrences but **designing the state**: what minimal information distinguishes one subproblem from another. In Python, `functools.lru_cache` turns a correct recursion into a memoised solution in one line.

### DP intuition ? overlapping subproblems, optimal substructure
Overlapping means the same subproblem recurs; optimal substructure means an optimal solution is built from optimal sub-solutions.

### Memoisation (top-down)
Write the natural recursion, then cache. Fastest route from brute force to optimal in an interview.
```python
from functools import lru_cache

def climb(n):
    @lru_cache(maxsize=None)
    def go(i):
        if i <= 2: return i
        return go(i - 1) + go(i - 2)
    return go(n)
# Time O(n), Space O(n) cache + O(n) stack
```
**Insight:** flag the costs ? unbounded cache growth, recursion depth, and that `lru_cache` on a **method** pins `self` forever (a real memory leak).

### Tabulation (bottom-up)
Iterative fill of a table; no recursion limit, usually faster, and easier to space-optimise.
```python
def climb_dp(n):
    if n <= 2: return n
    dp = [0] * (n + 1)
    dp[1], dp[2] = 1, 2
    for i in range(3, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
# Time O(n), Space O(n)
```

### State design, transition design, base cases
State = the arguments that uniquely identify a subproblem; transition = how states combine; base case = the smallest solvable instance. Most DP failures are wrong state, not wrong code.
```python
def rob(nums):                 # state = index; take vs skip
    prev = cur = 0
    for n in nums:
        prev, cur = cur, max(cur, prev + n)
    return cur
# Time O(n), Space O(1)
```

### 1-D DP and space optimisation
If a transition only looks back k steps, keep k variables instead of the whole table. Showing the O(n)-space version then rolling it to O(1) is exactly the "optimise when pushed" behaviour expected at senior level.

### 2-D DP and grid DP
```python
def unique_paths(m, n):
    dp = [1] * n
    for _ in range(1, m):
        for j in range(1, n):
            dp[j] += dp[j - 1]            # rolling row: O(n) space
    return dp[-1]
# Time O(m*n), Space O(n)
```

### Subsequence-style DP
```python
def lcs(a, b):                             # longest common subsequence
    dp = [[0] * (len(b) + 1) for _ in range(len(a) + 1)]
    for i in range(1, len(a) + 1):
        for j in range(1, len(b) + 1):
            dp[i][j] = dp[i-1][j-1] + 1 if a[i-1] == b[j-1] else max(dp[i-1][j], dp[i][j-1])
    return dp[-1][-1]
# Time O(n*m), Space O(n*m)

import bisect
def lis(nums):                             # O(n log n) patience sorting
    tails = []
    for x in nums:
        i = bisect.bisect_left(tails, x)
        if i == len(tails): tails.append(x)
        else:               tails[i] = x
    return len(tails)
# Time O(n log n), Space O(n)
```
**Insight:** `tails` is **not** the actual LIS ? only its length is correct. Claiming otherwise reveals the algorithm was memorised.

### Knapsack-style and partition-style DP
0/1 knapsack picks each item at most once; iterate capacity **descending** to prevent reuse.
```python
def knapsack(weights, values, cap):
    dp = [0] * (cap + 1)
    for w, v in zip(weights, values):
        for c in range(cap, w - 1, -1):    # descending = 0/1
            dp[c] = max(dp[c], dp[c - w] + v)
    return dp[cap]
# Time O(n*cap), Space O(cap)

def can_partition(nums):                   # equal-sum subset
    total = sum(nums)
    if total % 2: return False
    target = total // 2
    dp = {0}
    for n in nums:
        dp |= {n + x for x in dp if n + x <= target}
    return target in dp
# Time O(n*target), Space O(target)
```
**Insight:** ascending capacity turns 0/1 into **unbounded** knapsack. That single loop direction is the whole distinction.

### String DP
```python
def edit_distance(a, b):
    dp = list(range(len(b) + 1))
    for i in range(1, len(a) + 1):
        prev, dp[0] = dp[0], i
        for j in range(1, len(b) + 1):
            prev, dp[j] = dp[j], prev if a[i-1] == b[j-1] else 1 + min(prev, dp[j], dp[j-1])
    return dp[-1]
# Time O(n*m), Space O(m)
```

### Decision vs counting DP
```python
def coin_change(coins, amount):            # decision: fewest coins
    dp = [0] + [float("inf")] * amount
    for c in coins:
        for a in range(c, amount + 1):
            dp[a] = min(dp[a], dp[a - c] + 1)
    return dp[amount] if dp[amount] != float("inf") else -1

def coin_combinations(coins, amount):      # counting: number of ways
    dp = [1] + [0] * amount
    for c in coins:
        for a in range(c, amount + 1):
            dp[a] += dp[a - c]
    return dp[amount]
# Both Time O(n*amount), Space O(amount)
```
**Insight:** loop order matters ? coins outer counts **combinations**; amount outer counts **permutations**. Interviewers probe this.

### Recognising DP patterns and common pitfalls
Cues: "count the ways", "minimum/maximum cost", "can you reach", "longest/shortest subsequence". Pitfalls: wrong state, wrong traversal order, missing base case, mutating shared state, and forgetting that greedy fails ? coin change with coins `[1,3,4]` and amount 6 gives 3 coins greedily but 2 with DP.

---

## 14. Python-Specific DSA Tools and Problem-Solving Patterns

This is where Python candidates differentiate themselves. Knowing the right stdlib tool turns a 20-line hand-rolled structure into two lines, and knowing Python's performance characteristics prevents accidentally quadratic solutions that pass small tests and fail at scale.

### collections ? defaultdict, Counter, deque, namedtuple
```python
from collections import defaultdict, Counter, deque, namedtuple
groups = defaultdict(list); groups["a"].append(1)
freq = Counter("aab"); freq.most_common(1)      # [('a', 2)]
dq = deque([1,2]); dq.appendleft(0); dq.popleft()
Point = namedtuple("Point", "x y"); p = Point(1, 2); p.x
```
**Insight:** a plain `defaultdict` **read** inserts the key ? it silently grows the dict and changes `len()`. Use `.get()` for read-only access.

### heapq
```python
import heapq
xs = [5, 1, 3]
heapq.heapify(xs)                           # O(n)
print(heapq.heappushpop(xs, 2))             # cheaper than push then pop
print(heapq.nsmallest(2, ["aaa", "b", "cc"], key=len))   # O(n log k)
print(list(heapq.merge([1, 4], [2, 3])))    # lazy k-way merge
```

### bisect ? binary insertion and lookup
```python
import bisect
a, x = [1, 3, 5], 4
print(bisect.bisect_left(a, x))    # 2 - search O(log n)
bisect.insort(a, x)                # insert O(n)
print(a)
```

### itertools ? accumulate, permutations, combinations, product
```python
from itertools import accumulate, permutations, combinations, product, groupby, chain, islice
list(accumulate([1,2,3]))                 # [1,3,6] prefix sums
list(accumulate([3,1,2], max))            # [3,3,3] running max
list(product("ab", repeat=2))             # all pairs
list(chain([1],[2,3]))                    # lazy flatten one level
list(islice(range(100), 5))               # slice any iterable
```
**Insight:** `groupby` only groups **consecutive** equal keys ? it requires pre-sorted input, and forgetting that silently fragments groups.

### functools ? lru_cache
```python
from functools import lru_cache
@lru_cache(maxsize=None)
def f(n): return n * n
f(2); f(2)
print(f.cache_info())      # hits=1, misses=1
f.cache_clear()
```
**Insight:** arguments must be hashable ? so no list/dict/set parameters ? and keyword vs positional calls create **separate** cache entries.

### enumerate, zip, sorted with custom keys
```python
xs, ys, matrix = [1, 2, 3], "ab", [[1, 2], [3, 4]]
print([(i, v) for i, v in enumerate(xs, start=1)])
print(list(zip(xs, ys)))                  # stops at the SHORTEST, silently
try:
    list(zip(xs, ys, strict=True))        # 3.10+, raises on mismatch
except ValueError as e:
    print("ValueError:", e)
print([list(t) for t in zip(*matrix)])    # transpose
```
**Insight:** silent `zip` truncation is a genuine data-loss bug. `strict=True` should be the default when lengths must match.

### Comprehensions and generator expressions
```python
squares = [x*x for x in range(10)]        # list, O(n) memory
total = sum(x*x for x in range(10**6))    # generator, O(1) memory
```
**Insight:** a generator is single-use ? consuming it twice silently yields nothing the second time.

### Tuple packing for sorting and heaps
```python
import heapq, itertools
h, counter = [], itertools.count()
for priority, payload in [(2, "low"), (1, "high")]:
    heapq.heappush(h, (priority, next(counter), payload))   # counter prevents
print(heapq.heappop(h))                                     # payload comparison
items = [("b", 2), ("a", 2)]
print(sorted(items, key=lambda x: (-x[1], x[0])))           # score desc, name asc
```

### Using sets and dicts efficiently
```python
big_list = list(range(10_000)); big_set = set(big_list)
x = 9999
print(x in big_list)      # O(n)
print(x in big_set)       # O(1)
```

### Python recursion limit awareness
```python
import sys
sys.getrecursionlimit()       # 1000
```
**Insight:** convert to an explicit stack rather than raising the limit ? raising it risks a hard crash.

### Python-specific time complexity trade-offs

| Operation | list | deque | set/dict |
|---|---|---|---|
| index | O(1) | O(n) | - |
| append / push right | O(1)* | O(1) | - |
| pop right | O(1) | O(1) | - |
| **insert/pop left** | **O(n)** | **O(1)** | - |
| membership `in` | O(n) | O(n) | **O(1)** |

\* amortised

```python
parts = ["a", "b", "c"]
print("".join(parts))      # O(n)   - correct string building
# s += x inside a loop     # O(n^2) - quadratic trap
```

### Avoiding costly patterns
```python
# BAD: slicing in recursion - O(n^2)
def bad(arr):
    if not arr: return 0
    return arr[0] + bad(arr[1:])          # copies every call
# GOOD: pass an index - O(n)
def good(arr, i=0):
    if i == len(arr): return 0
    return arr[i] + good(arr, i + 1)
```

### Clean Python interview code style
Readable structure, helper functions, early returns, explicit edge handling, type hints, and a stated complexity.
```python
def two_sum(nums: list[int], target: int) -> list[int]:
    # Return indices of the two numbers adding to target. O(n) time, O(n) space.
    if len(nums) < 2:
        return []
    seen: dict[int, int] = {}
    for i, n in enumerate(nums):
        if (need := target - n) in seen:
            return [seen[need], i]
        seen[n] = i
    return []
```

---

## 15. High-Frequency Interview Problem Families

These families cover the large majority of product-company questions. Recognise the family, state the pattern, give the complexity, then code. Being able to name the family out loud is itself a senior signal.

| # | Family | Canonical problems | Pattern | Typical complexity |
|---|---|---|---|---|
| 1 | **Array & hashing** | Two Sum, Group Anagrams, Top K Frequent | dict/set/Counter | O(n) |
| 2 | **String patterns** | Valid Anagram, Longest Palindromic Substring | Counter, expand-around-centre | O(n) ? O(n^2) |
| 3 | **Two pointers** | 3Sum, Container With Most Water, Trapping Rain Water | inward pointers on sorted data | O(n) ? O(n^2) |
| 4 | **Sliding window** | Longest Substring Without Repeat, Min Window Substring | variable window + hash map | O(n) |
| 5 | **Stack / monotonic** | Valid Parentheses, Daily Temperatures, Largest Rectangle | stack of pending indices | O(n) |
| 6 | **Linked list** | Reverse, Cycle Detect, Merge K, Reorder | dummy head, fast/slow | O(n) |
| 7 | **Tree DFS/BFS** | Level Order, Max Depth, Diameter, Serialize | recursion / deque | O(n) |
| 8 | **BST property** | Validate BST, Kth Smallest, LCA | bounds, inorder | O(h) ? O(n) |
| 9 | **Heap / top-K** | Top K Frequent, Merge K Lists, Median Stream | heapq, two heaps | O(n log k) |
| 10 | **Graph traversal** | Number of Islands, Clone Graph, Course Schedule | BFS/DFS, topo sort | O(V+E) |
| 11 | **Binary search** | Rotated Array, Search 2D, Koko Bananas | search space / on answer | O(log n) |
| 12 | **Intervals** | Merge Intervals, Insert Interval, Meeting Rooms II | sort + sweep / heap | O(n log n) |
| 13 | **Backtracking** | Subsets, Permutations, N-Queens, Word Search | choose/explore/unchoose | O(2^n) ? O(n!) |
| 14 | **Dynamic programming** | Climb Stairs, Coin Change, LIS, Edit Distance | memo / tabulate | O(n) ? O(n*m) |
| 15 | **Union find** | Number of Components, Redundant Connection | DSU + path compression | O(E * alpha(n)) |
| 16 | **Mixed pattern** | LRU Cache, Min Stack, Design Twitter | composite structures | O(1) amortised |

### Worked example of a mixed-pattern design question
LRU Cache is the most-asked design-flavoured DSA question at product companies: it requires O(1) get and put.
```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.cache = OrderedDict()

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)            # mark most-recently used
        return self.cache[key]

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.cap:
            self.cache.popitem(last=False)     # evict least-recently used
# get/put O(1), Space O(capacity)
```
**Follow-ups to expect:** implement it without `OrderedDict` (dict + doubly linked list); make it thread-safe (a `Lock` around both methods); add TTL expiry; and "how does this differ from `functools.lru_cache`?"

### Final preparation checklist
- State the pattern and complexity **before** coding
- Clarify constraints and edge cases first
- Write clean, named, type-hinted code
- Verify with one small dry run
- Offer the optimisation when pushed, and name the trade-off
- Know the Python-specific traps: `list.pop(0)`, string `+=`, slicing in loops, recursion depth, `[[0]*n]*m` aliasing, silent `zip` truncation, `defaultdict` read-inserts
