# Senior Python Interview Master Sheet — Product-Based Companies (8–12 Years)
*Final 10/10 Version | Senior-Focused | Spoken-Answer Optimized | Production-Oriented*

> **Audience:** Python engineers with **8–12 years of experience** preparing for **product-based company interviews**.  
> **Expectation at this level:** Interviewers usually test **language mastery + design judgment + production trade-offs + debugging maturity + performance awareness + API clarity + maintainability thinking**.  
> **Recommended answer format in interviews:** **definition → internal reason → trade-off / edge case → production implication → tiny example**.  
> Tags: **[HF]** = high frequency, **[Senior]** = common at 8–12 years, **[Perf]** = performance-focused, **[Design]** = architecture/API judgment, **[Trap]** = frequent follow-up.

---

## 1) Python Runtime and Language Model

### 1. [HF][Senior] What is Python?
Python is a high-level, dynamically typed, strongly typed programming language with automatic memory management and a very strong standard library/ecosystem. In CPython, source code is compiled to bytecode and then executed by the Python Virtual Machine. In senior interviews, the strongest answer also mentions the trade-off: Python improves developer productivity and readability, but its runtime dynamism adds overhead compared to lower-level statically compiled languages.  
**Example**
```python
x = 10
print(type(x))  # <class 'int'>
```

### 2. [HF] Is Python interpreted or compiled?
The precise answer is: in CPython, Python is both bytecode-compiled and VM-interpreted. Source code is first compiled into bytecode, and that bytecode is executed by the interpreter. Senior interviewers often ask this because saying only “interpreted” is incomplete.  
**Example**
```python
import dis
dis.dis("a = 1 + 2")
```

### 3. [HF] What does dynamic typing mean?
Dynamic typing means names are bound to objects at runtime, and the object carries the type, not the variable name. That makes Python flexible, but in large codebases it increases reliance on type hints, tests, and disciplined interfaces. At senior level, the important point is not just the definition but how to control the risks.  
**Example**
```python
x = 10
x = "python"
```

### 4. What does strong typing mean in Python?
Python is strongly typed because it does not silently coerce unrelated types in unsafe operations. If types are incompatible, Python raises an exception unless conversion is explicit. This improves correctness, but it also means interface boundaries should be intentional and well-validated.  
**Example**
```python
# 1 + "2"  # TypeError
print(1 + int("2"))
```

### 5. [Senior] What are the practical implications of dynamic typing in large systems?
Dynamic typing speeds development and reduces ceremony, but it also shifts some guarantees from compile time to runtime. In large product codebases, that means stronger pressure on tests, type hints, validation, code review, and API contracts. Senior engineers are expected to combine Python’s flexibility with guardrails rather than treating dynamism as “free.”  
**Example**
```python
def process_user(user_id: int) -> str:
    return str(user_id)
```

### 6. What is bytecode?
Bytecode is Python’s intermediate, low-level representation generated from source before execution by the VM. It is not machine code, but it is lower-level than Python source. This layer improves portability and separates parsing/compilation from execution.  
**Example**
```python
import dis
def f(): return 2 + 3
dis.dis(f)
```

### 7. What is the Python Virtual Machine?
The Python Virtual Machine executes Python bytecode instruction by instruction. It provides a runtime layer between source code and hardware, which supports portability and introspection but adds overhead relative to native code. In senior interviews, it is useful to connect this with runtime flexibility and performance trade-offs.  
**Example**
```python
# CPython VM executes compiled bytecode
```

### 8. What are `.pyc` files?
`.pyc` files are cached bytecode files used to speed up future imports by skipping recompilation from source when possible. They improve import/startup behavior, not the runtime speed of business logic itself. That distinction is a common follow-up question.  
**Example**
```python
# __pycache__/module.cpython-312.pyc
```

### 9. Why is indentation part of Python syntax?
Python uses indentation to define blocks instead of braces, which makes structure visually explicit. This enforces consistency and often improves readability in teams. In practice, it reduces formatting ambiguity but requires discipline because whitespace affects semantics.  
**Example**
```python
if True:
    print("block")
```

### 10. [Senior] What matters most about Python in product-based companies?
What matters most is not syntax trivia but whether you can write clear APIs, choose the right data structures, reason about runtime behavior, design maintainable modules, and debug production issues effectively. Product companies value readability, iteration speed, ecosystem leverage, and operational clarity. Senior candidates are expected to connect language features to business-scale engineering decisions.  
**Example**
```python
def normalize_email(email: str) -> str:
    return email.strip().lower()
```

---

## 2) Object Model, Identity, Equality, and Mutability

### 11. [HF] What is the difference between identity, type, and value?
Every Python object has an identity, a type, and a value. Identity is what `id()` returns and what `is` compares, type is the object’s runtime class, and value is the logical content usually compared by `==`. In senior interviews, this matters because confusing identity with equality causes subtle bugs, especially with mutable objects and sentinels.  
**Example**
```python
a = [1, 2]
b = [1, 2]
print(a == b)  # True
print(a is b)  # False
```

### 12. [HF][Trap] `is` vs `==`?
`==` checks value equality, usually through `__eq__`, while `is` checks object identity. Two different objects can be equal but not identical. The strong senior answer is: use `is` for singleton identity checks like `None`, and use `==` for value comparison.  
**Example**
```python
x = None
print(x is None)
```

### 13. [HF] Why compare `None` with `is`?
`None` is a singleton, so identity comparison is the correct semantic test. Equality can be overloaded in surprising ways, but identity cannot. Using `is None` is therefore both clearer and safer.  
**Example**
```python
value = None
print(value is None)
```

### 14. [HF] Mutable vs immutable?
Mutable objects can change in place after creation, while immutable objects cannot. Lists, dicts, and sets are mutable; strings, tuples, ints, and frozensets are immutable. At senior level, mutability is important because it directly affects sharing, hashing, API contracts, and concurrency risk.  
**Example**
```python
a = [1, 2]
a.append(3)

s = "ab"
s = s + "c"
```

### 15. [HF][Senior] Why does mutability matter in production systems?
Mutability matters because shared mutable state increases coupling and makes behavior harder to reason about under reuse, caching, and concurrency. Immutable values are easier to share safely and are often better at boundaries. Senior engineers are expected to make mutation explicit and local rather than accidental and global.  
**Example**
```python
settings = {"timeout": 30}
# shared mutable state across modules can create side effects
```

### 16. [HF] What is hashability?
An object is hashable if its hash value remains stable during its lifetime and its equality semantics remain consistent with that hash. Hashable objects can be used as dictionary keys and set elements. The key connection is that mutation usually breaks hash stability.  
**Example**
```python
d = {("region", 1): "ok"}
```

### 17. [Trap] Can a tuple be unhashable?
Yes. A tuple is hashable only if all of its members are hashable. If it contains a list, dict, or set, the tuple itself becomes unhashable.  
**Example**
```python
print(hash((1, 2, 3)))
# hash((1, [2]))  # TypeError
```

### 18. What are truthy and falsy values?
Python evaluates objects in boolean context using truth-value testing. `None`, `False`, numeric zero, and empty containers are falsy; most other values are truthy. Senior engineers should be careful not to confuse “missing” with merely “empty” in business logic.  
**Example**
```python
print(bool([]), bool([1]))
```

### 19. What is aliasing?
Aliasing means multiple references point to the same object. If the object is mutable, a mutation through one alias is visible through the others. This is a common reason for hidden side effects and ownership bugs.  
**Example**
```python
a = [1, 2]
b = a
b.append(3)
print(a)
```

### 20. [HF][Trap] Why are mutable default arguments dangerous?
Default arguments are evaluated once at function definition time, not once per call. So a mutable default is shared across calls and can accumulate state unexpectedly. The production-safe pattern is to use `None` and initialize inside the function.  
**Example**
```python
def add_item(item, bucket=None):
    if bucket is None:
        bucket = []
    bucket.append(item)
    return bucket
```

### 21. [Senior] How do you reduce unintended mutation bugs in large Python codebases?
I make ownership boundaries explicit, avoid exposing internal mutable state directly, prefer immutable return shapes at boundaries where possible, and use copying intentionally rather than everywhere. I also review APIs for aliasing risk and shared mutable defaults. Senior engineering here is about predictable state flow, not just defensive programming.  
**Example**
```python
def user_ids(users):
    return tuple(u["id"] for u in users)
```

---

## 3) Data Structures and Complexity Trade-offs

### 22. [HF] List vs tuple?
A list is mutable and intended for dynamic collections, while a tuple is immutable and better for fixed records or grouped return values. Tuples can also be hashable if their elements are hashable. The senior-level answer includes not only mutability but also intent, memory semantics, and suitability as keys.  
**Example**
```python
event_ids = [101, 102, 103]
user_key = ("region-1", 42)
```

### 23. [HF] Set vs frozenset?
A set is mutable and supports add/remove operations. A frozenset is immutable and hashable, so it can be used where set-like semantics are needed inside a key or another set. The important design point is whether uniqueness must remain mutable or value-like.  
**Example**
```python
s = {1, 2, 3}
fs = frozenset([1, 2, 3])
```

### 24. [HF] How does a dictionary work internally?
A dictionary is implemented as a hash table. Python hashes the key, locates a candidate slot, and resolves collisions internally using probing. Average-case lookup, insert, and delete are $O(1)$, which is why dict is one of the most performance-critical built-ins in Python applications.  
**Example**
```python
user = {"id": 101, "name": "Ava"}
print(user["name"])
```

### 25. [HF] Why must dictionary keys be hashable?
Because dictionary lookup depends on a stable hash and equality-consistent behavior. If a key mutates after insertion, lookup semantics break because the object may no longer map to the correct slot. That is why mutable types like lists are not valid dict keys.  
**Example**
```python
d = {(1, 2): "ok"}
```

### 26. [HF] Are dictionaries ordered?
Yes. Since Python 3.7, insertion order is guaranteed by the language spec. In senior interviews, the nuance is that deterministic order is useful for APIs, diffs, tests, and serialization, but a dict is still conceptually a mapping, not a sequence abstraction.  
**Example**
```python
d = {"a": 1, "b": 2}
print(list(d.keys()))
```

### 27. [Perf] Why is `x in set` faster than `x in list` on average?
Set membership is hash-based and average-case $O(1)$, while list membership is linear scan and $O(n)$. In real systems, converting to a set is often worthwhile for repeated lookups, but only if you do not need duplicates or order preservation.  
**Example**
```python
allowed = {"read", "write", "delete"}
print("write" in allowed)
```

### 28. [Perf] What is the time complexity of list append?
`list.append()` is amortized $O(1)`. Most appends are constant time, but occasional resizes require allocating a larger array and copying references. The interview keyword is “amortized,” not simply “constant.”  
**Example**
```python
a = []
a.append(1)
```

### 29. [HF] What is shallow copy vs deep copy?
A shallow copy creates a new outer container but reuses references to nested objects. A deep copy recursively copies nested objects too. In senior code, deep copy should be used cautiously because it can be expensive, semantically wrong for identity-bearing objects, and unsafe with external resources.  
**Example**
```python
import copy
a = [[1], [2]]
b = copy.copy(a)
c = copy.deepcopy(a)
```

### 30. [Senior] How do you decide between copying and sharing?
I copy when callers should not observe or mutate internal state, or when ownership boundaries are unclear. I share when the object is intentionally shared, immutable in practice, or copying is too expensive and the contract is explicit. Senior judgment here is about defining ownership and lifecycle clearly rather than blindly copying.  
**Example**
```python
def get_config():
    return {"timeout": 30}.copy()
```

### 31. What is slicing, and does it return a view?
For built-in sequences like lists and strings, slicing returns a new object, not a view. That means slicing copies references or characters into a fresh object. In performance-sensitive code, repeated slicing of large collections can be surprisingly expensive.  
**Example**
```python
a = [1, 2, 3, 4]
print(a[1:3], a[::-1])
```

### 32. What is dictionary comprehension?
A dictionary comprehension creates a dictionary directly from an iterable using concise transformation syntax. It is great for simple mappings and filters. Senior engineers should still avoid dense comprehensions when readability drops.  
**Example**
```python
squares = {x: x * x for x in range(5)}
```

### 33. What is the difference between direct access, `get`, and `setdefault`?
Direct access `d[k]` raises `KeyError` if the key is missing. `get` returns a default without modifying the dict, and `setdefault` returns a value while also inserting the default if absent. The trade-off is that `setdefault` is convenient but can be less explicit than a `defaultdict` in repeated grouping logic.  
**Example**
```python
d = {}
d.setdefault("team", []).append("alice")
```

### 34. [HF] When is `defaultdict` better than `setdefault`?
`defaultdict` is better when missing-key initialization is repetitive and uniform, such as grouping, counting, or building adjacency lists. It reduces loop noise and communicates intent more clearly. I would prefer `setdefault` only for isolated or local one-off cases.  
**Example**
```python
from collections import defaultdict
groups = defaultdict(list)
groups["backend"].append("alice")
```

### 35. [Senior] How do you choose the right data structure in product code?
I start with the access pattern, mutation pattern, and business semantics. Lookup-heavy problems usually favor dict/set, ordered append-heavy flows favor list, queue semantics favor deque, fixed records favor tuple/dataclass, and frequency problems often favor Counter. The right answer is driven by complexity, readability, ownership, and future change.  
**Example**
```python
from collections import deque
jobs = deque()
jobs.append("sync-task")
```

---

## 4) Strings, Unicode, and Data Cleanliness

### 36. Why is repeated string concatenation inefficient?
Strings are immutable, so each concatenation creates a new string and copies data. In loops, that can lead to quadratic behavior. For multiple parts, `''.join(...)` is the idiomatic and efficient pattern.  
**Example**
```python
parts = ["senior", "python", "engineer"]
print(" ".join(parts))
```

### 37. What is string interning?
String interning is an optimization where identical immutable strings may share storage. This can reduce memory usage and sometimes make identity comparisons appear true for some literals. Even so, code should still use `==` for value comparison.  
**Example**
```python
a = "hello"
b = "hello"
print(a is b)  # may be True
```

### 38. What is the difference between `find` and `index`?
Both search for a substring, but `find` returns `-1` when the substring is missing while `index` raises `ValueError`. I use `find` when “not found” is a normal case and `index` when absence is exceptional. That difference is usually the interviewer’s real target.  
**Example**
```python
s = "python"
print(s.find("z"))
```

### 39. Why are f-strings preferred?
F-strings are concise, readable, and allow expressions inline with formatting specifiers. They are usually clearer than `%` formatting or `str.format()` for day-to-day application code. Senior engineers prefer them because they optimize maintainability, not just brevity.  
**Example**
```python
name, years = "Mira", 9
print(f"{name} has {years} years of experience")
```

### 40. What is the difference between `str` and `bytes`?
`str` represents Unicode text, while `bytes` represents raw binary data. Encoding converts text to bytes; decoding converts bytes to text. Senior candidates should be precise here because many production bugs happen at text/binary boundaries.  
**Example**
```python
text = "café"
raw = text.encode("utf-8")
print(raw.decode("utf-8"))
```

### 41. [Senior] What data-cleanliness issues matter in production systems?
Common issues include whitespace normalization, inconsistent casing, Unicode normalization, accidental double-encoding, locale-sensitive transformations, and ambiguous empty-vs-missing values. Many business bugs come from poor normalization rather than algorithmic failure. Senior engineers should design normalization close to input boundaries.  
**Example**
```python
def normalize_email(email: str) -> str:
    return email.strip().lower()
```

---

## 5) Control Flow and Expression Semantics

### 42. [HF] `pass` vs `continue` vs `break`?
`pass` is a no-op placeholder, `continue` skips the rest of the current loop iteration, and `break` exits the nearest loop entirely. These are small constructs, but imprecision here signals weak fundamentals. Senior answers should be brief and exact.  
**Example**
```python
for i in range(5):
    if i == 1:
        continue
    if i == 3:
        break
    print(i)
```

### 43. What is short-circuit evaluation?
In `and`, evaluation stops at the first falsy operand; in `or`, it stops at the first truthy operand. This avoids unnecessary work and supports guard-style expressions. The subtle point is that these operators return operands, not forced booleans.  
**Example**
```python
x = None
print(x or "default")
```

### 44. What is chained comparison?
Chained comparison allows expressions like `a < b < c` and evaluates them efficiently with the middle operand evaluated once. It is cleaner than repeating the middle term with `and`. I use it when it improves readability rather than as a trick.  
**Example**
```python
x = 7
print(1 < x < 10)
```

### 45. What is tuple unpacking?
Tuple unpacking assigns values from an iterable to multiple variables in one statement. It is widely used for swaps, loop variables, and function returns. Extended unpacking with `*` is especially useful in parsing or splitting head/tail structures.  
**Example**
```python
a, b = 1, 2
a, b = b, a
```

### 46. What is the walrus operator, and when would you use it?
The walrus operator `:=` assigns inside an expression. I use it when it removes repeated work and improves readability in a small local scope. In senior code, I avoid it if it makes the expression harder to scan.  
**Example**
```python
if (n := len("python")) > 5:
    print(n)
```

---

## 6) Functions, Scope, and API Design

### 47. [HF] Are functions first-class objects in Python?
Yes. Functions can be assigned to variables, passed to other functions, returned, and stored in containers. That enables decorators, callbacks, factories, and higher-order abstractions.  
**Example**
```python
def greet():
    return "hi"

f = greet
print(f())
```

### 48. What is the difference between parameters and arguments?
Parameters are the names in the function definition, while arguments are the actual values passed during a call. This distinction matters when discussing function signatures, readability, and API misuse. Senior candidates should answer this with precise vocabulary.  
**Example**
```python
def add(a, b):  # parameters
    return a + b

add(2, 3)       # arguments
```

### 49. [HF] What are `*args` and `**kwargs`?
`*args` collects extra positional arguments into a tuple, and `**kwargs` collects extra keyword arguments into a dictionary. They are useful for wrappers and extensibility points, but overusing them can make APIs vague and harder to discover. Senior code values explicitness over flexibility-by-default.  
**Example**
```python
def log_call(*args, **kwargs):
    print(args, kwargs)
```

### 50. [HF][Senior] What is the LEGB rule?
Python resolves names in Local, Enclosing, Global, and Built-in scope order. This explains nested function behavior and closure lookup. The important follow-up is that assignment creates a local binding unless you explicitly declare `global` or `nonlocal`.  
**Example**
```python
x = "global"
def outer():
    x = "enclosing"
    def inner():
        print(x)
    inner()
outer()
```

### 51. [HF] What is a closure?
A closure is a function that retains access to variables from its enclosing lexical scope even after the outer function returns. Closures are useful for decorators, factories, and lightweight stateful behavior. Internally, Python stores captured variables in closure cells.  
**Example**
```python
def outer(x):
    def inner():
        return x
    return inner

print(outer(10)())
```

### 52. [HF][Trap] What is late binding in closures?
Late binding means a closure looks up free variables when the inner function executes, not when it is defined. This causes the classic loop-closure bug where every function sees the final loop value. The usual fix is to bind the current value using a default argument or another scope layer.  
**Example**
```python
funcs = [lambda x=i: x for i in range(3)]
print([f() for f in funcs])
```

### 53. What does `nonlocal` do?
`nonlocal` allows rebinding a name in the nearest enclosing function scope. It is used when a nested function must update captured state. Without it, assignment creates a new local in the inner function.  
**Example**
```python
def outer():
    count = 0
    def inner():
        nonlocal count
        count += 1
        return count
    return inner
```

### 54. What does `global` do?
`global` declares that a name refers to the module-level binding for assignment. Reading a global does not require `global`; rebinding does. In senior design conversations, frequent use of globals is usually a sign of hidden coupling.  
**Example**
```python
count = 0
def inc():
    global count
    count += 1
```

### 55. What is a lambda function?
A lambda is an anonymous function limited to a single expression. It is appropriate for small local behaviors like sort keys or callbacks. For real business logic, a named function is easier to trace, test, and debug.  
**Example**
```python
items = [("a", 3), ("b", 1)]
print(sorted(items, key=lambda x: x[1]))
```

### 56. [Senior][Design] How do you design clean Python function APIs?
I prefer explicit parameters, keyword-only arguments when readability matters, stable return contracts, and minimal ambiguity around defaults. I avoid `**kwargs` as a substitute for clear interface design. Good senior API design makes misuse difficult and change safer.  
**Example**
```python
def fetch_user(user_id: int, *, include_orders: bool = False) -> dict:
    return {"id": user_id, "orders": [] if include_orders else None}
```

### 57. What are positional-only and keyword-only arguments?
Positional-only parameters appear before `/` and must be passed by position. Keyword-only parameters appear after `*` and must be passed by name. These features are useful when you want API clarity and backward-compatible signature evolution.  
**Example**
```python
def f(a, /, b, *, c):
    return a, b, c
```

### 58. Can Python return multiple values?
Python returns a single object, but multiple values are typically packed into a tuple implicitly. The caller then unpacks that tuple. So the syntax looks like multiple returns, but semantically it is tuple packing/unpacking.  
**Example**
```python
def stats():
    return 10, 20

avg, total = stats()
```

---

## 7) Iteration, Lazy Evaluation, and Streaming Patterns

### 59. [HF] What is an iterable?
An iterable is any object that can provide an iterator, usually via `__iter__()`. Lists, tuples, strings, dicts, sets, and generators are all iterable. The practical meaning is that the object can be used in `for` loops and many built-ins.  
**Example**
```python
for ch in "abc":
    print(ch)
```

### 60. [HF] What is an iterator?
An iterator is an object implementing `__iter__()` and `__next__()`. It yields items one at a time and raises `StopIteration` when exhausted. Iterators are stateful and consumable.  
**Example**
```python
it = iter([1, 2, 3])
print(next(it))
```

### 61. [HF] Iterable vs iterator?
An iterable can produce an iterator, while an iterator is the object that actually tracks iteration state and yields successive items. Every iterator is iterable, but not every iterable is an iterator. This matters when deciding whether data can be traversed multiple times.  
**Example**
```python
data = [1, 2, 3]
it = iter(data)
print(iter(it) is it)
```

### 62. [HF] What is a generator?
A generator is a special iterator created by a function containing `yield` or by a generator expression. It computes values lazily and preserves execution state between yields. This makes it very effective for streaming pipelines and large datasets.  
**Example**
```python
def gen():
    yield 1
    yield 2
```

### 63. [HF] What does `yield` do?
`yield` pauses the function, returns a value, and saves local state so execution can resume later. That is how generator functions produce values incrementally without storing all results in memory. It is one of Python’s key lazy-evaluation features.  
**Example**
```python
def count():
    yield 1
    yield 2
```

### 64. [HF] Generator expression vs list comprehension?
A list comprehension eagerly materializes all values, while a generator expression computes values lazily on demand. I choose generators for large or one-pass flows and lists when I need indexing, reuse, or eager inspection. The trade-off is memory versus immediacy and repeatability.  
**Example**
```python
lst = [x * x for x in range(5)]
gen = (x * x for x in range(5))
```

### 65. [Trap] What is iterator exhaustion?
Once an iterator has been consumed, it cannot be reused unless you build a new iterator from the original iterable. This often causes subtle bugs when code logs, validates, or partially consumes a generator and then expects to iterate again. Senior engineers should watch for this in data pipelines.  
**Example**
```python
it = iter([1, 2])
print(list(it))
print(list(it))  # []
```

### 66. What are `enumerate` and `zip` used for?
`enumerate` yields index-value pairs and avoids manual index tracking. `zip` combines multiple iterables elementwise and stops at the shortest iterable. Both are standard, readable Python tools for parallel iteration.  
**Example**
```python
for i, v in enumerate(["a", "b"], 1):
    print(i, v)

print(list(zip([1, 2], ["x", "y"])))
```

### 67. What do `map`, `filter`, and `reduce` do?
`map` transforms each element, `filter` keeps elements matching a predicate, and `reduce` combines elements into one accumulated result. In Python, comprehensions are often more readable than `map`/`filter`, but these functions still have a place in composable pipelines. Senior answers should prioritize readability over functional cleverness.  
**Example**
```python
from functools import reduce
print(list(map(lambda x: x * 2, [1, 2, 3])))
print(list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4])))
print(reduce(lambda a, b: a + b, [1, 2, 3], 0))
```

### 68. What do `any` and `all` do?
`any` returns `True` if at least one element is truthy, while `all` returns `True` only if every element is truthy. Both short-circuit, which makes them efficient for validations. They are especially clean when combined with generator expressions.  
**Example**
```python
print(any([0, "", 5]))
print(all([1, 2, 3]))
```

### 69. [Senior] Where do generators help in real product systems?
Generators help when processing large files, paginated APIs, event streams, or ETL-style transformations where full materialization is wasteful. They reduce peak memory and support composable pipelines. The trade-off is that lazy flows are single-pass by default and can be harder to debug if overused.  
**Example**
```python
def read_lines(path):
    with open(path) as f:
        for line in f:
            yield line.strip()
```

---

## 8) Object-Oriented Python and Design Judgment

### 70. What is a class?
A class is a blueprint for creating objects and encapsulating shared behavior and state. In Python, classes are themselves runtime objects created by metaclasses. In interviews, the main point is usually how classes help define behavior and contracts, not just syntax.  
**Example**
```python
class User:
    pass
```

### 71. What is an object?
An object is an instance of a class with identity, type, and state. Everything in Python is an object, including functions, classes, and modules. This uniformity is central to the language model.  
**Example**
```python
x = 10
print(type(x))
```

### 72. [HF] Instance variable vs class variable?
Instance variables belong to individual objects, while class variables belong to the class and are shared unless shadowed. A common trap is putting mutable shared state in a class variable unintentionally. Senior engineers are expected to catch that immediately.  
**Example**
```python
class A:
    shared = []
    def __init__(self, x):
        self.x = x
```

### 73. What is `self`?
`self` is the conventional name for the current instance passed to instance methods. It is not a keyword, but the convention is universal. It provides access to per-instance state and behavior.  
**Example**
```python
class A:
    def show(self):
        return "instance method"
```

### 74. [HF] What is inheritance?
Inheritance allows a class to reuse and extend the behavior of another class. It enables polymorphism and code reuse, but in senior design conversations it should represent a real “is-a” relationship. Overusing inheritance can make systems rigid and fragile.  
**Example**
```python
class Animal:
    def speak(self): return "sound"

class Dog(Animal):
    def speak(self): return "bark"
```

### 75. [HF] What is method overriding?
Method overriding happens when a subclass defines a method with the same name as one in its base class. Runtime method dispatch then resolves to the subclass implementation according to MRO. It is a core mechanism behind polymorphism.  
**Example**
```python
class A:
    def f(self): return "A"

class B(A):
    def f(self): return "B"
```

### 76. [HF][Trap] Does Python support method overloading?
Not in the traditional compile-time, signature-based way seen in languages like Java or C++. Redefining the same method name overwrites the earlier definition. Similar behavior is usually achieved via default arguments, `*args`, or explicit dispatch strategies.  
**Example**
```python
class A:
    def f(self, x, y=None):
        return x if y is None else x + y
```

### 77. [HF] What is polymorphism in Python?
Polymorphism means the same interface can work across objects of different concrete types. Python often achieves this through duck typing instead of strict inheritance. The strongest answer connects polymorphism with API design and substitutability, not just with class hierarchies.  
**Example**
```python
def render(obj):
    return obj.render()
```

### 78. What is encapsulation in Python?
Encapsulation is grouping data and behavior while controlling what is exposed. Python relies more on conventions and properties than on strict access modifiers. The point is disciplined interface design, not pretending private state is impossible to reach.  
**Example**
```python
class A:
    def __init__(self):
        self._internal = 1
        self.__mangled = 2
```

### 79. What is abstraction?
Abstraction means exposing essential behavior while hiding implementation detail behind stable interfaces. In Python, this can be expressed using abstract base classes, protocols, or simply well-designed callable contracts. Senior engineers use abstraction to reduce coupling and support evolution.  
**Example**
```python
from abc import ABC, abstractmethod

class Storage(ABC):
    @abstractmethod
    def save(self, item): ...
```

### 80. [HF] Instance method vs classmethod vs staticmethod?
Instance methods receive `self` and operate on object state. Classmethods receive `cls` and are ideal for alternative constructors or class-aware factories. Staticmethods receive neither and are just utilities logically grouped with the class.  
**Example**
```python
class User:
    def instance_method(self): pass

    @classmethod
    def from_dict(cls, d):
        return cls()

    @staticmethod
    def validate_age(age):
        return age >= 18
```

### 81. [HF] What is `super()`?
`super()` returns a proxy that delegates method lookup to the next class in the MRO. In multiple inheritance, this enables cooperative method calls across the hierarchy. Senior candidates should avoid the oversimplified phrase “calls the parent method,” because MRO may involve more than a direct parent.  
**Example**
```python
class A:
    def f(self): print("A")

class B(A):
    def f(self):
        super().f()
        print("B")
```

### 82. [HF] What is MRO?
MRO stands for Method Resolution Order and defines how Python searches classes for methods and attributes. In multiple inheritance, Python uses C3 linearization to keep resolution consistent and monotonic. This matters because it determines which method actually runs in complex hierarchies.  
**Example**
```python
class A: pass
class B(A): pass
class C(A): pass
class D(B, C): pass

print(D.__mro__)
```

### 83. What is duck typing?
Duck typing means an object is acceptable based on the behavior it provides, not the class hierarchy it belongs to. If it implements the required methods, it can often be used. This supports flexible extension points with low coupling.  
**Example**
```python
def read_all(src):
    return src.read()
```

### 84. [HF] What are dunder methods?
Dunder methods are special protocol methods such as `__init__`, `__iter__`, `__len__`, and `__repr__`. They let custom objects integrate with Python syntax and built-ins. Strong answers connect them to protocols and language integration rather than treating them as trivia.  
**Example**
```python
class Box:
    def __len__(self):
        return 5
```

### 85. What is the difference between `__init__` and `__new__`?
`__new__` is responsible for creating and returning the new instance, while `__init__` initializes that instance after creation. `__new__` matters mostly for immutable types or advanced instantiation control. Most business classes only need `__init__`.  
**Example**
```python
class A:
    def __new__(cls):
        return super().__new__(cls)
    def __init__(self):
        self.x = 1
```

### 86. [HF] `__repr__` vs `__str__`?
`__repr__` is for developer-facing, ideally unambiguous representation; `__str__` is for readable end-user display. If `__str__` is absent, Python falls back to `__repr__`. In production systems, a good `__repr__` improves logs and debugging significantly.  
**Example**
```python
class A:
    def __repr__(self): return "A()"
    def __str__(self): return "A object"
```

### 87. [HF] What is a property?
A property allows method-backed access with attribute syntax. It is useful for validation, computed fields, and maintaining backward compatibility while evolving internal implementation. Senior engineers often use properties to keep APIs stable while tightening invariants.  
**Example**
```python
class Temp:
    def __init__(self, c):
        self._c = c

    @property
    def celsius(self):
        return self._c
```

### 88. [HF] What is `__slots__`?
`__slots__` restricts instance attributes to a fixed set and can reduce memory usage by avoiding per-instance `__dict__`. It is useful when you create many lightweight objects. The trade-off is reduced flexibility, trickier inheritance, and some tooling limitations.  
**Example**
```python
class Point:
    __slots__ = ("x", "y")
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

### 89. [Senior][Design] Inheritance vs composition?
I prefer composition when behavior varies by capability, strategy, or collaboration rather than by a true subtype relationship. Inheritance is appropriate when substitutability is strong and shared lifecycle semantics are clear. In product systems, composition is usually easier to evolve and reason about.  
**Example**
```python
class Cache:
    def __init__(self, backend):
        self.backend = backend
```

### 90. [Senior] What is the attribute lookup chain in Python?
Attribute lookup usually checks the instance namespace first, then the class, then base classes according to MRO. If descriptors are involved, they can affect how attributes are resolved and bound. This matters when debugging unexpected shadowing, class-variable behavior, and property resolution.  
**Example**
```python
class A:
    role = "class"

a = A()
a.role = "instance"
print(a.role, A.role)
```

### 91. [Senior] What are descriptors at a high level?
A descriptor is an object that defines attribute access behavior using methods like `__get__`, `__set__`, or `__delete__`. Properties, bound methods, and many framework features rely on descriptors internally. At interview level, the key point is that descriptors power controlled attribute behavior in Python’s object model.  
**Example**
```python
class Positive:
    def __set_name__(self, owner, name):
        self.name = name
    def __get__(self, obj, owner):
        return obj.__dict__[self.name]
    def __set__(self, obj, value):
        if value < 0:
            raise ValueError("must be positive")
        obj.__dict__[self.name] = value
```

### 92. [Senior] What are protocols / structural typing at a high level?
Protocols describe behavior-based interfaces where compatibility depends on available methods/attributes rather than explicit inheritance. This matches Python’s duck-typing philosophy while still helping static tooling. In large codebases, protocols can improve flexibility without forcing deep inheritance trees.  
**Example**
```python
from typing import Protocol

class Reader(Protocol):
    def read(self) -> str: ...
```

### 93. [Senior] What are metaclasses at interview-summary level?
A metaclass controls how classes themselves are created, just as a class controls how instances are created. Most product code does not need custom metaclasses, but they matter in frameworks, ORMs, and DSL-like systems. A strong senior answer keeps this high-level and practical rather than overly theoretical.  
**Example**
```python
class MyMeta(type):
    pass
```

---

## 9) Decorators, Context Managers, and Cross-Cutting Behavior

### 94. [HF] What is a decorator?
A decorator is a callable that takes a function or class and returns a wrapped or modified version. It is commonly used for logging, caching, retries, authorization, metrics, and instrumentation. The senior point is that decorators help centralize cross-cutting concerns without duplicating boilerplate.  
**Example**
```python
def deco(fn):
    def wrapper(*a, **k):
        return fn(*a, **k)
    return wrapper
```

### 95. [HF][Trap] Why use `functools.wraps`?
`functools.wraps` preserves the wrapped function’s metadata such as name, docstring, annotations, and module. Without it, introspection, debugging, and tooling become misleading. This is a high-signal production-quality detail.  
**Example**
```python
from functools import wraps

def deco(fn):
    @wraps(fn)
    def wrapper(*a, **k):
        return fn(*a, **k)
    return wrapper
```

### 96. What is a higher-order function?
A higher-order function takes another function as an argument, returns one, or both. Decorators, callbacks, and utilities like `map` all fall into this category. This is possible because functions are first-class objects in Python.  
**Example**
```python
def apply(fn, x):
    return fn(x)
```

### 97. [HF] What is a context manager?
A context manager defines setup and teardown behavior around a block of code. It is used with `with` and guarantees cleanup even if an exception happens. This is the standard pattern for files, locks, transactions, and other resource lifecycles.  
**Example**
```python
with open("a.txt", "w") as f:
    f.write("hello")
```

### 98. [HF] How does `with` work internally?
The `with` statement calls `__enter__()` before the block and `__exit__()` after the block. If an exception occurs, Python passes exception information to `__exit__()`, which may suppress it by returning `True`. This gives deterministic cleanup and explicit lifecycle control.  
**Example**
```python
class CM:
    def __enter__(self):
        return self
    def __exit__(self, exc_type, exc, tb):
        return False
```

### 99. Decorator vs context manager?
A decorator wraps callable behavior, while a context manager wraps a specific runtime block with setup/teardown semantics. Decorators are better for reusable function-level concerns; context managers are better for scoped resource lifetime. The difference is behavioral wrapping versus block-scoped control.  
**Example**
```python
# decorator -> function behavior
# context manager -> resource/block lifecycle
```

### 100. [Senior] Where do decorators become risky in large systems?
Decorators become risky when they hide control flow, swallow exceptions, obscure signatures, alter idempotency semantics, or stack too much invisible behavior around critical code. In senior reviews, I look for transparency and observability. Cross-cutting power is useful only when the behavior remains understandable and testable.  
**Example**
```python
# avoid invisible retries around non-idempotent writes
```

---

## 10) Exceptions, Error Strategy, and Operational Robustness

### 101. [HF] How does exception handling work in Python?
Python uses `try`, `except`, `else`, and `finally` to separate normal logic from failure-handling logic. Exceptions propagate up the call stack until handled. Senior candidates should emphasize catching only what they can meaningfully handle.  
**Example**
```python
try:
    1 / 0
except ZeroDivisionError:
    print("bad")
```

### 102. [HF] `except Exception` vs bare `except`?
`except Exception` catches most application-level errors, while bare `except` also catches `KeyboardInterrupt` and `SystemExit`. Bare `except` is usually too broad and can hide operational signals or shutdown flows. Product code should almost always avoid it.  
**Example**
```python
try:
    risky()
except Exception as e:
    print(e)
```

### 103. What is the purpose of `else` in a `try` block?
`else` runs only if the `try` block completed without raising an exception. It helps keep success-path logic separate from statements that may fail, reducing accidental over-catching. That usually improves readability and correctness together.  
**Example**
```python
try:
    x = int("10")
except ValueError:
    print("bad input")
else:
    print(x)
```

### 104. [HF] Does `finally` always execute?
`finally` is intended to execute whether or not an exception occurs and is the normal place for cleanup. In ordinary execution, it is effectively guaranteed. Extreme events like process termination can still prevent it, so “always” is only practically true, not absolutely universal.  
**Example**
```python
try:
    print("work")
finally:
    print("cleanup")
```

### 105. [HF] How do you raise exceptions properly?
Use `raise` with a specific exception instance or class that accurately communicates the failure. Good exception design improves debuggability and supports precise handling in calling code. Generic exceptions should be a last resort, not the default.  
**Example**
```python
if age < 0:
    raise ValueError("age cannot be negative")
```

### 106. What is exception chaining?
Exception chaining preserves the original failure when raising a higher-level exception using `raise NewError(...) from e`. This is valuable in layered systems because it maintains debugging context without leaking raw low-level details into the API surface.  
**Example**
```python
try:
    int("x")
except ValueError as e:
    raise RuntimeError("parse failed") from e
```

### 107. What is a custom exception?
A custom exception is a user-defined exception type derived from `Exception` or one of its subclasses. It lets the domain express meaningful failure categories explicitly. That improves clarity compared to recycling unrelated built-in exceptions for business errors.  
**Example**
```python
class PaymentError(Exception):
    pass
```

### 108. [Senior] When do you prefer exceptions over return codes?
I use exceptions when the situation breaks normal control flow or needs to propagate across layers. I prefer return values when “absence” or “not found” is a normal business outcome rather than an error. The senior judgment is to keep contracts explicit and easy for callers to reason about.  
**Example**
```python
def parse_int(s: str) -> int:
    return int(s)
```

### 109. [Senior] What makes exception handling poor in production code?
Poor exception handling catches too broadly, loses context, retries blindly, logs without enough metadata, or swallows failures silently. Good error handling preserves context, distinguishes expected from unexpected failures, and supports observability. At senior level, operational clarity matters as much as local correctness.  
**Example**
```python
# anti-pattern:
# except Exception:
#     pass
```

### 110. [Senior] What does observability-minded error handling look like?
It means exceptions carry enough context to debug, logs include identifiers and boundaries, and error paths are instrumented without hiding root causes. It also means separating transient failures from deterministic ones and making retry semantics explicit. Senior teams design error handling so operators can act on it, not just developers read stack traces.  
**Example**
```python
# log order_id, user_id, operation, and failure class together
```

### 111. [Senior] Why does idempotency matter when discussing retries?
Retries are safe only when repeating the operation will not create duplicate side effects or corruption. Network calls, payments, and state transitions often need idempotency keys or carefully designed semantics. This is a high-signal production topic for senior candidates because it connects Python code with system reliability.  
**Example**
```python
# retrying "charge card" is unsafe without idempotency guarantees
```

---

## 11) Modules, Packages, Imports, and Code Organization

### 112. What is a module?
A module is a single Python file containing code, definitions, and executable statements. Importing a module executes its top-level code once and stores the loaded module in `sys.modules`. Modules are Python’s basic unit of reuse and namespacing.  
**Example**
```python
import math
print(math.sqrt(16))
```

### 113. What is a package?
A package is a directory structure that groups related modules. Traditional packages use `__init__.py`, though namespace packages can omit it. Packages help large codebases organize capabilities and ownership more clearly.  
**Example**
```python
# app/
#   __init__.py
#   service.py
```

### 114. `import module` vs `from module import name`?
`import module` keeps access qualified and makes origin explicit, which improves readability and avoids name collisions. `from module import name` is shorter but can make names less traceable. In larger codebases, explicit module imports are often the safer default.  
**Example**
```python
import math
from math import pi
```

### 115. What does `if __name__ == "__main__"` mean?
When a file is run directly, `__name__` is set to `"__main__"`. When it is imported as a module, `__name__` is set to the module name. This lets a file support both reusable definitions and script-style execution.  
**Example**
```python
if __name__ == "__main__":
    print("run directly")
```

### 116. [Trap] What are circular imports?
Circular imports happen when modules depend on each other during import time. That can leave one or both modules partially initialized and cause confusing runtime errors. The long-term fix is almost always improving module boundaries rather than adding import hacks.  
**Example**
```python
# a imports b; b imports a
```

### 117. What is a virtual environment?
A virtual environment isolates Python packages per project so dependency versions do not collide across applications. It improves reproducibility in development, testing, and deployment. This is standard Python engineering hygiene.  
**Example**
```bash
python -m venv .venv
```

### 118. What is the role of `requirements.txt`?
`requirements.txt` records dependency versions so environments can be recreated consistently. It supports deterministic builds, local setup, CI, and deployment workflows. The senior angle is dependency control and reproducibility under change.  
**Example**
```txt
fastapi==0.115.0
pytest==8.3.0
```

### 119. [Senior][Design] How do you structure Python modules in large services?
I organize by capability and clear boundaries, minimize circular dependencies, keep public interfaces explicit, and avoid top-level side effects. I want module structure to support testability, ownership, and safe change. Senior code organization is about reducing accidental coupling as the system grows.  
**Example**
```python
# billing/
#   api.py
#   domain.py
#   repository.py
```

### 120. [Senior] How do you evolve Python APIs backward-compatibly?
I prefer additive changes over breaking ones, keyword-only parameters for clarity, deprecation windows for behavioral shifts, and stable return contracts. I also avoid positional-argument traps in public APIs. At senior level, API evolution is as much about consumer safety as elegance.  
**Example**
```python
def fetch_user(user_id: int, *, include_orders: bool = False):
    ...
```

---

## 12) File Handling, Serialization, and Boundary Design

### 121. [HF] How does file handling work in Python?
Files are opened with `open()` in text or binary mode, and they should usually be managed with `with` for deterministic closure. File operations can raise I/O-related exceptions that should be handled at the right boundary. Senior answers should mention explicit encoding choices instead of relying on environment defaults.  
**Example**
```python
with open("data.txt", "r", encoding="utf-8") as f:
    data = f.read()
```

### 122. Text mode vs binary mode?
Text mode reads/writes strings and performs encoding or decoding. Binary mode reads/writes raw bytes with no text translation. This distinction matters because treating raw bytes as text is a common real-world source of bugs.  
**Example**
```python
with open("image.png", "rb") as f:
    raw = f.read()
```

### 123. What is pickling, and what are the risks?
Pickling serializes Python objects into a byte stream and unpickling reconstructs them. It is Python-specific and unsafe for untrusted input because deserialization can trigger arbitrary code execution paths. Senior candidates should call out that security concern immediately.  
**Example**
```python
import pickle
blob = pickle.dumps({"a": 1})
print(pickle.loads(blob))
```

### 124. JSON vs pickle?
JSON is text-based, language-neutral, and well-suited for interoperable APIs and storage of basic data types. Pickle supports richer Python object graphs but is Python-specific and unsafe for untrusted input. For service boundaries, JSON is usually the safer default.  
**Example**
```python
import json
payload = json.dumps({"a": 1})
print(json.loads(payload))
```

### 125. [Senior][Design] How do you think about serialization boundaries?
At service boundaries, I prefer explicit, stable, language-neutral formats such as JSON or schema-based formats like Protobuf when needed. I avoid leaking internal Python object models across boundaries. Senior design here is about compatibility, debuggability, and long-term contract safety.  
**Example**
```python
event = {"user_id": 101, "action": "login"}
```

---

## 13) Memory, GC, Performance, and Concurrency

### 126. [HF] How does Python manage memory?
CPython primarily uses reference counting for immediate object lifetime tracking and a cyclic garbage collector for unreachable reference cycles. Small-object allocation is optimized with allocators like `pymalloc`. The senior answer should clearly separate reference counting from cyclic GC.  
**Example**
```python
a = []
b = a
del a
```

### 127. [HF] What is reference counting?
Reference counting tracks how many references point to an object. When that count reaches zero, CPython can usually reclaim the object immediately. It gives predictable behavior for many objects but does not handle cycles on its own.  
**Example**
```python
import sys
x = []
print(sys.getrefcount(x))
```

### 128. [HF] What is garbage collection in Python?
Garbage collection in CPython mainly means detecting and reclaiming unreachable reference cycles. It complements reference counting rather than replacing it. This matters when objects keep each other alive despite no remaining useful references.  
**Example**
```python
import gc
gc.collect()
```

### 129. [HF][Trap] What is a reference cycle?
A reference cycle occurs when objects reference one another and keep their reference counts above zero even though the rest of the program can no longer reach them. Reference counting alone cannot reclaim such structures. That is exactly why CPython also has cyclic GC.  
**Example**
```python
a = []
b = [a]
a.append(b)
```

### 130. [Senior] Can Python still leak memory?
Yes. Logical leaks still happen when applications retain references longer than needed, such as through caches, global registries, unbounded queues, or complex object graphs. GC helps with some cases, but it cannot fix poor lifetime design or uncontrolled retention.  
**Example**
```python
cache = []
def save(item):
    cache.append(item)
```

### 131. [HF] What is the GIL?
The Global Interpreter Lock is a mutex in CPython that allows only one thread to execute Python bytecode at a time. It simplifies interpreter internals and reference-management logic. Its practical consequence is that CPU-bound Python threads do not scale across cores the way separate processes can.  
**Example**
```python
# CPU-bound threads are limited in CPython
```

### 132. [HF][Trap] Does the GIL make Python single-threaded?
No. Python supports threads, but in CPython only one thread executes Python bytecode at a given moment. Threads still help with I/O-bound workloads because the interpreter can release the GIL around blocking I/O.  
**Example**
```python
# network I/O can benefit from threads
```

### 133. [HF] Multithreading vs multiprocessing?
Threads share memory and are lighter weight, but CPU-bound bytecode execution is constrained by the GIL. Processes have separate memory spaces and allow true parallel CPU execution, but with higher startup and inter-process communication costs. The correct choice depends on workload and data-sharing needs.  
**Example**
```python
from multiprocessing import Process
```

### 134. [HF] When do you use threading vs multiprocessing?
I use threading for I/O-bound concurrency such as HTTP fan-out, file/network waits, and external service calls. I use multiprocessing for CPU-bound work such as heavy parsing, transformation, or numerical tasks. Senior answers should also mention serialization overhead and coordination cost between processes.  
**Example**
```python
# threading -> I/O fan-out
# multiprocessing -> CPU-heavy transforms
```

### 135. [Senior] Thread safety vs process safety?
Thread safety is about correct behavior under concurrent access within one shared memory space. Process safety is about correctness when multiple processes interact, usually through files, databases, queues, or IPC rather than shared memory. Senior engineers should not assume a design safe for one model is safe for the other.  
**Example**
```python
# a plain in-memory lock does not coordinate across processes
```

### 136. [Senior][Perf] How do you reason about Python performance issues?
I first determine whether the problem is CPU-bound, I/O-bound, memory-bound, or serialization-bound. Then I profile before optimizing, because intuition is often wrong. Senior performance work is about identifying the true hotspot, choosing the right data structures, reducing unnecessary allocations/copies, and only then considering architectural changes.  
**Example**
```python
# profile first, optimize second
```

---

## 14) Standard Library Tools, Dataclasses, and Typing

### 137. [HF] What is `Counter`?
`collections.Counter` is a dictionary subclass specialized for counting hashable objects. It is ideal for frequency analysis and offers helpful methods like `most_common()`. It is one of the most practical interview tools in Python.  
**Example**
```python
from collections import Counter
print(Counter("banana"))
```

### 138. [HF] What is `defaultdict`?
`defaultdict` automatically creates missing keys using a factory function. It removes repetitive missing-key checks and is especially useful for grouping and graph-like structures. It is often cleaner than repeated `setdefault` calls.  
**Example**
```python
from collections import defaultdict
d = defaultdict(list)
d["a"].append(1)
```

### 139. [HF] What is `deque`?
`deque` is a double-ended queue optimized for O(1) appends and pops from both ends. It is a better fit than list for queue semantics, BFS, and sliding-window problems. Repeated left pops from a list are inefficient because they shift elements.  
**Example**
```python
from collections import deque
q = deque([1, 2])
q.appendleft(0)
```

### 140. What is `namedtuple`?
`namedtuple` creates lightweight immutable record types with named fields. It gives more readability than raw tuples without the full ceremony of a custom class. It is useful when you want structured, low-overhead data containers.  
**Example**
```python
from collections import namedtuple
Point = namedtuple("Point", ["x", "y"])
print(Point(1, 2).x)
```

### 141. [HF] What is a dataclass?
A dataclass auto-generates common methods like `__init__`, `__repr__`, and `__eq__` from annotated fields. It is a strong fit for data-centric models where the main value is state rather than heavy behavior. Senior engineers use dataclasses when they improve clarity rather than by habit.  
**Example**
```python
from dataclasses import dataclass

@dataclass
class User:
    id: int
    name: str
```

### 142. [Trap] What does `frozen=True` do in a dataclass?
`frozen=True` prevents attribute reassignment and gives instances immutable-like behavior. It can improve safety and support hashability in some cases. The subtle point is that it does not make nested mutable members deeply immutable.  
**Example**
```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Point:
    x: int
    y: int
```

### 143. What are type hints?
Type hints are optional annotations describing expected types for parameters, returns, and sometimes variables. Python does not enforce them at runtime by default, but they improve static analysis, IDE feedback, refactoring safety, and documentation. In large teams, they are a communication tool as much as a correctness tool.  
**Example**
```python
def add(a: int, b: int) -> int:
    return a + b
```

### 144. [Senior] Why do type hints matter more in large codebases?
They make interfaces clearer, reduce refactoring risk, help tools catch mismatches earlier, and improve discoverability across team boundaries. In senior codebases, type hints often act as lightweight contracts that reduce ambiguity without giving up Python’s flexibility. They are especially valuable around shared abstractions and public APIs.  
**Example**
```python
from typing import Iterable

def total(nums: Iterable[int]) -> int:
    return sum(nums)
```

---

## 15) Patterns, Sorting, and High-Signal Gotchas

### 145. What is monkey patching?
Monkey patching means modifying a module, class, or function at runtime after it was originally defined. It can be useful in tests or framework integration points, but it is risky because behavior changes become implicit and global. Senior engineers use it sparingly and intentionally.  
**Example**
```python
import math
math.pi = 3
```

### 146. [HF] What is memoization?
Memoization caches function results for repeated inputs to avoid recomputation. It is effective for pure functions and overlapping-subproblem recursion. Python’s `functools.lru_cache` makes this easy and readable.  
**Example**
```python
from functools import lru_cache

@lru_cache(None)
def fib(n):
    return n if n < 2 else fib(n-1) + fib(n-2)
```

### 147. [HF] What is sorting stability?
A stable sort preserves the original relative order of elements with equal keys. Python’s sort is stable, which makes multi-level sorting straightforward. Timsort also performs well on partially ordered real-world data, which is one reason it is so effective in practice.  
**Example**
```python
data = [("a", 2), ("b", 2), ("c", 1)]
print(sorted(data, key=lambda x: x[1]))
```

### 148. [HF] `sort()` vs `sorted()`?
`list.sort()` sorts a list in place and returns `None`, while `sorted()` returns a new sorted list from any iterable. The choice is about mutation, memory, and preserving original input. In product code, this often matters for clarity as much as efficiency.  
**Example**
```python
a = [3, 1, 2]
b = sorted(a)
a.sort()
```

### 149. What does the `key` parameter in sorting do?
The `key` function computes a comparison key once per element, and sorting is performed using those keys. This is typically cleaner and more efficient than writing custom comparison logic. Senior answers should mention that readability is usually the bigger win.  
**Example**
```python
words = ["aaa", "b", "cc"]
print(sorted(words, key=len))
```

### 150. Why is `range(len(seq))` often discouraged?
If you only need values, iterate directly over the sequence. If you need both index and value, `enumerate()` is clearer and less error-prone. `range(len(seq))` is best reserved for cases where index arithmetic or positional updates really matter.  
**Example**
```python
for i, v in enumerate(["a", "b"]):
    print(i, v)
```

### 151. [HF] What is unpacking with `*` and `**`?
`*` unpacks iterables into positional arguments or sequence literals, while `**` unpacks mappings into keyword arguments or merged dictionaries. This is common in wrappers, delegation, and config composition. Senior engineers should use it to clarify forwarding, not to hide messy signatures.  
**Example**
```python
nums = [1, 2]
print(*nums)

a = {"x": 1}
b = {"y": 2}
print({**a, **b})
```

### 152. Why is modifying a list during iteration dangerous?
Because the iterator’s view of progress can become inconsistent with the mutated container, causing items to be skipped or processed unpredictably. This is a frequent source of subtle bugs. The safer approach is iterating over a copy or building a new result list.  
**Example**
```python
a = [1, 2, 3, 4]
for x in a[:]:
    if x % 2 == 0:
        a.remove(x)
```

### 153. What is a sentinel object?
A sentinel is a unique marker used to distinguish “not provided” from a meaningful value like `None`. It is useful when `None` itself is a valid input. The usual pattern is to create one with `object()`.  
**Example**
```python
_MISSING = object()

def f(x=_MISSING):
    if x is _MISSING:
        print("not provided")
```

### 154. [HF][Trap] What is the common bug with mutable class attributes?
A mutable class attribute is shared across all instances unless shadowed by an instance attribute. That can accidentally create cross-instance state sharing. It is a classic interview trap because it tests both mutability and attribute lookup rules.  
**Example**
```python
class A:
    items = []
```

### 155. What is `heapq` used for?
`heapq` implements a binary min-heap using a list. It is useful for priority queues and problems requiring repeated retrieval of the smallest element. Push and pop are O(log n), while access to the minimum element is O(1).  
**Example**
```python
import heapq
h = [3, 1, 2]
heapq.heapify(h)
print(heapq.heappop(h))
```

### 156. What is `bisect` used for?
`bisect` performs binary search on sorted lists to find insertion points efficiently. It is useful when maintaining sorted order incrementally. Search is O(log n), but insertion into a list remains O(n) because elements must shift.  
**Example**
```python
import bisect
a = [1, 3, 5]
print(bisect.bisect_left(a, 4))
```

### 157. [Senior] What does “Pythonic” mean at senior level?
Pythonic code is not just short; it is readable, explicit, maintainable, and aligned with Python’s strengths and standard library. At senior level, Pythonic also means easy to debug, hard to misuse, and sensible for the team that will maintain it. Clever one-liners are not Pythonic if they reduce clarity.  
**Example**
```python
active_ids = [u.id for u in users if u.active]
```

---

## 16) Testing, Debugging, and Maintainability

### 158. What is unit testing?
Unit testing verifies small isolated units of behavior, usually functions or methods. Its purpose is correctness, regression prevention, and safer refactoring. In senior interviews, testing is also a signal of engineering discipline and change safety.  
**Example**
```python
def add(a, b):
    return a + b

assert add(2, 3) == 5
```

### 159. Assertion vs exception?
An assertion checks internal assumptions and may be disabled in optimized runs. An exception represents a runtime error condition that calling code may need to handle or propagate. Assertions should not be used as production input validation.  
**Example**
```python
assert 2 + 2 == 4
```

### 160. What is mocking?
Mocking replaces real dependencies with controllable test doubles so the unit under test is isolated from external systems or nondeterministic behavior. It is useful for APIs, databases, clocks, and filesystem interactions. The senior nuance is to mock boundaries without over-coupling tests to implementation details.  
**Example**
```python
# replace a payment client with a mock during tests
```

### 161. [Senior] What do strong debugging answers sound like?
Strong debugging answers are structured: reproduce the issue, narrow the scope, inspect assumptions, verify data shape, check recent changes, use logs/metrics/traces, and isolate the failing boundary. Senior debugging is hypothesis-driven and observability-aware, not random experimentation. Interviewers often judge maturity heavily from this.  
**Example**
```python
# reproduce -> localize -> inspect inputs -> confirm fix with test
```

### 162. [Senior] What maintainability signals matter in Python interviews?
Interviewers look for clear naming, API discipline, testability, low hidden coupling, good error boundaries, and appropriate standard-library use. They also care whether your code remains understandable six months later under team ownership. Senior Python is as much about operational maintainability as language fluency.  
**Example**
```python
def build_invoice_summary(invoice: dict) -> dict:
    ...
```

---

## 17) Top 35 Must-Master Senior Python Questions for Product Companies

1. `is` vs `==`  
2. Mutable vs immutable  
3. Hashability and dict keys  
4. List vs tuple  
5. Set vs frozenset  
6. Dict internals and average-case complexity  
7. Shallow copy vs deep copy  
8. Ownership vs copying  
9. Mutable default argument trap  
10. Function API design with keyword-only args  
11. `*args` / `**kwargs` trade-offs  
12. LEGB rule  
13. Closure and late binding  
14. Iterable vs iterator  
15. Generator vs list comprehension  
16. Iterator exhaustion  
17. Decorators and `functools.wraps`  
18. Context managers and `with`  
19. `classmethod` vs `staticmethod`  
20. Inheritance vs composition  
21. MRO and `super()`  
22. `__repr__` vs `__str__`  
23. `__slots__` trade-offs  
24. Descriptors at a high level  
25. Protocols / structural typing  
26. Exception strategy and exception chaining  
27. Observability-minded error handling  
28. Idempotency and retries  
29. Reference counting and cyclic GC  
30. GIL  
31. Threading vs multiprocessing  
32. Thread safety vs process safety  
33. Sorting stability and Timsort  
34. `defaultdict`, `Counter`, and `deque`  
35. Type hints and backward-compatible API evolution

---

## Final Senior Interview Rule

For Python interviews at the **8–12 years** level, the strongest answers usually sound like this:

1. **Precise definition**  
2. **Internal rule or runtime behavior**  
3. **Trade-off / edge case**  
4. **Production implication**  
5. **Tiny example**

That structure makes the answer sound like it comes from someone who has not only used Python, but built, debugged, and evolved Python systems in production.
