# Core Python — Senior Interview Preparation
### Product-Based Company Edition

> **How to use:** every topic gives the interview question, the answer you are expected to give, runnable code, and the follow-up probe that separates a good answer from a great one. All code targets Python 3.10+.
>
> **Companion file:** `python_dsa_interview_topics.md` covers data structures and algorithms.

---

## 1. Python Fundamentals and Execution Model

### Introduction to Python — features, interpreted vs compiled, implementations
**Q: Is Python interpreted or compiled?**
Both. CPython compiles source to **bytecode** (`.pyc` in `__pycache__`), then the Python Virtual Machine interprets that bytecode. It is not compiled to machine code ahead of time, which is why it is slower than C but portable and introspectable.
```python
import dis
def add(a, b):
    return a + b
dis.dis(add)       # LOAD_FAST a, LOAD_FAST b, BINARY_OP +, RETURN_VALUE
```
**Implementations:** CPython (reference, C), PyPy (JIT, much faster for long-running pure Python), Jython (JVM), IronPython (.NET).
**Probe:** *"Does PyPy have a GIL?"* — Yes, but its JIT makes single-threaded code far faster. The GIL is a **CPython implementation detail**, not part of the language.

### Running Python code — interactive, script, module
Three modes, and the difference matters for imports and `__name__`.
```python
# python                 -> interactive REPL
# python app.py          -> script mode,  __name__ == "__main__"
# python -m package.app  -> module mode,  runs package __init__ first, sys.path differs
```
**Probe:** *"Why does `-m` fix some relative-import errors?"* Because it runs the file **as part of its package**, so relative imports resolve. Running the same file directly gives `ImportError: attempted relative import with no known parent package`.

### Source structure — indentation, statements, comments, docstrings
Indentation is syntax, not style; a docstring is the first string literal in a module, function, or class and is retrievable at runtime.
```python
def area(radius: float) -> float:
    "Return circle area. Accessible at runtime as area.__doc__."
    return 3.14159 * radius ** 2

print(area.__doc__)
```
**Probe:** *"What does `python -OO` do to docstrings?"* Strips them. `-O` additionally removes `assert` statements — so never use `assert` for validation of real input.

### Variables and naming — dynamic typing, rules, conventions
Names are **bound to objects**; types live on the object, not the name. Nothing is declared.
```python
x = 42          # x -> int object
x = "now a str" # rebinding, not reassignment of a typed slot
```
**Conventions (PEP 8):** `snake_case` functions/variables, `PascalCase` classes, `UPPER_SNAKE` constants, `_private` convention, `__mangled` name mangling, `__dunder__` reserved.
**Probe:** *"What are the practical costs of dynamic typing at scale?"* Errors move from compile time to runtime; refactoring is riskier; mitigate with type hints checked in CI, not with care.

### Basic operators — arithmetic, comparison, logical, assignment, membership, identity
```python
print(7 / 2)      # 3.5   true division - always float
print(7 // 2)     # 3     floor division
print(-7 // 2)    # -4    floors toward NEGATIVE infinity, not toward zero
print(-7 % 2)     # 1     sign follows the DIVISOR
print(2 ** 10)    # 1024
x = 2
print(1 < x < 3)          # chained: evaluates x ONCE, short-circuits
print("a" in "cat")       # membership
print(x is None)          # identity - never use == for None
```
**Probe:** *"What does `-7 // 2` give, and why does it matter?"* `-4`. Many algorithm problems expect truncation toward zero — use `int(-7/2)` or `math.trunc` for that. A classic calculator-problem bug.

### Input and output — print, input, string formatting
```python
name, score = "Ada", 99.456
print(f"{name} scored {score:.1f}")          # f-string: fastest, clearest (3.6+)
print("{} scored {:.1f}".format(name, score))
print("%s scored %.1f" % (name, score))      # legacy; still used by logging
print("a", "b", sep="-", end="!\n")
print(f"{score=}")                            # 'score=99.456'  debug helper (3.8+)
```
**Probe:** *"Why use `logger.info('x=%s', x)` instead of an f-string?"* The f-string formats eagerly even when the log level filters the record out; `%s` defers formatting to the handler, and keeps the raw template for structured logging.

### Control flow — if/elif/else, for, while, break, continue, pass
```python
for i in range(3):
    if i == 1:
        continue
    print(i)
else:
    print("completed without break")   # runs ONLY if no break fired
```
**Probe:** *"When does the `for...else` clause run?"* When the loop completes **without** `break` — including on an empty iterable. It is not "else if the body never ran". Useful for search-with-fallback; many style guides discourage it because readers misread it.

### Truthiness and falsiness
Falsy: `None`, `False`, `0`, `0.0`, `""`, `()`, `[]`, `{}`, `set()`, `range(0)`. Custom objects consult `__bool__`, falling back to `__len__`.
```python
class Box:
    def __init__(self, items): self.items = items
    def __len__(self): return len(self.items)
print(bool(Box([])))      # False - __len__ returned 0
```
**Probe:** *"Why is `if x:` different from `if x is not None:`?"* `if not count:` wrongly fires when `count == 0`; `if not items:` cannot distinguish "empty list" from "no list supplied". Use `is None` whenever absent and empty mean different things.

### Python execution model — bytecode, .pyc, PVM
Source is parsed to an AST, compiled to bytecode, cached in `__pycache__`, then executed by a stack-based VM.
```python
import sys
print(sys.implementation.name, sys.version_info[:2])
```
**Probe:** *"Is the `.pyc` a speed optimisation?"* Only for **import** time — it skips recompilation. It does not make execution faster.

### Keywords, built-ins, and the Zen of Python
```python
import keyword, builtins
print(len(keyword.kwlist))     # reserved words - cannot be used as names
print(len(dir(builtins)))      # built-in namespace - the final LEGB fallback
# import this                  # The Zen of Python
```
**Probe:** *"What happens if you name a variable `list`?"* You shadow the builtin in that scope. Later `list(...)` raises `TypeError: 'list' object is not callable` — and at module level it poisons the whole module. Linters flag this (ruff `A001`).

---

## 2. Data Types and Mutability

### Numeric types — int, float, complex, bool
Python `int` is **arbitrary precision** (no overflow); `float` is IEEE-754 double; `bool` subclasses `int`.
```python
import math
print(2 ** 200)                      # exact - no overflow
print(0.1 + 0.2 == 0.3)              # False -> 0.30000000000000004
print(math.isclose(0.1 + 0.2, 0.3))  # True - correct float comparison
print(isinstance(True, int))         # True  - bool IS an int
print(sum([True, False, True]))      # 2     - useful for counting
print({1: "one", True: "yes"})       # {1: 'yes'} - True collides with 1
```
**Probe:** *"When do you use `Decimal` instead of `float`?"* Money, always — or integer minor units. `Decimal` is base-10 with exact decimal fractions and explicit rounding; `float` cannot represent `0.1`.

### Sequence types — str, list, tuple, range
```python
s = "hello"        # immutable
l = [1, 2, 3]      # mutable, dynamic array of pointers
t = (1, 2, 3)      # immutable, fixed record
r = range(1000000) # lazy, O(1) memory
```
**Probe:** *"Tuple vs list beyond immutability?"* Tuples signal a fixed heterogeneous **record** and are hashable (if their elements are), so they work as dict keys; lists signal a homogeneous varying-length **sequence**.

### Set types — set, frozenset
```python
a, b = {1, 2, 3}, {3, 4}
print(a & b, a | b, a - b, a ^ b)   # intersection, union, difference, symmetric
fs = frozenset([1, 2])              # hashable -> usable as a dict key
print({fs: "ok"})
```

### Mapping type — dict
```python
d = {"a": 1}
print(d.get("b", 0))           # default without inserting
d.setdefault("c", [])          # insert-if-absent, returns the value
print({**d, "e": 5})           # merge
print(d | {"f": 6})            # 3.9+ union operator
```
**Probe:** *"What does dict ordering guarantee?"* Insertion order is a **language guarantee** since 3.7. It does **not** give sorted order, and two dicts with different insertion order still compare equal. Reinserting a deleted key moves it to the end.

### Binary types — bytes, bytearray, memoryview
`bytes` is immutable, `bytearray` mutable, `memoryview` a zero-copy window.
```python
b = "hello".encode("utf-8")       # str -> bytes
print(b.decode("utf-8"))          # bytes -> str
ba = bytearray(b); ba[0] = 72
mv = memoryview(ba)               # slicing mv does NOT copy
print(bytes(mv[:2]))
```
**Probe:** *"What causes `UnicodeDecodeError`?"* The byte sequence is invalid for the assumed codec. Fix the source assumption; `errors='replace'` silently corrupts data.

### None type
A singleton, the default return value, and the correct "absent" sentinel.
```python
def f(): pass
print(f() is None)        # True
```

### Type inspection — type(), isinstance()
```python
print(type(True) is int)          # False - exact type
print(isinstance(True, int))      # True  - respects inheritance
print(isinstance(3, (int, float)))
```
**Probe:** *"Which do you use and why?"* `isinstance` almost always, because it respects subclassing and duck-typed ABCs. `type(x) is C` only when you must reject subclasses.

### Mutable vs immutable, object identity
```python
a = [1, 2]; b = a          # same object - alias
b.append(3)
print(a)                   # [1, 2, 3] - mutation is visible
b = [9]                    # rebinding - a is unaffected
print(a, id(a) == id(b))
```
**Probe:** *"Is Python pass-by-value or pass-by-reference?"* Neither — **call by sharing**. The callee binds its own name to the caller's object: rebinding is invisible, mutating is visible.

### String operations — indexing, slicing, methods
```python
s = "interview"
print(s[0], s[-1], s[1:4], s[::-1])
print("a  b".split(), "a  b".split(" "))      # ['a','b'] vs ['a','','b']
print("example.com".removesuffix(".com"))     # 3.9+ exact suffix -> 'example'
print("commerce.com".strip(".com"))           # 'erce' - strip takes a CHAR SET!
print("".join(["a", "b"]))                    # O(n) - correct string building
```
**Probe:** *"Why is `s += x` in a loop a problem?"* Strings are immutable, so each `+=` allocates and copies: O(n^2). Build a list and `join` once.

### List operations — indexing, slicing, methods, nested lists
```python
xs = [3, 1, 2]
xs.append(4); xs.pop(); xs.sort()
print(xs, sorted(xs, reverse=True))
print(xs[1:])                            # COPIES - O(k)
grid = [[0] * 3 for _ in range(3)]       # correct
bad  = [[0] * 3] * 3                     # 3 refs to ONE row
bad[0][0] = 1
print(bad)                               # [[1,0,0],[1,0,0],[1,0,0]] - aliasing bug
```
**Probe:** *"What does `list.sort()` return?"* `None` — it mutates in place. This is the Python convention for mutating methods (`append`, `extend`, `reverse`, `update`, `shuffle`), specifically so `x = y.sort()` cannot look like it produced a sorted copy.

### Tuple behaviour — packing, unpacking, singleton
```python
t = 1, 2, 3                 # packing
a, b, c = t                 # unpacking
first, *rest = [1, 2, 3]    # rest is a LIST -> [2, 3]
single = (1,)               # trailing comma makes it a tuple; (1) is just int
a, b = b, a                 # swap without a temp
print(first, rest, type(single).__name__)
```

### Set behaviour — uniqueness, operations
```python
nums = [3, 1, 3, 2]
print(list(dict.fromkeys(nums)))      # [3,1,2] - dedupe PRESERVING order
print(len(set(nums)) < len(nums))     # has duplicates, O(n)
```
**Probe:** *"Why prefer a set over a list for membership?"* `in` is O(1) on a set versus O(n) on a list — the single highest-value one-line optimisation in interviews.

### Dictionary behaviour — keys, values, items, methods, insertion order
```python
from collections import defaultdict, Counter
d = {"a": 1, "b": 2}
for k, v in d.items(): pass
groups = defaultdict(list); groups["x"].append(1)
print(Counter("mississippi").most_common(2))      # [('i',4), ('s',4)]
```
**Probe:** *"What's the `defaultdict` trap?"* A plain **read** of a missing key inserts it, silently changing `len()` and iteration. Use `.get()` when you only want a default on read.

### Type conversion — implicit vs explicit
```python
print(1 + 2.0)                 # 3.0 implicit numeric widening
print(int("42"), str(42), list("abc"), dict([("a", 1)]))
# int("4.2")                   # ValueError - no implicit str->float->int chain
```

### Memory references through objects
```python
import sys
a = [1, 2, 3]
print(sys.getrefcount(a))      # refcount (inflated by one for the call argument)
```

---

## 3. Functions and Argument Handling

### Defining functions, function objects, parameters vs arguments
Functions are first-class objects with attributes; **parameters** are in the definition, **arguments** at the call site.
```python
def greet(name: str) -> str:      # name = parameter
    return f"Hi {name}"
print(greet("Ada"))               # "Ada" = argument
print(greet.__name__, greet.__defaults__)
greet.custom = 1                  # functions carry attributes
```

### Types of arguments — positional, keyword, default, *args, **kwargs
```python
def f(a, b=2, *args, c, **kwargs):
    return a, b, args, c, kwargs
print(f(1, 3, 4, 5, c=6, d=7))      # (1, 3, (4,5), 6, {'d':7})
```
Order is fixed: positional-only `/`, positional-or-keyword, `*args`, keyword-only, `**kwargs`.

### Keyword-only and positional-only arguments
```python
def resize(img, /, width, *, upscale=False):
    return img, width, upscale
# img     : positional-only - renaming it is not a breaking change
# width   : positional or keyword
# upscale : keyword-only - call sites must name it, so it reads clearly
print(resize("i", 10, upscale=True))
```
**Probe:** *"Why would an API author want keyword-only arguments?"* They prevent boolean traps (`do(x, True, False)`), and let you add parameters later without breaking positional callers. One of the highest-value API habits in Python.

### Argument unpacking
```python
def f2(a, b, c): return a + b + c
args, kw = (1, 2), {"c": 3}
print(f2(*args, **kw))              # unpack AT THE CALL SITE
print([*[1], *[2, 3]], {**{"a": 1}, **{"b": 2}})
```

### Return values — single, multiple, tuples
```python
def stats(xs):
    return min(xs), max(xs)     # returns a tuple
lo, hi = stats([3, 1, 4])
print(lo, hi)
```
**Probe:** *"Tuple vs dict vs object as a return type?"* Tuple for 2–3 values consumed immediately (adding a field breaks unpacking); dict for flexible keys but no schema and silent typos; **dataclass/NamedTuple past two fields** — self-documenting, typed, extensible.

### First-class functions
```python
ops = {"add": lambda a, b: a + b}            # stored in a dict
def apply(fn, x): return fn(x)               # passed as an argument
def make_adder(n): return lambda x: x + n    # returned
print(ops["add"](1, 2), apply(abs, -3), make_adder(5)(3))
```

### Anonymous functions — lambda
A single expression; no statements, no annotations, no docstring, and `__name__ == '<lambda>'` which degrades tracebacks.
```python
print(sorted(["ccc", "a", "bb"], key=lambda w: len(w)))
# f = lambda x: x    <- PEP 8 says use def instead when naming it
```

### Recursion
```python
import sys
print(sys.getrecursionlimit())        # 1000 - no tail-call optimisation
def fact(n): return 1 if n <= 1 else n * fact(n - 1)
print(fact(10))
```
**Probe:** *"What breaks past 1000 frames?"* `RecursionError`. Convert to iterative rather than raising the limit — raising it risks a hard interpreter crash.

### Function annotations
Metadata only; Python never enforces them at runtime.
```python
def f3(x: int) -> str: return str(x)
print(f3.__annotations__)   # {'x': <class 'int'>, 'return': <class 'str'>}
print(f3("not an int"))     # runs fine - no runtime checking
```

### Common pitfalls — mutable default arguments
```python
def bad(item, basket=[]):       # evaluated ONCE at def time
    basket.append(item); return basket
print(bad("a"), bad("b"))       # ['a'] then ['a','b']  <-- shared!

def good(item, basket=None):
    basket = [] if basket is None else basket
    basket.append(item); return basket
print(good("a"), good("b"))     # ['a'] then ['b']
```
**Probe:** *"Why does this happen?"* Defaults are evaluated once when the `def` executes and stored on the function object — inspect `bad.__defaults__`. Linters flag it (ruff `B006`).

### Built-in functional tools — map, filter, zip, enumerate, sorted
```python
print(list(map(str, [1, 2])))                  # comprehension usually reads better
print(list(filter(None, [0, 1, "", 2])))       # drops falsy -> [1, 2]
print(list(zip([1, 2, 3], "ab")))              # STOPS at shortest -> data loss
print(list(enumerate("ab", start=1)))          # [(1,'a'), (2,'b')]
print(sorted([("b", 2), ("a", 2)], key=lambda r: (-r[1], r[0])))
```
**Probe:** *"What's the `zip` bug?"* It truncates to the shortest input **silently** — a real ETL data-loss bug. Use `zip(..., strict=True)` (3.10+) when lengths must match.

### functools.partial
```python
from functools import partial
fns_bad  = [lambda: i for i in range(3)]
fns_good = [partial(lambda x: x, i) for i in range(3)]
print([f() for f in fns_bad])    # [2, 2, 2]  <- late binding
print([f() for f in fns_good])   # [0, 1, 2]
```
**Probe:** *"partial vs lambda?"* `partial` binds the **value** at creation and is introspectable and picklable; a lambda captures the **variable** and re-reads it when called — which is the late-binding bug.

---

## 4. Scope, Namespaces, and Closures

### Names, bindings, and namespaces
A namespace maps names to objects. Names are bound, not typed.

### Scope resolution — the LEGB rule
**L**ocal → **E**nclosing → **G**lobal (module) → **B**uilt-in; resolution stops at the first hit.
```python
x = "global"
def outer():
    x = "enclosing"
    def inner():
        return x            # found in Enclosing
    return inner()
print(outer())
```
**Probe:** *"Is a class body an enclosing scope for its methods?"* **No.** A method cannot see class-level names without `self.` or `ClassName.` — a common surprise.

### global and nonlocal
Both are needed only to **rebind**, never to read.
```python
count = 0
def bump():
    global count            # rebinds at module level
    count += 1

def counter():
    n = 0
    def inc():
        nonlocal n          # rebinds in the nearest ENCLOSING function
        n += 1
        return n
    return inc
c = counter()
print(c(), c(), c())        # 1 2 3
```
**Probe:** *"Why is heavy `global` use a smell?"* It is usually a missing parameter or a missing class, and it makes testing order-dependent.

### Closures — inner functions, captured variables
A closure keeps enclosing names alive after the outer function returns, via **cell** objects.
```python
def make_adder(n):
    def add(x): return x + n
    return add
add5 = make_adder(5)
print(add5(3))                            # 8
print(add5.__closure__[0].cell_contents)  # 5
print(add5.__code__.co_freevars)          # ('n',)
```

### Late binding behaviour in closures
Closures capture the **variable (cell)**, not the value at creation time.
```python
fns = [lambda: i for i in range(3)]
print([f() for f in fns])         # [2, 2, 2]  <-- not [0,1,2]

def make(i): return lambda: i
print([make(i)() for i in range(3)])            # [0,1,2] - factory, clearest
print([f() for f in [lambda i=i: i for i in range(3)]])  # works, abuses defaults
```

### Nested functions and function factories
```python
def power_of(exp):
    def apply(base): return base ** exp
    return apply
square = power_of(2)
print(square(7))
```
**Probe:** *"Closure or class for holding state?"* Closure for one operation and a little state; class when there are multiple methods, when state should be inspectable in a debugger or `repr`, or when it must be serialised. Closure state is hard to introspect — that is the real cost.

### Lifetime of variables and scope traps
```python
def trap():
    try:
        print(y)      # UnboundLocalError - y is LOCAL because assigned below
    except UnboundLocalError as e:
        print("UnboundLocalError:", e)
    y = 1
trap()
```
**Probe:** *"Why `UnboundLocalError` and not `NameError`?"* The compiler scans the whole function body: any assignment makes the name local for the **entire** function, including before the assignment line.

---

## 5. Comprehensions, Iterables, Iterators, and Generators

### Comprehensions — list, set, dict, generator expressions
```python
print([x*x for x in range(5) if x % 2])     # list   - eager, O(n) memory
print({x % 3 for x in range(5)})            # set    - deduplicates
print({k: len(k) for k in ["ab", "c"]})     # dict
print(sum(x*x for x in range(5)))           # generator - lazy, O(1) memory
```

### Nested comprehensions
Clause order reads like nested loops, but the expression comes first.
```python
matrix = [[r*c for c in range(3)] for r in range(3)]
print(matrix)
print([x for row in matrix for x in row])   # flatten - outer loop first
```
**Probe:** *"When is a comprehension the wrong choice?"* When it needs a statement (try/except, logging), when it spans more than a line or two, or when it is used purely for side effects — use a `for` loop.

### Scope behaviour
In Python 3 a comprehension has its own scope, so the loop variable does not leak.
```python
i = "safe"
_ = [i for i in range(3)]
print(i)                 # 'safe' - leaked in Python 2, not in 3
```

### Iterable vs iterator, and the iterator protocol
**Iterable** has `__iter__` returning a fresh iterator. **Iterator** has `__next__` and `__iter__` returning `self`; it is stateful and single-use, raising `StopIteration` when exhausted.
```python
xs = [1, 2, 3]
it = iter(xs)
print(next(it))          # 1
print(list(it))          # [2, 3] - consumed
print(list(it))          # []     - exhausted, silently
```
**Probe:** *"Why does an iterator's `__iter__` return `self`?"* So `for` works uniformly on both iterables and iterators without the caller knowing which it has.

### Creating custom iterators
```python
class Countdown:
    def __init__(self, n): self.n = n
    def __iter__(self):                 # generator -> FRESH iterator each call
        n = self.n
        while n > 0:
            yield n
            n -= 1
print(list(Countdown(3)), list(Countdown(3)))   # re-iterable
```
**Probe:** *"What breaks if `__iter__` returns `self` with position on the instance?"* It becomes single-use, and two nested loops over the same object interfere. The generator form avoids this entirely.

### Generators — functions, yield, state, lazy evaluation
```python
def squares(n):
    for i in range(n):
        yield i * i
g = squares(3)
print(type(g).__name__, list(g))    # 'generator' [0, 1, 4]
```
Calling a generator function runs **no code** — it returns a generator object. Work happens on each `next()`.
**Probe:** *"What do generators cost?"* Single-use; no `len()`, indexing, or slicing; errors surface at consumption time far from the cause; and the traceback points at the consumer, not the pipeline definition.

### yield from
Delegates iteration **and** forwards `send`/`throw`/`close`, propagating the subgenerator's return value.
```python
def chain2(*its):
    for it in its:
        yield from it
print(list(chain2([1, 2], [3])))
```

### Key differences

| | Iterable | Iterator | Generator |
|---|---|---|---|
| `__iter__` | yes | yes (returns self) | yes |
| `__next__` | no | yes | yes |
| Reusable | yes | no | no |
| Memory | holds data | holds position | holds frame |

```python
print(sum(x*x for x in range(1000)))     # genexp: O(1) memory
```

### Common iteration tools
```python
print(list(enumerate("ab", 1)), list(zip([1, 2], "ab")), list(reversed([1, 2, 3])))
print(sorted({"a": 2, "b": 1}.items(), key=lambda kv: kv[1]))
print(any([]), all([]))          # (False, True) <- all([]) is vacuously True
```
**Probe:** *"What do `any`/`all` return for an empty iterable?"* `False` and **`True`**. The `all([])` result surprises people and is a genuine validation bug on empty input. Both short-circuit — pass a generator, not a list comprehension, to keep that benefit.

---

## 6. Exceptions, Context Managers, and File Handling

### Errors vs exceptions and the hierarchy
`BaseException` → `Exception` → everything you should normally catch. `KeyboardInterrupt`, `SystemExit` and `GeneratorExit` sit **outside** `Exception` deliberately.
```python
print(issubclass(ValueError, Exception))            # True
print(issubclass(KeyboardInterrupt, Exception))     # False - it's control flow
```
**Probe:** *"Why is `except BaseException` almost always wrong?"* It swallows Ctrl-C and `sys.exit()`, so your process becomes unkillable and shutdown breaks.

### Exception handling — try, except, else, finally
```python
import json
raw = '{"a": 1}'
try:
    data = json.loads(raw)
except json.JSONDecodeError as e:
    print("bad payload:", e); raise
else:
    print("ok:", data)      # runs only if NO exception - keeps try{} minimal
finally:
    print("cleanup")        # ALWAYS runs
```
**Probe:** *"What if `finally` contains a `return`?"* It **discards** the pending exception and any pending return value — silent data loss. Python 3.14 emits a `SyntaxWarning` for it (PEP 765).

### Raising and re-raising
```python
class ConfigError(Exception): pass
try:
    try:
        int("x")
    except ValueError as e:
        raise ConfigError("bad port") from e    # sets __cause__, keeps the chain
except ConfigError as e:
    print(type(e).__name__, "caused by", type(e.__cause__).__name__)
```
**Probe:** *"What does `from` change?"* It marks a deliberate translation — "The above exception was the direct cause" — versus the automatic `__context__` ("During handling..."). Use `from None` to suppress noise.

### Custom exceptions
One base per domain, then specific subclasses, carrying structured data.
```python
class PaymentError(Exception):
    def __init__(self, msg, order_id=None):
        super().__init__(msg)
        self.order_id = order_id
class CardDeclined(PaymentError): pass
e = CardDeclined("declined", order_id=7)
print(isinstance(e, PaymentError), e.order_id)
```
**Probe:** *"Why a hierarchy?"* Callers catch at the granularity they need, and you can add new error types without breaking `except PaymentError`.

### Catching broadly
```python
cfg = {}
try:
    host = cfg["db"]["host"]
except Exception:
    host = "localhost"        # hides KeyError, TypeError, your own typos
print(host)
```
**Probe:** *"When is broad catching acceptable?"* Only at a genuine top-level boundary — request handler, worker loop, CLI entry point — and it must log the traceback.

### assert
```python
def withdraw(balance, amount):
    assert amount > 0          # REMOVED under python -O !
    return balance - amount
print(withdraw(100, 30))
```
**Probe:** *"What happens under `-O`?"* Every `assert` is stripped from the bytecode, so `withdraw(100, -500)` silently returns `600`. Use `if ... raise ValueError` for real validation; `assert` is only for impossible-by-design invariants.

### Context managers — with, resource management
Guarantees setup and teardown even on exception, early return, or break. Entering is left-to-right, exiting right-to-left.

### Creating context managers — class-based and function-based
```python
import time, contextlib

class Timer:
    def __enter__(self):
        self.t = time.perf_counter(); return self
    def __exit__(self, exc_type, exc, tb):
        self.elapsed = time.perf_counter() - self.t
        return False          # falsy -> exception PROPAGATES

@contextlib.contextmanager
def timer():
    t = time.perf_counter()
    try:
        yield
    finally:                  # MANDATORY - else cleanup is skipped on error
        print(f"took {time.perf_counter() - t:.4f}s")

with Timer() as tm:
    sum(range(1000))
with timer():
    sum(range(1000))
```
**Probe:** *"What does `__exit__`'s return value control?"* A **truthy** return **suppresses** the exception. Accidentally returning something truthy silently swallows errors — a nasty bug.

### File handling — modes, reading, writing, binary vs text
```python
# 'r' read, 'w' truncate+write, 'a' append, 'x' create-exclusive
# '+' read/write, 'b' binary, 't' text (default)
import tempfile, os
path = os.path.join(tempfile.mkdtemp(), "data.txt")
with open(path, "w", encoding="utf-8") as f:     # ALWAYS set encoding
    f.write("line1\nline2\n")
with open(path, encoding="utf-8") as f:
    for line in f:            # LAZY - streams, O(1) memory
        print(line.rstrip())
with open(path, "rb") as f:
    print(f.read(5))          # binary mode -> bytes
```
**Probe:** *"Why always pass `encoding=`?"* The default is platform-dependent, so the same code produces different results on Windows and Linux — a classic "works on my machine" bug.

### File object methods and paths
```python
from pathlib import Path
import tempfile, os
path = os.path.join(tempfile.mkdtemp(), "data.txt")
Path(path).write_text("line1\nline2\n", encoding="utf-8")

p = Path(path)
print(p.exists(), p.suffix, p.stem)
print(p.read_text(encoding="utf-8").count("line"))
print((Path("data") / "f.txt").as_posix())      # OS-independent joining
```**Probe:** *"`readlines()` vs iterating the file?"* `readlines()` loads everything into memory; iterating streams line by line. For a 10 GB log, only one works.

### Best practices in file operations
Use `with` (never bare `open`), set `encoding`, prefer EAFP over `os.path.exists` checks (which are racy), and catch the specific error.
```python
try:
    with open("missing.txt", encoding="utf-8") as f:
        data = f.read()
except FileNotFoundError:
    data = "DEFAULT"          # specific, not bare except
print(data)
```
**Probe:** *"Why is `if exists(): open()` worse?"* Time-of-check/time-of-use race — the file can vanish in the gap. EAFP performs the operation atomically from the caller's view.

---

## 7. OOP in Python

### OOP basics, defining classes, instances
```python
class Account:
    bank = "Acme"                       # class attribute - SHARED
    def __init__(self, owner, balance=0):
        self.owner = owner              # instance attributes - per object
        self.balance = balance
a = Account("Ada", 100)
print(a.owner, Account.bank, isinstance(a, Account))
```
**Probe:** *"How is Python OOP different from Java/C++?"* No true private (only `_` convention and `__` mangling), no interfaces (duck typing / ABCs / Protocols), no overloading by signature, everything is patchable, multiple inheritance via MRO, and **properties mean you never need upfront getters/setters**.

### Instance vs class attributes — the shared-mutable trap
```python
class Cart:
    items = []                          # SHARED across all instances!
    def add(self, i): self.items.append(i)
c1, c2 = Cart(), Cart()
c1.add("apple")
print(c2.items)                         # ['apple'] <-- bug

class CartFixed:
    def __init__(self): self.items = []     # per-instance
```
**Probe:** *"Why does `self.items.append(x)` leak but `self.items = [...]` not?"* `append` **mutates** the class attribute (no rebinding); assignment **creates** an instance attribute that shadows it.

### Instance, class, and static methods
```python
class Temp:
    def __init__(self, c): self.c = c
    def describe(self): return f"{self.c}C"          # instance
    @classmethod
    def from_f(cls, f): return cls((f - 32) * 5 / 9) # alt constructor
    @staticmethod
    def is_valid(c): return c >= -273.15             # no state
print(Temp.from_f(212).describe(), Temp.is_valid(-300))
```
**Probe:** *"When is `@classmethod` right?"* Alternative constructors — `cls(...)` means a **subclass** gets an instance of itself, which a hardcoded class name would break. `@staticmethod` is justified when it belongs to the class's API but needs no state; if it is pure namespacing, a module-level function is better and more testable.

### Constructors — __init__ vs __new__
```python
class Point:
    def __new__(cls, *a):          # CREATES the instance
        return super().__new__(cls)
    def __init__(self, x, y):      # INITIALISES it, must return None
        self.x, self.y = x, y
```
**Probe:** *"When override `__new__`?"* Subclassing immutables (`int`, `str`, `tuple`), singletons, instance caching. The classic bug: a cached singleton still runs `__init__` on **every** call.

### String representation — __str__ and __repr__
```python
class User:
    def __init__(self, uid, name): self.uid, self.name = uid, name
    def __repr__(self): return f"User(uid={self.uid!r}, name={self.name!r})"
    def __str__(self):  return self.name
u = User(1, "Ada")
print(repr(u), str(u), [u])     # lists/dicts/debuggers use __repr__
```
**Probe:** *"Which do you implement if only one?"* `__repr__` — `__str__` falls back to it, and `repr` is what appears in containers, logs, and debuggers. It must never raise.

### Encapsulation and attribute access
```python
class Acct:
    def __init__(self):
        self.public = 1
        self._internal = 2      # convention: "don't touch"
        self.__private = 3      # NAME MANGLED to _Acct__private
a = Acct()
print(a.public, a._internal, a._Acct__private)   # still reachable
```
**Probe:** *"Is `__private` really private?"* No — it is name-mangled to avoid **subclass collisions**, not to enforce access. Python relies on convention.

### Inheritance — single, multiple, multilevel; overriding; super()
```python
class A:
    def ping(self): return "A"
class B(A):
    def ping(self): return "B" + super().ping()
class C(A):
    def ping(self): return "C" + super().ping()
class D(B, C):
    def ping(self): return "D" + super().ping()
print(D().ping())                              # DBCA
print([k.__name__ for k in D.__mro__])         # D B C A object
```
**Probe:** *"What does `super()` actually do?"* **Not** "call my parent" — it calls the next class in **this instance's MRO**. `B.ping`'s `super()` resolves to `C`, even though `B` never heard of `C`. That is cooperative multiple inheritance, and it only works if every class calls `super()`.

### MRO and C3 linearisation
C3 guarantees a class precedes its parents, parent order is preserved, and the result is monotonic. If no consistent order exists, the `class` statement itself raises `TypeError`.
```python
class A: pass
class B(A): pass
try:
    class Bad(A, B): pass          # inconsistent - B is more derived than A
except TypeError as e:
    print("TypeError:", str(e)[:45])
```

### Polymorphism and duck typing
```python
class Dog:  
    def speak(self): return "woof"
class Cat:
    def speak(self): return "meow"
print([x.speak() for x in (Dog(), Cat())])    # no shared base needed
```

### Composition vs inheritance
Prefer composition when the relationship is "has-a", when you would inherit only to reuse code, or when you need to vary behaviour at runtime. Inheritance couples you to the parent's **implementation** — the fragile base class problem.
```python
class Engine:
    def start(self): return "vroom"
class Car:
    def __init__(self): self.engine = Engine()   # has-a
    def start(self): return self.engine.start()
print(Car().start())
```

### Properties — @property, setter, deleter
```python
class Temperature:
    def __init__(self, celsius): self.celsius = celsius   # goes THROUGH setter
    @property
    def celsius(self): return self._celsius
    @celsius.setter
    def celsius(self, v):
        if v < -273.15: raise ValueError("below absolute zero")
        self._celsius = v
    @property
    def fahrenheit(self): return self._celsius * 9 / 5 + 32
t = Temperature(25)
print(t.fahrenheit, t.__dict__)      # 77.0 {'_celsius': 25}
```
**Probe:** *"What problem does `@property` solve?"* It lets a plain attribute **become** computed later without changing any call site — which is why Python code does not write getters and setters upfront. Keep them cheap and side-effect-free; a property that hits the database is a production incident waiting to happen.

### Dunder methods — arithmetic, comparison, container, callable
```python
from functools import total_ordering
@total_ordering
class Money:
    def __init__(self, amt): self.amt = amt
    def __add__(self, o):  return Money(self.amt + o.amt)
    def __eq__(self, o):   return isinstance(o, Money) and self.amt == o.amt
    def __lt__(self, o):
        if not isinstance(o, Money): return NotImplemented
        return self.amt < o.amt
    def __hash__(self):    return hash(self.amt)
    def __repr__(self):    return f"Money({self.amt})"

class Bag:
    def __init__(self, xs): self.xs = list(xs)
    def __len__(self):      return len(self.xs)
    def __getitem__(self, i): return self.xs[i]
    def __contains__(self, v): return v in self.xs
    def __iter__(self):     return iter(self.xs)
    def __call__(self):     return "called"

print(Money(2) + Money(3), Money(1) < Money(2), sorted([Money(2), Money(1)]))
b = Bag([1, 2, 3])
print(len(b), b[0], 2 in b, list(b), b())
```
**Probe:** *"Why return `NotImplemented` instead of `False`?"* So Python tries the **reflected** operation on the other operand. Returning `False` claims a definitive answer and breaks symmetric comparison with types that could have handled it.

---

## 8. Decorators, Descriptors, Dataclasses, and Advanced Object Behaviour

### Decorators — function, with arguments, chaining, preserving metadata
`@d` means `f = d(f)`; `@d(arg)` means `f = d(arg)(f)`, which is why parameterised decorators need an extra layer. Stacked decorators apply **bottom-up**.
```python
import functools

def log_calls(fn):
    @functools.wraps(fn)                 # preserves __name__/__doc__/signature
    def wrapper(*args, **kwargs):
        return fn(*args, **kwargs)
    return wrapper

def retry(attempts=3, exceptions=(Exception,)):
    def decorator(fn):
        @functools.wraps(fn)
        def wrapper(*args, **kwargs):
            for attempt in range(attempts):
                try:
                    return fn(*args, **kwargs)
                except exceptions:
                    if attempt == attempts - 1:
                        raise
        return wrapper
    return decorator

@log_calls
@retry(attempts=2)
def fetch(x):
    "Fetch a thing."
    return x * 2

print(fetch(21), fetch.__name__, fetch.__doc__)
```
**Probe:** *"What breaks without `functools.wraps`?"* `__name__` becomes `'wrapper'`, `__doc__` becomes `None`, and `inspect.signature` reports `(*args, **kwargs)`. Downstream that breaks Sphinx docs, pytest fixtures, FastAPI/click signature introspection, pickling, and profiler output.

### Class decorators
Receive the already-built class and modify or replace it; unlike metaclasses they are **not inherited**.
```python
def register(cls):
    cls.registered = True
    return cls
@register
class Service: pass
print(Service.registered)
```

### Closures behind decorators
Every decorator is a closure over the wrapped function — which is why `fn` stays alive in `wrapper`.
```python
import functools
def log_calls(fn):
    @functools.wraps(fn)
    def wrapper(*a, **k): return fn(*a, **k)
    return wrapper

@log_calls
def fetch(x): return x * 2

print(fetch.__wrapped__.__name__)    # 'fetch' - reach the original via wraps
```

### Descriptors — data vs non-data, the protocol
A descriptor is any object defining `__get__`, `__set__`, or `__delete__`, placed as a **class attribute**.
```python
class Positive:
    def __set_name__(self, owner, name):      # 3.6+ learns its own name
        self.name = "_" + name
    def __get__(self, obj, objtype=None):
        if obj is None: return self
        return getattr(obj, self.name)
    def __set__(self, obj, value):            # __set__ => DATA descriptor
        if value <= 0: raise ValueError("must be positive")
        setattr(obj, self.name, value)

class Product:
    price = Positive()
    qty = Positive()                          # REUSE - the point of descriptors
    def __init__(self, price, qty): self.price, self.qty = price, qty

p = Product(10, 2)
print(p.price, p.qty)
try: p.price = -1
except ValueError as e: print("ValueError:", e)
```
**Probe:** *"Data vs non-data descriptor?"* A **data** descriptor (has `__set__`/`__delete__`) takes priority **over** the instance `__dict__`; a **non-data** descriptor (only `__get__`) loses to it. That priority rule *is* the attribute lookup order.

### How functions become bound methods
Functions are **non-data descriptors** — `function.__get__` returns a bound method.
```python
class K:
    def m(self): return self
k = K()
print(type(K.__dict__["m"]).__name__)    # function
print(type(k.m).__name__)                # method - produced by __get__
print(k.m.__self__ is k)                 # True
```
**Probe:** *"Why are functions descriptors?"* It is the single best test of whether someone understands the data model rather than having memorised it.

### Dataclasses — generated methods, defaults, default_factory, frozen, ordering
```python
from dataclasses import dataclass, field, FrozenInstanceError, replace

@dataclass(frozen=True, order=True)
class Point:
    x: int
    y: int = 0
    tags: list = field(default_factory=list, compare=False)

p1, p2 = Point(1, 2), Point(1, 2)
print(p1, p1 == p2, p1 < Point(2, 0))
print(len({p1, p2}))                 # 1 - frozen+eq => hashable
try: p1.x = 5
except FrozenInstanceError: print("frozen: cannot mutate")
print(replace(p1, x=9))              # the update idiom for frozen data
```
**Probe:** *"Why does a mutable default need `default_factory`?"* A bare `tags: list = []` would share one list across all instances. Dataclasses **detect** this and raise `ValueError` at class creation — one of the best guardrails in the stdlib.
**Probe:** *"How do `eq` and `frozen` affect `__hash__`?"* `eq=True, frozen=False` sets `__hash__ = None` (unhashable). `eq=True, frozen=True` **generates** a hash. `eq=False` keeps the inherited identity hash.

### __slots__
Replaces the per-instance `__dict__` with a fixed array, cutting memory significantly and slightly speeding attribute access.
```python
class Slim:
    __slots__ = ("x", "y")
    def __init__(self, x, y): self.x, self.y = x, y
s = Slim(1, 2)
try: s.z = 3
except AttributeError as e: print("AttributeError:", e)
print(hasattr(s, "__dict__"))        # False
```
**Probe:** *"What does `__slots__` cost?"* No `__dict__` (no new attributes), no `__weakref__` unless declared, fiddly inheritance (a subclass without `__slots__` reintroduces `__dict__` and erases the saving), awkward with default pickle, **incompatible with `cached_property`**, and less monkeypatchable for tests. Only after measuring — typically millions of small objects. `@dataclass(slots=True)` gives both ergonomics and the win.

### Object lifecycle — construction and destruction
```python
import weakref
class Res:
    def __del__(self): print("finalised")
r = Res()
wr = weakref.ref(r)
print(wr() is r)
del r                                 # refcount hits 0 -> __del__ runs
print(wr())                           # None
```
**Probe:** *"Why not use `__del__` for cleanup?"* It is **not guaranteed to run** — reference cycles, interpreter shutdown, and held tracebacks delay or skip it; exceptions inside it are swallowed. Use a **context manager**, with `weakref.finalize` as a safety net.

### Attribute lookup flow
For `obj.x`: (1) data descriptors on the type's MRO → (2) `obj.__dict__` → (3) non-data descriptors and class attributes → (4) `__getattr__` → (5) `AttributeError`. All orchestrated by `__getattribute__`.
```python
class Proxy:
    def __init__(self, target): self._t = target
    def __getattr__(self, name):          # ONLY called when normal lookup FAILS
        return getattr(self._t, name)
print(Proxy("hello").upper())
```
**Probe:** *"`__getattr__` vs `__getattribute__`?"* `__getattribute__` runs on **every** access and recurses infinitely unless you delegate to `super()`. `__getattr__` is the safe fallback, used for proxies and lazy loading.

### Customising object behaviour with dunder methods
Implement dunders to satisfy a protocol the language already defines — not to look clever. Each one is an implicit contract readers must learn.

---

## 9. Modules, Imports, Typing, Memory Model, Copy, Equality, Hashing

### Modules and the import system
On first import Python finds the module, compiles it, **inserts it into `sys.modules` before executing it**, then runs the body. Later imports return the cache.
```python
import sys, json as j                      # aliasing
from math import pi, sqrt                  # from-import
print("json" in sys.modules, j.__name__, round(pi, 3))
```
**Probe:** *"Why does that ordering matter?"* It is exactly why circular imports fail with a **partially initialised module** rather than infinite recursion — and why the error depends on which module is imported first.

### Absolute vs relative imports, packages, __init__.py
```python
# from myapp.models import User     # absolute - explicit, PEP 8 preferred
# from .models import User          # relative - breaks if run as a script
```
A **package** is a directory with `__init__.py`; without it you get a **namespace package** (PEP 420), which can span directories — and an accidentally missing `__init__.py` creates one silently.
**Probe:** *"Why avoid `from x import *`?"* It pollutes the namespace, can shadow builtins, hides where names came from, and breaks static analysis.

### __name__ == "__main__"
```python
def main(): print("running")
if __name__ == "__main__":
    main()        # does not fire on import - required for multiprocessing spawn
```

### Import-time side effects
Module bodies run on import, so connecting to a database or reading config at module level makes the module untestable and slows startup. **Module bodies should define, not do.**

### Typing basics
```python
from typing import Optional, Union, Any, Callable, TypeAlias, Protocol, TYPE_CHECKING
UserId: TypeAlias = int
def find(uid: UserId) -> Optional[str]: ...          # same as str | None
def apply(fn: Callable[[int], str], x: int) -> str: return fn(x)
def f(xs: list[int]) -> dict[str, int]: return {}    # built-in generics, PEP 585

class HasRead(Protocol):                              # structural typing
    def read(self) -> bytes: ...
print(apply(str, 5))
```
**Probe:** *"What is wrong with `def f(x: int = None)`?"* The annotation says `int` but the default is `None`; it should be `x: int | None = None`. Also: hints are **never enforced at runtime** — they live in `__annotations__` and need mypy/pyright in CI.
**Probe:** *"`list[int]` or `Sequence[int]` in a signature?"* Accept the most general (`Sequence` — also signals you will not mutate), return the most concrete (`list`).

### Memory model — references, identity, reference counting, GC
CPython frees an object the moment its refcount hits zero; a cyclic garbage collector exists **only** to collect reference cycles, which refcounting can never free.
```python
import gc
a = []; a.append(a)          # cycle - refcount never reaches 0
del a
print(gc.collect() >= 0)     # the cyclic collector reclaims it
```
**Probe:** *"Common causes of memory growth?"* Unbounded caches (`lru_cache(maxsize=None)`), module-level registries, lingering references, `lru_cache` on a **method** pinning `self`, stored tracebacks pinning whole frames, and accumulating threads/tasks.

### Copying objects — assignment vs shallow vs deep
```python
import copy
orig = {"a": [1, 2]}
alias   = orig                      # same object
shallow = copy.copy(orig)           # new dict, SAME inner list
deep    = copy.deepcopy(orig)       # fully independent
shallow["a"].append(3)
print(orig, deep)                   # orig changed; deep did not
```
**Probe:** *"When is `deepcopy` wrong?"* Large graphs (slow, doubles memory), objects holding sockets/locks/connections, when identity matters (copying a registry breaks the invariant), and when shared-by-design state gets silently forked. Often the better answer is "make it immutable" rather than copy.

### Equality vs identity
```python
a, b = [1, 2], [1, 2]
print(a == b, a is b)        # True False
print(a is not None)         # identity for singletons only
```

### Hashing — hash(), hashable types, equality/hash relation
**Contract: if `a == b` then `hash(a) == hash(b)`.** Defining `__eq__` sets `__hash__ = None`.
```python
class P:
    def __init__(self, x): self.x = x
    def __eq__(self, o): return isinstance(o, P) and self.x == o.x
try: {P(1)}
except TypeError as e: print("TypeError:", e)
```

### The mutable keys problem
```python
class Q:
    def __init__(self, x): self.x = x
    def __eq__(self, o): return isinstance(o, Q) and self.x == o.x
    def __hash__(self): return hash(self.x)
q = Q(1); s = {q}; q.x = 2
print(q in s, q in list(s))     # False True  <- wrong bucket, silent corruption
```
**Probe:** *"How do you get this right cheaply?"* `@dataclass(frozen=True)` generates a correct `__eq__`/`__hash__` **and** prevents the mutation. That is the answer for most cases.

### Interning and small integer caching
```python
a, b = 256, 256
c, d = 257, 257
print(a is b)        # True  - small ints -5..256 are cached
print(c is d)        # may be True in one compiled block, False across them
```
**Probe:** *"Should you rely on this?"* Never. It is a CPython implementation detail that differs between the REPL and a module and across implementations — and it is exactly why `is` is reserved for singletons.

---

## 10. Concurrency Basics, Debugging, and Performance

### Concurrency vs parallelism
**Concurrency** = multiple tasks in progress and interleaving; **parallelism** = multiple tasks executing simultaneously on different cores. Threads and asyncio give concurrency; multiprocessing gives parallelism.

### The GIL — what it is, impact on threads
A CPython mutex ensuring only one thread executes Python bytecode at a time, which makes reference counting safe without per-object locks.
```python
import threading
counter = 0
def bump():
    global counter
    for _ in range(100_000):
        counter += 1          # LOAD, ADD, STORE - NOT atomic
ts = [threading.Thread(target=bump) for _ in range(2)]
[t.start() for t in ts]; [t.join() for t in ts]
print(counter, "(expected 200000 - usually less)")
```
**Probe:** *"Does the GIL make my code thread-safe?"* **No** — the most dangerous misconception. It guarantees bytecode-level atomicity, not operation-level: `counter += 1` is three bytecodes and can be interrupted between them. You still need locks.
**What the GIL does NOT prevent:** concurrency during I/O (it is released), parallelism in C extensions (numpy, zlib, hashlib release it), race conditions, or multiprocessing parallelism.

### Threading basics — use cases and limitations
Useful for I/O-bound work and for using blocking libraries without a rewrite; useless for CPU-bound pure Python.
```python
from concurrent.futures import ThreadPoolExecutor
with ThreadPoolExecutor(max_workers=4) as ex:
    results = list(ex.map(str, range(5)))
print(results)
```
**Probe:** *"What happens to an exception inside a `Future`?"* It is **stored, not raised** — you only see it when you call `.result()`. Fire-and-forget submissions lose failures silently.

### Multiprocessing basics — use cases, process isolation
Each process has its own interpreter and GIL, giving true parallelism; the cost is startup, and **all arguments and results must be picklable** and are copied.
```python
# NOTE: save as a .py file and run it - spawn re-imports __main__,
# so this cannot run from a REPL or an exec'd string.
from concurrent.futures import ProcessPoolExecutor

def square(n): return n * n

if __name__ == "__main__":
    with ProcessPoolExecutor() as ex:
        print(list(ex.map(square, range(5))))     # [0, 1, 4, 9, 16]
```
**Probe:** *"What commonly is not picklable?"* Lambdas, locally-defined functions and classes, open file handles, locks, and generators.

### Asyncio basics — event loop, coroutines, async/await, tasks
Single-threaded cooperative concurrency: a task runs until it voluntarily yields at an `await`.
```python
import asyncio
async def work(n, delay):
    await asyncio.sleep(delay)
    return n
async def main():
    results = await asyncio.gather(work(1, 0.02), work(2, 0.01))
    return results            # INPUT order [1, 2], not completion order
print(asyncio.run(main()))
```
**Probe:** *"Coroutine vs task vs future?"* Calling `async def` returns a **coroutine** that has **not started**; `asyncio.create_task` wraps it in a **Task** scheduled on the loop so it runs concurrently; a **Future** is the lower-level result placeholder (`Task` subclasses it).
**Probe:** *"Why are blocking calls inside async code a problem?"* `time.sleep` or `requests.get` never yields, so the **entire loop** freezes — every other task, heartbeat, and timeout. Use an async library or `await asyncio.to_thread(fn)`. Detect with `loop.set_debug(True)`.

### Choosing threading vs multiprocessing vs asyncio

| Workload | Choice | Why |
|---|---|---|
| I/O-bound, blocking libraries | **Threads** | GIL released during I/O; no rewrite |
| I/O-bound, thousands of connections | **asyncio** | No per-connection stack |
| CPU-bound pure Python | **Multiprocessing** | Separate GILs = real parallelism |
| CPU-bound in numpy/C | **Threads work** | Those libraries release the GIL |

**Probe:** *"Existing sync codebase with an I/O bottleneck — threads or asyncio?"* **Threads**, almost always. Migrating to async is viral across the whole call stack for a benefit that only materialises at high concurrency.

### Debugging basics — tracebacks, pdb, logging
Read tracebacks **bottom-up**: the last frame is where it raised.
```python
import logging
logging.basicConfig(level=logging.INFO, format="%(levelname)s %(name)s %(message)s")
log = logging.getLogger(__name__)
log.info("processing %s items", 5)       # lazy %s formatting, not f-string
# breakpoint()    # 3.7+ entry point; respects PYTHONBREAKPOINT
```
**Probe:** *"What does 'During handling of the above exception' mean?"* An exception was raised **inside** an `except` block — the first one is usually the real cause. "The above exception was the direct cause" instead means an explicit `raise ... from`.
**Probe:** *"Library vs application logging?"* A library uses `getLogger(__name__)` and adds **no** handlers and no `basicConfig` — calling `basicConfig` in library code hijacks the host application's global configuration.

### Performance basics — complexity, profiling, efficient iteration, data structures
```python
import timeit
big_list = list(range(10_000)); big_set = set(big_list)
t_list = timeit.timeit(lambda: 9999 in big_list, number=1000)
t_set  = timeit.timeit(lambda: 9999 in big_set,  number=1000)
print(f"list {t_list:.4f}s vs set {t_set:.6f}s")    # O(n) vs O(1)
```
Tools: **`cProfile`** (deterministic, per-function, high overhead), **sampling profilers** like `py-spy` (low overhead, attaches to a running process), **`timeit`** (microbenchmarks), **`tracemalloc`** (allocation by line).

| Operation | list | deque | set/dict |
|---|---|---|---|
| index | O(1) | O(n) | - |
| append right | O(1)* | O(1) | - |
| **pop/insert left** | **O(n)** | **O(1)** | - |
| membership `in` | O(n) | O(n) | **O(1)** |

\* amortised

### Common optimisation principles
Measure before optimising; prefer algorithmic improvement over micro-optimisation; choose the right data structure first.
```python
# quadratic traps to recognise instantly:
# x in some_list inside a loop   -> use a set
# s += char inside a loop        -> "".join(parts)
# list.pop(0) / insert(0, x)     -> collections.deque
# arr[i:] inside recursion       -> pass an index
```
**Probe:** *"A function got slower after a refactor — what do you do?"* Quantify it, reproduce it deterministically, `git bisect` the change, **profile rather than guess**, fix, then **re-measure** to confirm the hypothesis.

---

## Final Interview Checklist

- **Say the mental model, not the memorised fact** — "names bound to objects" explains aliasing, late binding, and call-by-sharing all at once
- **State complexity and edge cases unprompted** — at senior level, having to be asked is itself a finding
- **Name the trade-off and the condition that would flip it**
- **Know the Python-specific traps:** mutable defaults, late binding, `[[0]*n]*m` aliasing, shared class attributes, `s += x` in loops, `list.pop(0)`, silent `zip` truncation, `defaultdict` read-inserts, `assert` under `-O`, `all([]) is True`, `__eq__` nulling `__hash__`, `finally` swallowing exceptions
- **"I don't know, but here's how I'd find out"** beats a confident wrong answer
