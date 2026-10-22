# Python Technical and Practical Notes — Concise Interview-Friendly Guide

This file is a **concise technical-notes document** for Python.

It is designed to cover:

- core syntax
- variables and data types
- control flow
- data structures
- functions
- common Python patterns
- debugging tips
- interview-friendly explanations

---

# 1) What Python is

Python is a **high-level, interpreted, general-purpose programming language** known for readable syntax and broad use in:
- scripting
- backend development
- automation
- data analysis
- testing
- machine learning
- DevOps tooling

A useful mental model:

- Python emphasizes readability
- indentation is syntactically important
- code is usually written in a concise, expressive style
- batteries-included standard library is a major strength

---

# 2) Core syntax basics

## 2.1 Variables

Python variables are created by assignment.

```python
x = 10
name = "Alice"
pi = 3.14
is_ready = True
```

Python is dynamically typed, which means you do not declare variable types explicitly in normal assignment.

---

## 2.2 Comments

Single-line comment:

```python
# this is a comment
```

Docstring-style string often used for documentation:

```python
def add(a, b):
    """Return the sum of a and b."""
    return a + b
```

---

## 2.3 Basic input and output

```python
print("Hello, world")
name = input("Enter your name: ")
print(name)
```

---

## 2.4 Indentation

Indentation defines code blocks.

```python
if x > 0:
    print("positive")
```

This is one of the most important Python syntax rules.

---

## 2.5 Basic operators

Arithmetic:
```python
+  -  *  /  //  %  **
```

Comparison:
```python
==  !=  >  <  >=  <=
```

Logical:
```python
and  or  not
```

Assignment:
```python
=  +=  -=  *=  /=
```

Membership:
```python
in  not in
```

Identity:
```python
is  is not
```

---

# 3) Data types

Common built-in types:

- `int`
- `float`
- `str`
- `bool`
- `list`
- `tuple`
- `set`
- `dict`
- `NoneType`

Examples:

```python
age = 25
price = 19.99
title = "Engineer"
active = True
items = [1, 2, 3]
point = (10, 20)
tags = {"python", "api"}
user = {"name": "Alice", "role": "admin"}
value = None
```

---

## 3.1 Type conversion

```python
int("10")
float("3.14")
str(123)
list("abc")
tuple([1, 2, 3])
set([1, 1, 2, 3])
```

---

## 3.2 Truthy and falsy values

Common falsy values:
- `False`
- `None`
- `0`
- `0.0`
- `""`
- `[]`
- `{}`
- `set()`

Everything else is generally truthy.

Example:

```python
if not items:
    print("empty")
```

---

# 4) Control flow

## 4.1 if / elif / else

```python
x = 5

if x > 10:
    print("big")
elif x > 0:
    print("positive")
else:
    print("zero or negative")
```

---

## 4.2 for loop

```python
for item in [1, 2, 3]:
    print(item)
```

Loop with `range`:

```python
for i in range(5):
    print(i)
```

---

## 4.3 while loop

```python
count = 0

while count < 3:
    print(count)
    count += 1
```

---

## 4.4 break, continue, pass

```python
for i in range(10):
    if i == 5:
        break
```

```python
for i in range(5):
    if i == 2:
        continue
    print(i)
```

```python
def todo():
    pass
```

---

# 5) Strings

## 5.1 Common string operations

```python
text = "python"

print(text.upper())
print(text.lower())
print(text.strip())
print(text.replace("py", "my"))
print(text.split("t"))
```

---

## 5.2 String formatting

f-string:

```python
name = "Alice"
age = 30
print(f"{name} is {age} years old")
```

`format()` style:

```python
print("{} is {}".format(name, age))
```

f-strings are usually preferred for readability.

---

# 6) Lists

A list is ordered and mutable.

```python
nums = [1, 2, 3]
nums.append(4)
nums.remove(2)
print(nums[0])
print(nums[-1])
```

Common operations:
- append
- insert
- remove
- pop
- sort
- reverse
- slicing

Examples:

```python
nums = [5, 1, 3]
nums.sort()
print(nums)

print(nums[0:2])
print(nums[::-1])
```

---

## 6.1 List comprehension

```python
squares = [x * x for x in range(5)]
```

Conditional example:

```python
evens = [x for x in range(10) if x % 2 == 0]
```

This is a common interview topic.

---

# 7) Tuples

A tuple is ordered and immutable.

```python
point = (10, 20)
x, y = point
```

Useful when data should not be changed.

---

# 8) Sets

A set stores unique elements.

```python
nums = {1, 2, 3}
nums.add(4)
nums.add(2)
print(nums)
```

Common use cases:
- remove duplicates
- membership checks
- set operations

Examples:

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)   # union
print(a & b)   # intersection
print(a - b)   # difference
```

---

# 9) Dictionaries

A dictionary stores key-value pairs.

```python
user = {"name": "Alice", "age": 30}
print(user["name"])
print(user.get("role"))
```

Add or update:

```python
user["role"] = "admin"
```

Iterate:

```python
for key, value in user.items():
    print(key, value)
```

Common methods:
- `get`
- `keys`
- `values`
- `items`
- `pop`
- `update`

---

# 10) Functions

## 10.0 Why functions matter

Functions are central in Python because they:
- encapsulate logic
- improve reuse
- make testing easier
- reduce duplication
- make code easier to reason about

Interviewers often look for whether you can break problems into small, clear functions.

---


## 10.1 Basic function

```python
def greet(name):
    return f"Hello, {name}"
```

---

## 10.2 Default arguments

```python
def greet(name="world"):
    return f"Hello, {name}"
```

---

## 10.3 Keyword arguments

```python
def power(base, exp):
    return base ** exp

print(power(exp=3, base=2))
```

---

## 10.4 Variable-length arguments

```python
def add_all(*args):
    return sum(args)
```

```python
def print_info(**kwargs):
    print(kwargs)
```

---

## 10.5 Scope

```python
x = 10

def show():
    y = 5
    print(x, y)
```

Important idea:
- local scope
- global scope

Be careful with modifying global variables.

---

## 10.6 Lambda

## 10.7 Mutable default argument pitfall

This is one of the most common Python interview pitfalls.

Bad:

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

The default list is created once and reused across calls.

Better:

```python
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

---

## 10.8 `try` / `except` / `else` / `finally`

```python
try:
    value = int("10")
except ValueError:
    print("bad input")
else:
    print("conversion worked")
finally:
    print("always runs")
```

This is useful in interviews because it shows understanding of normal path vs error path cleanup.

---


```python
square = lambda x: x * x
print(square(4))
```

Good for short expressions, but normal `def` is usually clearer for non-trivial logic.

---

# 11) Useful built-in functions

## 11.1 Very common interview helpers

`enumerate()` is useful when you need both index and value.

```python
for i, item in enumerate(["a", "b", "c"]):
    print(i, item)
```

`zip()` is useful when iterating over multiple collections together.

```python
for a, b in zip([1, 2], ["x", "y"]):
    print(a, b)
```

These are common “Pythonic” interview patterns.

---


Common built-ins:

- `len()`
- `type()`
- `range()`
- `sum()`
- `min()`
- `max()`
- `sorted()`
- `enumerate()`
- `zip()`
- `map()`
- `filter()`

Examples:

```python
items = ["a", "b", "c"]

for i, value in enumerate(items):
    print(i, value)
```

```python
a = [1, 2]
b = ["x", "y"]
print(list(zip(a, b)))
```

---

# 12) Exception handling

## 12.1 Common exception types to know

High-value exceptions:
- `TypeError`
- `ValueError`
- `KeyError`
- `IndexError`
- `AttributeError`
- `ZeroDivisionError`

A good Python engineer should quickly recognize these from tracebacks.

---


Use exceptions to handle runtime errors.

```python
try:
    x = 10 / 0
except ZeroDivisionError:
    print("cannot divide by zero")
```

Multiple exceptions:

```python
try:
    value = int("abc")
except ValueError:
    print("bad integer")
```

Optional `finally`:

```python
try:
    file = open("data.txt")
finally:
    print("cleanup runs")
```

---

# 13) Common practical patterns

## 13.0 Modules and imports

Python code is commonly organized into modules.

Examples:

```python
import math
print(math.sqrt(16))
```

```python
from math import sqrt
print(sqrt(16))
```

Important idea:
- `import module` keeps the namespace clearer
- `from module import name` is shorter but can be overused

---

## 13.0.1 File handling basics

```python
with open("data.txt", "r") as f:
    content = f.read()
```

The `with` statement is important because it handles cleanup automatically.

---


## 13.1 Iterate over list safely

```python
for item in items:
    print(item)
```

---

## 13.2 Check dictionary key safely

Preferred:

```python
value = user.get("email")
```

Instead of blindly doing:

```python
value = user["email"]
```

if the key may not exist.

---

## 13.3 Swap values

```python
a, b = b, a
```

---

## 13.4 Multiple assignment

```python
x, y, z = 1, 2, 3
```

---

## 13.5 Membership checks

## 13.6 Dictionary and set comprehensions

Dictionary comprehension:

```python
squares = {x: x * x for x in range(5)}
```

Set comprehension:

```python
evens = {x for x in range(10) if x % 2 == 0}
```

These are common follow-ups after list comprehensions.

---

## 13.7 Shallow copy vs deep copy thinking

A practical Python idea:
- assignment copies the reference
- shallow copy copies the outer container
- nested objects may still be shared

Example shallow copy:

```python
a = [[1], [2]]
b = a.copy()
b[0].append(99)
print(a)
```

This still affects nested data inside `a`.

This is a frequent debugging and interview topic.

---


```python
if "admin" in roles:
    print("allowed")
```

Set membership is usually faster than list membership for large collections.

---

# 14) Common debugging tips

## 14.1 Read the traceback carefully

The traceback often tells you:
- what file failed
- which line failed
- what exception occurred
- which call path led there

This is the first debugging skill.

---

## 14.2 Print or inspect variables

Simple debugging:

```python
print(value)
print(type(value))
```

This is basic but effective.

---

## 14.3 Check types explicitly

A common Python bug is using the wrong type.

```python
print(type(data))
```

Examples of common problems:
- string instead of integer
- list instead of dictionary
- `None` where a value was expected

---

## 14.4 Watch for mutable side effects

Lists and dictionaries are mutable.

Example:

```python
a = [1, 2]
b = a
b.append(3)
print(a)
```

This changes `a` too because both variables reference the same object.

Use a copy when needed:

```python
b = a.copy()
```

---

## 14.5 Watch indentation carefully

Incorrect indentation can:
- break logic
- raise syntax errors
- make loops and conditionals behave incorrectly

---

## 14.6 Check off-by-one errors

Very common in loops and indexing.

Example:

```python
nums = [10, 20, 30]
print(nums[2])   # valid
```

```python
print(nums[3])   # IndexError
```

---

## 14.7 Handle missing keys and None safely

Examples:

```python
user.get("email")
```

```python
if value is None:
    print("missing")
```

---

## 14.8 Reproduce with a small example

## 14.9 Check names, scope, and shadowing

Common mistakes:
- reusing built-in names like `list`, `dict`, or `str`
- using a variable before assignment
- shadowing outer-scope values unintentionally

Example of a bad idea:

```python
list = [1, 2, 3]
```

Now `list()` no longer refers to the built-in constructor in that scope.

---

## 14.10 Watch algorithm cost on large inputs

Even in Python basics interviews, a simple performance mindset helps.

Examples:
- membership in a list is usually slower than membership in a set for large collections
- repeated string concatenation in loops can be inefficient
- nested loops may create avoidable quadratic behavior

You do not need advanced algorithms for this note, but a basic complexity mindset is valuable.

---


When debugging:
1. reduce the code
2. isolate the failing input
3. test a minimal case
4. confirm expected vs actual behavior

This is one of the best practical debugging habits.

---

# 14.11 Classes and objects basics

Python supports object-oriented programming.

Basic example:

```python
class User:
    def __init__(self, name):
        self.name = name

    def greet(self):
        return f"Hello, {self.name}"
```

Important ideas:
- class defines behavior and structure
- object is an instance of a class
- `self` refers to the current instance
- `__init__` initializes object state

For many interviews, basic class understanding is enough even when the role is not deeply OOP-heavy.

---

# 15) Common interview-ready differences

## List vs tuple

A strong answer:

> A list is mutable and can be changed after creation, while a tuple is immutable. Lists are used for collections that may change, while tuples are useful for fixed data and can also serve as lightweight structured values.

---

## List vs set

A strong answer:

> A list preserves order and allows duplicates, while a set stores unique elements and is useful for fast membership checks and set operations like union and intersection.

---

## `==` vs `is`

A strong answer:

> `==` checks value equality, while `is` checks object identity. Use `==` when comparing values, and use `is` mainly for identity checks such as `value is None`.

---

## Function arguments: `*args` vs `**kwargs`

A strong answer:

> `*args` collects extra positional arguments into a tuple, while `**kwargs` collects extra keyword arguments into a dictionary.

---

## Mutable vs immutable types

## Shallow copy vs deep copy

A strong answer:

> Shallow copy copies the outer container but may still share nested mutable objects. Deep copy recursively copies nested objects as well. This matters when debugging unexpected shared-state changes.

---

## Why is `with open(...)` preferred for files?

A strong answer:

> `with open(...)` is preferred because it ensures the file is closed automatically, even if an exception happens. It is the safer and more Pythonic way to manage file resources.

---

## Why is `items=None` better than `items=[]` as a default?

A strong answer:

> Because default argument values are evaluated once when the function is defined, not each time it is called. Using `None` avoids accidentally sharing the same mutable object across calls.

---


A strong answer:

> Mutable objects can be changed after creation, like lists and dictionaries. Immutable objects cannot be changed after creation, like strings, tuples, and integers.

---

# 16) Practical interview examples

## 16.0 Common dictionary counting pattern

```python
counts = {}
for word in ["a", "b", "a"]:
    counts[word] = counts.get(word, 0) + 1
print(counts)
```

This is one of the most common Python interview patterns.

---


## Reverse a string

```python
s = "python"
print(s[::-1])
```

---

## Count characters with a dictionary

```python
text = "hello"
count = {}

for ch in text:
    count[ch] = count.get(ch, 0) + 1

print(count)
```

---

## Remove duplicates from a list

```python
nums = [1, 2, 2, 3, 4, 4]
unique = list(set(nums))
print(unique)
```

If order matters, interviewers may want a more careful answer.

---

## Find even numbers

```python
nums = [1, 2, 3, 4, 5, 6]
evens = [x for x in nums if x % 2 == 0]
print(evens)
```

---

# 17) Final summary

The most important way to understand Python is this:

- Python uses clean, readable syntax
- indentation defines blocks
- lists, tuples, sets, and dictionaries are core data structures
- functions are central to reusable logic
- modules, files, and classes are important practical building blocks
- exceptions handle runtime errors
- debugging often starts with tracebacks, types, scope checks, and minimal reproduction
- Python interviews often test common pitfalls such as mutable default arguments and shared mutable state
- interview success depends on clear understanding of fundamentals more than memorizing obscure details

If you understand:
- variables and syntax
- data types
- control flow
- strings and collections
- functions and arguments
- exceptions
- modules, files, and basic classes
- common debugging patterns
- pitfalls like mutable defaults and shallow copies
- core differences like list vs tuple and `==` vs `is`

then you understand the practical technical core of Python at a strong interview-ready level.
