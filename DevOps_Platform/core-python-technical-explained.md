# Core Python: Technical Topics and Subtopics

## 1. Python Execution Model
The Python execution model defines how source text is compiled, loaded, and evaluated inside an interpreter process.
It covers startup behavior, module initialization, bytecode caching, runtime state, and the rules that determine program entry.

### 1.1 Source Code Execution
Source code execution starts from a Python source file whose source file structure may contain statements, definitions, imports, and optional executable top-level code; the same code can be run in interactive mode, script mode, or by module execution with `python -m`.
Before execution, CPython normally compiles the source to bytecode and may store cache files inside `__pycache__`, allowing later imports to reuse compiled artifacts when valid.
- Source file structure
- Interactive mode
- Script mode
- Module execution
- Bytecode generation
- `__pycache__`

**Concrete example:**
```python
# hello.py
print("running top-level code")

# Terminal examples:
# python hello.py      -> script mode
# python -m hello      -> module execution
# python               -> interactive mode
#
# Importing hello from another file may create:
# __pycache__/hello.cpython-XY.pyc
```

### 1.2 Runtime Model
The runtime model is the active interpreter process that performs statement execution in sequence, applies the evaluation model to compute expression values, and manages frames, namespaces, and exceptions while the program runs.
Its object model overview means every runtime value is an object with identity, type, and state, so even integers, functions, and classes participate in uniform object-based execution semantics.
- Interpreter process
- Evaluation model
- Statement execution
- Expression evaluation
- Object model overview

**Concrete example:**
```python
x = 10
y = x * 2 + 1      # expression evaluation
print(y)           # statement execution

# At runtime, x and y are names bound to int objects.
print(type(y), id(y))
```

### 1.3 Program Entry
Program entry is controlled by the special variable `__name__`, which is set to `__main__` for the initial top-level module and to the real module name for imported modules.
The guard `if __name__ == "__main__":` isolates executable startup logic so library definitions remain import-safe while still allowing direct execution.
- `__name__`
- `__main__`
- `if __name__ == "__main__":`

**Concrete example:**
```python
def main():
    print("Program started")

if __name__ == "__main__":
    main()
```

---

## 2. Lexical Structure and Syntax
Lexical structure and syntax define how Python source code is broken into tokens and assembled into valid grammatical constructs.
This topic covers the parser-visible rules for identifiers, literals, indentation, statements, comments, and line continuation.

### 2.1 Tokens
Tokens are the smallest meaningful lexical units in Python, including keywords that have reserved syntactic meaning, identifiers that name objects, literals that denote fixed values, operators that express computation, and delimiters that separate or group syntax.
The tokenizer converts source text into these token categories before parsing, so legal program structure depends first on correct token formation.
- Keywords
- Identifiers
- Literals
- Operators
- Delimiters

**Concrete example:**
```python
value = 3 + 4
# value   -> identifier
# =, +    -> operators used in syntax
# 3, 4    -> numeric literals
# if, for -> keywords
# (), [], :, , -> common delimiters
```

### 2.2 Indentation and Blocks
Python uses indentation rules instead of braces to delimit suites of statements, so a block formation is created by a consistent increase in leading whitespace after compound-statement headers such as `if`, `for`, `while`, `def`, and `class`.
Nested blocks arise when one indented suite contains another compound statement, and inconsistent indentation produces a syntax-level indentation error.
- Indentation rules
- Block formation
- Nested blocks

**Concrete example:**
```python
if True:
    print("outer block")
    for i in range(2):
        print("nested block", i)
```

### 2.3 Statements
Python statements are executable syntactic units; simple statements fit on one logical line, while compound statements introduce headers followed by indented suites.
Assignment statements bind or rebind targets to values, whereas expression statements evaluate expressions primarily for their side effects, such as a function call.
- Simple statements
- Compound statements
- Assignment statements
- Expression statements

**Concrete example:**
```python
x = 5            # assignment statement
print(x)         # expression statement

if x > 0:        # compound statement
    x -= 1       # simple statement inside the block
```

### 2.4 Comments and Documentation
Single-line comments start with `#` and are ignored by the parser, while docstrings are string literals placed as the first statement in a module, class, or function body and become attached documentation.
Inline documentation refers to explanatory comments or adjacent clarifying notes placed near code to explain intent, constraints, or non-obvious behavior without changing execution.
- Single-line comments
- Docstrings
- Inline documentation

**Concrete example:**
```python
# Convert radius to area using the circle formula.
def area(radius):
    """Return the area of a circle for the supplied radius."""
    return 3.14159 * radius ** 2  # inline note: approximate pi
```

### 2.5 Line Structure
A physical line is one line of source text as stored in the file, while a logical line is the complete statement seen by the parser after continuation rules are applied.
Implicit line continuation occurs inside parentheses, brackets, or braces, whereas explicit line continuation uses a backslash at the end of a physical line.
- Logical lines
- Physical lines
- Implicit line continuation
- Explicit line continuation

**Concrete example:**
```python
total = (1 +
         2 +
         3)   # one logical line over three physical lines

value = 10 +         20    # explicit continuation
```

---

## 3. Variables, Names, and References
Python variables are names that refer to objects rather than typed storage cells containing values directly.
This topic covers binding rules, identifier validity, shared references, and deletion semantics for names and container elements.

### 3.1 Name Binding
Assignment binds a name to an object, rebinding changes an existing name to refer to a different object, and multiple assignment evaluates the right-hand side before assigning to each target.
Sequence unpacking destructures iterable values into multiple targets, and extended unpacking uses a starred target to capture a variable-length remainder.
- Assignment
- Rebinding
- Multiple assignment
- Sequence unpacking
- Extended unpacking

**Concrete example:**
```python
a = 10                  # assignment
a = 20                  # rebinding
x, y = 1, 2             # multiple assignment
first, second = [3, 4]  # sequence unpacking
head, *mid, tail = [1, 2, 3, 4]  # extended unpacking

print(a, x, y, first, second, head, mid, tail)
```

### 3.2 Identifiers
Identifiers are names formed under Python naming rules: they must start with a letter or underscore, may contain digits after the first character, and cannot contain spaces or punctuation such as `-`.
They are case sensitive, so `name` and `Name` are distinct, and reserved keywords such as `for`, `class`, and `if` cannot be used as identifiers.
- Naming rules
- Case sensitivity
- Reserved keywords

**Concrete example:**
```python
user_name = "Ava"
User_Name = "Noah"   # different identifier because of case sensitivity
# class = 1          # invalid: reserved keyword
print(user_name, User_Name)
```

### 3.3 References
In Python, names are bound to objects, so assigning one name to another usually copies a reference rather than duplicating the underlying object.
This reference semantics allows aliasing, where multiple names point to the same mutable object and observe the same later mutations.
- Names bound to objects
- Reference semantics
- Aliasing

**Concrete example:**
```python
a = [1, 2]
b = a          # aliasing: both names reference the same list
b.append(3)
print(a)       # [1, 2, 3]
print(a is b)  # True
```

### 3.4 Deletion
The `del` statement removes a binding, attribute, slice, or indexed element, depending on the target form used.
Unbinding names deletes the name from its namespace, while deleting container elements removes data from the container object rather than destroying the entire container.
- `del`
- Unbinding names
- Deleting container elements

**Concrete example:**
```python
items = ["a", "b", "c"]
x = 99

del x            # unbind name
del items[1]     # remove container element

print(items)     # ['a', 'c']
```

---

## 4. Data Types and Built-in Objects
Python includes a rich set of built-in object types with well-defined semantics and standard operations.
This topic covers primitive and composite types, binary data types, runtime inspection, and conversion between representations.

### 4.1 Numeric Types
Python numeric types include `int` for arbitrary-precision integers, `float` for double-precision floating-point values, and `complex` for numbers with real and imaginary parts.
Numeric literals are source-level notations such as decimal, binary, hexadecimal, floating-point, and imaginary forms that create numeric objects directly.
- `int`
- `float`
- `complex`
- Numeric literals

**Concrete example:**
```python
count = 42        # int
price = 19.95     # float
z = 2 + 3j        # complex
mask = 0b1010     # numeric literal
print(count, price, z, mask)
```

### 4.2 Boolean Type
The `bool` type has exactly two singleton values, `True` and `False`, and is used to model logical truth values in conditions and boolean operations.
Truth testing converts objects to a boolean context using defined rules, where values like `0`, `None`, empty containers, and `False` are falsey and many other objects are truthy.
- `bool`
- Truth values
- Truth testing

**Concrete example:**
```python
print(bool(0))        # False
print(bool("text"))   # True
print(5 > 3)          # True from comparison
```

### 4.3 None Type
`None` is the sole instance of the `NoneType` and represents the intentional absence of a value, result, or object reference.
Its null-like semantics make it useful as a default sentinel, a missing-result marker, or the implicit return value of functions that do not return explicitly.
- `None`
- Null-like semantics

**Concrete example:**
```python
result = None

if result is None:
    print("No value has been produced yet.")
```

### 4.4 Sequence Types
Sequence types are ordered collections that support positional access, length queries, and iteration; Python’s core built-in sequence types here are `str`, `list`, `tuple`, and `range`.
`str` stores immutable Unicode text, `list` stores mutable ordered elements, `tuple` stores immutable ordered elements, and `range` models an arithmetic progression lazily.
- `str`
- `list`
- `tuple`
- `range`

**Concrete example:**
```python
text = "python"
items = [10, 20, 30]
point = (4, 5)
numbers = range(3)

print(text[0], items[1], point[:], list(numbers))
```

### 4.5 Set Types
A `set` is a mutable unordered collection of unique hashable elements, while a `frozenset` is its immutable counterpart.
Both support efficient membership testing and set algebra, but only `frozenset` is hashable and therefore usable as a dictionary key or set element.
- `set`
- `frozenset`

**Concrete example:**
```python
s = {1, 2, 2, 3}
fs = frozenset([1, 2, 3])
print(s, fs)
```

### 4.6 Mapping Type
The built-in mapping type `dict` stores key-value associations and uses hash-based lookup for fast average-case retrieval and updates.
Keys must be hashable, while values may be of any type, making dictionaries the standard structure for labeled and structured data.
- `dict`

**Concrete example:**
```python
student = {"name": "Lina", "score": 95}
student["score"] += 1
print(student)
```

### 4.7 Binary Types
`bytes` is an immutable sequence of integers in the range 0–255, `bytearray` is the mutable variant, and `memoryview` exposes zero-copy access to an existing binary buffer.
These types are used for raw binary I/O, protocol data, encoded text, and high-performance slicing or mutation workflows.
- `bytes`
- `bytearray`
- `memoryview`

**Concrete example:**
```python
raw = b"ABC"
buffer = bytearray(b"ABC")
view = memoryview(raw)

print(raw, buffer[0], view[1])
```

### 4.8 Type Introspection
`type()` returns the runtime class of an object, `isinstance()` checks whether an object conforms to a class or tuple of classes, and `issubclass()` checks inheritance relationships between classes.
These APIs support reflection, validation, and behavior branching based on runtime type information.
- `type()`
- `isinstance()`
- `issubclass()`

**Concrete example:**
```python
print(type(3.14))
print(isinstance(True, bool))
print(issubclass(bool, int))
```

### 4.9 Type Conversion
Implicit conversion occurs automatically in limited numeric contexts such as mixing `int` and `float`, while explicit conversion requires the programmer to request a new representation.
Constructor-based casting uses type constructors such as `int()`, `float()`, `str()`, `list()`, or `tuple()` to produce converted objects when possible.
- Implicit conversion
- Explicit conversion
- Constructor-based casting

**Concrete example:**
```python
print(2 + 3.5)        # implicit int -> float conversion
print(int("12"))      # explicit conversion
print(list("abc"))    # constructor-based casting
```

## 5. Operators and Expressions
Operators and expressions define how Python computes values, updates objects or bindings, and combines operands according to precedence and associativity rules.
It includes arithmetic, comparison, assignment, logical, bitwise, membership, identity, and conditional evaluation forms.

### 5.1 Arithmetic Operators
Arithmetic Operators defines the semantics of `+`, `-`, `*`, `/`, `//`, `%`, and `**`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `+`
- `-`
- `*`
- `/`
- `//`
- `%`
- `**`

**Concrete example:**
```python
print(8 + 2, 8 - 2, 8 * 2, 8 / 2, 9 // 2, 9 % 2, 2 ** 5)
```

### 5.2 Comparison Operators
Comparison Operators defines the semantics of `==`, `!=`, `<`, `>`, `<=`, and `>=`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `==`
- `!=`
- `<`
- `>`
- `<=`
- `>=`

**Concrete example:**
```python
print(3 == 3, 3 != 4, 2 < 5, 7 > 1, 4 <= 4, 9 >= 8)
```

### 5.3 Assignment Operators
Assignment Operators defines the semantics of `=`, `+=`, `-=`, `*=`, `/=`, `//=`, `%=`, and `**=`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `=`
- `+=`
- `-=`
- `*=`
- `/=`
- `//=`
- `%=` 
- `**=`

**Concrete example:**
```python
x = 10
x += 5
x *= 2
x //= 3
x **= 2
print(x)
```

### 5.4 Logical Operators
Logical Operators defines the semantics of `and`, `or`, and `not`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `and`
- `or`
- `not`

**Concrete example:**
```python
user_is_admin = True
user_is_active = False
print(user_is_admin and user_is_active)
print(user_is_admin or user_is_active)
print(not user_is_active)
```

### 5.5 Bitwise Operators
Bitwise Operators defines the semantics of `&`, `|`, `^`, `~`, `<<`, and `>>`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `&`
- `|`
- `^`
- `~`
- `<<`
- `>>`

**Concrete example:**
```python
a, b = 6, 3
print(a & b, a | b, a ^ b, ~a, a << 1, a >> 1)
```

### 5.6 Membership Operators
Membership Operators defines the semantics of `in`, and `not in`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `in`
- `not in`

**Concrete example:**
```python
print("py" in "python")
print(7 not in [1, 2, 3])
```

### 5.7 Identity Operators
Identity Operators defines the semantics of `is`, and `is not`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `is`
- `is not`

**Concrete example:**
```python
a = []
b = a
c = []
print(a is b)       # True: same object
print(a is not c)   # True: different objects
```

### 5.8 Conditional Expressions
Conditional Expressions defines the semantics of Ternary expression, and Short-circuit behavior, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Ternary expression
- Short-circuit behavior

**Concrete example:**
```python
age = 17
status = "adult" if age >= 18 else "minor"
print(status)
```

### 5.9 Operator Precedence
Operator Precedence defines the semantics of Precedence rules, Associativity, and Parenthesized expressions, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Precedence rules
- Associativity
- Parenthesized expressions

**Concrete example:**
```python
print(2 + 3 * 4)      # 14
print((2 + 3) * 4)    # 20
```

---

## 6. Strings and Text Processing
Strings in Python are immutable Unicode sequences that support indexing, slicing, formatting, searching, and encoding-aware text processing.
This topic covers text construction, transformation methods, Unicode handling, and string literal forms used for raw and multiline text.

### 6.1 String Fundamentals
String Fundamentals defines the semantics of String creation, Immutability, Character indexing, and Slicing, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- String creation
- Immutability
- Character indexing
- Slicing

**Concrete example:**
```python
text = "Python"
print(text[0])      # indexing
print(text[1:4])    # slicing
# Strings are immutable, so text[0] = "J" would fail
```

### 6.2 Slicing Mechanics
Slicing Mechanics defines the semantics of Start, stop, step, Negative indices, Negative step, and Slice copying, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Start, stop, step
- Negative indices
- Negative step
- Slice copying

**Concrete example:**
```python
s = "abcdef"
print(s[1:5:2])   # bdf-like stepped slice
print(s[-4:-1])   # cde using negative indices
print(s[::-1])    # reverse via negative step
print(s[:])       # full slice copy
```

### 6.3 String Operations
String Operations defines the semantics of Concatenation, Repetition, Membership, and Lexicographic comparison, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Concatenation
- Repetition
- Membership
- Lexicographic comparison

**Concrete example:**
```python
print("Py" + "thon")      # concatenation
print("ha" * 3)           # repetition
print("th" in "python")   # membership
print("abc" < "abd")      # lexicographic comparison
```

### 6.4 String Methods
String Methods defines the semantics of Case conversion, Trimming, Search methods, Replace methods, Split and join, Prefix and suffix checks, and Alignment and padding, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Case conversion
- Trimming
- Search methods
- Replace methods
- Split and join
- Prefix and suffix checks
- Alignment and padding

**Concrete example:**
```python
s = "  hello,python  "
print(s.strip().upper())                  # trimming + case conversion
print("banana".replace("a", "o"))         # replace
print("a,b,c".split(","))                 # split
print("-".join(["a", "b", "c"]))          # join
print("report.txt".startswith("rep"))     # prefix check
print("report.txt".endswith(".txt"))      # suffix check
print("cat".center(7, "."))               # alignment/padding
```

### 6.5 String Formatting
String Formatting defines the semantics of f-strings, `str.format()`, `%` formatting, and Format specifiers, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- f-strings
- `str.format()`
- `%` formatting
- Format specifiers

**Concrete example:**
```python
name = "Riya"
score = 93.456
print(f"{name}: {score:.2f}")                 # f-string
print("{}: {:.2f}".format(name, score))       # str.format
print("%s: %.2f" % (name, score))             # %-formatting
```

### 6.6 Unicode and Encoding
Unicode and Encoding defines the semantics of Unicode code points, UTF-8 basics, `encode()`, and `decode()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Unicode code points
- UTF-8 basics
- `encode()`
- `decode()`

**Concrete example:**
```python
text = "café"
encoded = text.encode("utf-8")
decoded = encoded.decode("utf-8")
print(encoded, decoded)
```

### 6.7 Raw and Multiline Strings
Raw and Multiline Strings defines the semantics of Raw strings, and Triple-quoted strings, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Raw strings
- Triple-quoted strings

**Concrete example:**
```python
path = r"C:
ew	est"
message = """Line 1
Line 2
Line 3"""
print(path)
print(message)
```

---

## 7. Lists
Lists are mutable ordered sequence objects designed for indexed storage, dynamic resizing, and efficient append-oriented workflows.
This topic covers structural mutation, list APIs, sorting, nested list access, and list-comprehension-based construction.

### 7.1 List Fundamentals
List Fundamentals defines the semantics of List creation, Indexing, Slicing, and Mutability, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- List creation
- Indexing
- Slicing
- Mutability

**Concrete example:**
```python
items = [10, 20, 30]
print(items[0])      # indexing
print(items[1:])     # slicing
items[1] = 99        # mutability
print(items)
```

### 7.2 List Modification
List Modification defines the semantics of Item assignment, Slice assignment, Insertions, and Deletions, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Item assignment
- Slice assignment
- Insertions
- Deletions

**Concrete example:**
```python
nums = [1, 2, 3, 4]
nums[0] = 10         # item assignment
nums[1:3] = [20, 30] # slice assignment
nums.insert(1, 15)   # insertion
del nums[-1]         # deletion
print(nums)
```

### 7.3 List Methods
List Methods defines the semantics of `append()`, `extend()`, `insert()`, `remove()`, `pop()`, `clear()`, `index()`, `count()`, `sort()`, `reverse()`, and `copy()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `append()`
- `extend()`
- `insert()`
- `remove()`
- `pop()`
- `clear()`
- `index()`
- `count()`
- `sort()`
- `reverse()`
- `copy()`

**Concrete example:**
```python
data = [3, 1, 2]
data.append(4)
data.extend([5, 6])
data.insert(0, 0)
data.remove(1)
last = data.pop()
count_2 = data.count(2)
index_3 = data.index(3)
copy_data = data.copy()
data.sort()
data.reverse()
print(data, last, count_2, index_3, copy_data)
```

### 7.4 Sorting
Sorting defines the semantics of In-place sorting, `sorted()`, `key`, and `reverse`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- In-place sorting
- `sorted()`
- `key`
- `reverse`

**Concrete example:**
```python
words = ["pear", "apple", "kiwi"]
words.sort(key=len, reverse=True)      # in-place sorting
print(words)
print(sorted(words, reverse=False))    # new sorted list
```

### 7.5 Nested Lists
Nested Lists defines the semantics of Accessing nested elements, and Updating nested elements, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Accessing nested elements
- Updating nested elements

**Concrete example:**
```python
matrix = [[1, 2], [3, 4]]
print(matrix[1][0])     # access nested element
matrix[0][1] = 99       # update nested element
print(matrix)
```

### 7.6 List Comprehensions
List Comprehensions defines the semantics of Basic comprehensions, Conditional comprehensions, Nested comprehensions, and Multiple `for` clauses, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Basic comprehensions
- Conditional comprehensions
- Nested comprehensions
- Multiple `for` clauses

**Concrete example:**
```python
squares = [x * x for x in range(5)]                       # basic
evens = [x for x in range(10) if x % 2 == 0]             # conditional
pairs = [(i, j) for i in range(2) for j in range(2)]     # multiple for clauses
flat = [item for row in [[1, 2], [3, 4]] for item in row]# nested
print(squares, evens, pairs, flat)
```

---

## 8. Tuples
Tuples are immutable ordered sequences commonly used for fixed records, grouping values, and multi-value returns from functions.
This topic covers tuple syntax, access patterns, limited built-in methods, and packing and unpacking mechanics.

### 8.1 Tuple Fundamentals
Tuple Fundamentals defines the semantics of Tuple creation, Single-element tuples, Indexing, Slicing, and Immutability, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Tuple creation
- Single-element tuples
- Indexing
- Slicing
- Immutability

**Concrete example:**
```python
point = (10, 20)
single = (5,)          # single-element tuple
print(point[0], point[:])
# point[0] = 99 would fail because tuples are immutable
```

### 8.2 Tuple Operations
Tuple Operations defines the semantics of Concatenation, Repetition, and Membership, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Concatenation
- Repetition
- Membership

**Concrete example:**
```python
a = (1, 2)
b = (3, 4)
print(a + b)           # concatenation
print(a * 2)           # repetition
print(2 in a)          # membership
```

### 8.3 Tuple Methods
Tuple Methods defines the semantics of `count()`, and `index()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `count()`
- `index()`

**Concrete example:**
```python
values = (1, 2, 2, 3)
print(values.count(2))
print(values.index(3))
```

### 8.4 Packing and Unpacking
Packing and Unpacking defines the semantics of Tuple packing, Tuple unpacking, Extended unpacking, and Swapping values, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Tuple packing
- Tuple unpacking
- Extended unpacking
- Swapping values

**Concrete example:**
```python
packed = 1, 2, 3            # tuple packing
a, b, c = packed            # tuple unpacking
first, *middle, last = [10, 20, 30, 40]  # extended unpacking
x, y = 1, 2
x, y = y, x                  # swapping values
```

---

## 9. Sets
Sets model unordered collections of unique hashable elements and provide efficient membership testing and algebraic set operations.
This topic covers mutable sets, immutable frozensets, update methods, and logical relations such as subset and superset checks.

### 9.1 Set Fundamentals
Set Fundamentals defines the semantics of Set creation, Unordered collection semantics, and Uniqueness, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Set creation
- Unordered collection semantics
- Uniqueness

**Concrete example:**
```python
values = {1, 2, 2, 3}
print(values)       # duplicates removed, order not guaranteed
```

### 9.2 Set Operations
Set Operations defines the semantics of Union, Intersection, Difference, Symmetric difference, and Subset and superset tests, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Union
- Intersection
- Difference
- Symmetric difference
- Subset and superset tests

**Concrete example:**
```python
a = {1, 2, 3}
b = {3, 4, 5}
print(a | b)        # union
print(a & b)        # intersection
print(a - b)        # difference
print(a ^ b)        # symmetric difference
print({1, 2} <= a, a >= {1, 2})  # subset / superset tests
```

### 9.3 Set Methods
Set Methods defines the semantics of `add()`, `update()`, `remove()`, `discard()`, `pop()`, `clear()`, `union()`, `intersection()`, `difference()`, and `symmetric_difference()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `add()`
- `update()`
- `remove()`
- `discard()`
- `pop()`
- `clear()`
- `union()`
- `intersection()`
- `difference()`
- `symmetric_difference()`

**Concrete example:**
```python
s = {1, 2}
s.add(3)
s.update([4, 5])
s.remove(2)
s.discard(99)        # no error if absent
popped = s.pop()
u = s.union({10})
i = s.intersection({1, 10})
d = s.difference({1})
sd = s.symmetric_difference({10, 20})
print(s, popped, u, i, d, sd)
s.clear()
```

### 9.4 Frozenset
Frozenset defines the semantics of Immutable set semantics, and Hashability, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Immutable set semantics
- Hashability

**Concrete example:**
```python
fs = frozenset([1, 2, 3])
lookup = {fs: "immutable and hashable"}
print(lookup[fs])
```

---

## 10. Dictionaries
Dictionaries are hash-table-backed mappings from hashable keys to arbitrary values, optimized for keyed lookup and update.
This topic covers construction, mutation, iteration, comprehensions, and nested mapping patterns used in structured data.

### 10.1 Dictionary Fundamentals
Dictionary Fundamentals defines the semantics of Dictionary creation, Key-value mapping, Hashable keys, and Insertion-order behavior, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Dictionary creation
- Key-value mapping
- Hashable keys
- Insertion-order behavior

**Concrete example:**
```python
user = {"id": 1, "name": "Ana"}
user["role"] = "admin"
print(user)   # insertion order is preserved
```

### 10.2 Access and Modification
Access and Modification defines the semantics of Key lookup, Item assignment, Updating values, and Deleting items, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Key lookup
- Item assignment
- Updating values
- Deleting items

**Concrete example:**
```python
config = {"host": "localhost", "port": 8000}
print(config["host"])   # key lookup
config["port"] = 8080   # update
config["debug"] = True  # assignment
del config["host"]      # deletion
print(config)
```

### 10.3 Dictionary Methods
Dictionary Methods defines the semantics of `get()`, `keys()`, `values()`, `items()`, `update()`, `pop()`, `popitem()`, `clear()`, `copy()`, and `setdefault()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `get()`
- `keys()`
- `values()`
- `items()`
- `update()`
- `pop()`
- `popitem()`
- `clear()`
- `copy()`
- `setdefault()`

**Concrete example:**
```python
d = {"a": 1, "b": 2}
print(d.get("c", 0))
print(list(d.keys()), list(d.values()), list(d.items()))
d.update({"b": 20, "c": 3})
removed = d.pop("a")
last = d.popitem()
d.setdefault("x", 100)
copy_d = d.copy()
print(removed, last, copy_d)
d.clear()
```

### 10.4 Dictionary Iteration
Dictionary Iteration defines the semantics of Iterating keys, Iterating values, and Iterating items, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Iterating keys
- Iterating values
- Iterating items

**Concrete example:**
```python
data = {"x": 1, "y": 2}
for key in data:
    print("key:", key)
for value in data.values():
    print("value:", value)
for key, value in data.items():
    print(key, value)
```

### 10.5 Dictionary Comprehensions
Dictionary Comprehensions defines the semantics of Basic dictionary comprehensions, and Conditional dictionary comprehensions, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Basic dictionary comprehensions
- Conditional dictionary comprehensions

**Concrete example:**
```python
squares = {x: x * x for x in range(5)}
evens = {x: x * x for x in range(6) if x % 2 == 0}
print(squares, evens)
```

### 10.6 Nested Dictionaries
Nested Dictionaries defines the semantics of Access, Update, and Traversal, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Access
- Update
- Traversal

**Concrete example:**
```python
student = {
    "name": "Lina",
    "marks": {"math": 95, "science": 91}
}
print(student["marks"]["math"])
student["marks"]["math"] = 97
for subject, score in student["marks"].items():
    print(subject, score)
```

---

## 11. Control Flow
Control flow determines the order in which statements run, whether branches execute, and how loops continue, terminate, or skip work.
This topic covers conditionals, iterative statements, loop modifiers, loop else clauses, and pattern-directed dispatch.

### 11.1 Conditional Statements
Conditional Statements defines the semantics of `if`, `if-else`, `if-elif-else`, and Nested conditionals, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `if`
- `if-else`
- `if-elif-else`
- Nested conditionals

**Concrete example:**
```python
x = 7
if x < 0:
    print("negative")
elif x == 0:
    print("zero")
else:
    if x % 2 == 0:
        print("positive even")
    else:
        print("positive odd")
```

### 11.2 Looping
Looping defines the semantics of `for`, `while`, and Nested loops, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `for`
- `while`
- Nested loops

**Concrete example:**
```python
for ch in "abc":
    print(ch)

n = 3
while n > 0:
    print(n)
    n -= 1

for i in range(2):
    for j in range(2):
        print(i, j)
```

### 11.3 Loop Control Statements
Loop Control Statements defines the semantics of `break`, `continue`, and `pass`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `break`
- `continue`
- `pass`

**Concrete example:**
```python
for x in range(5):
    if x == 1:
        continue
    if x == 4:
        break
    print(x)

if True:
    pass
```

### 11.4 Loop `else`
Loop `else` defines the semantics of `for-else`, and `while-else`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `for-else`
- `while-else`

**Concrete example:**
```python
for n in [2, 4, 6]:
    if n % 2 != 0:
        break
else:
    print("No odd numbers found")

count = 0
while count < 3:
    count += 1
else:
    print("while loop ended normally")
```

### 11.5 Structural Pattern Matching
Structural Pattern Matching defines the semantics of `match`, `case`, Literal patterns, Sequence patterns, Mapping patterns, and Guard clauses, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `match`
- `case`
- Literal patterns
- Sequence patterns
- Mapping patterns
- Guard clauses

**Concrete example:**
```python
command = {"action": "move", "x": 3, "y": 4}

match command:
    case {"action": "move", "x": x, "y": y} if x >= 0 and y >= 0:
        print("move to", x, y)
    case {"action": "quit"}:
        print("quit")
    case _:
        print("unknown command")
```

---

## 12. Functions
Functions define callable units of behavior with explicit parameter lists, return semantics, lexical scoping, and optional type metadata.
This topic covers declaration, argument passing, closures, recursion, lambda expressions, annotations, and core functional helpers.

### 12.1 Function Definition
Function Definition defines the semantics of `def`, Function body, Return values, and Implicit `None`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `def`
- Function body
- Return values
- Implicit `None`

**Concrete example:**
```python
def add(a, b):
    return a + b

def log_message(msg):
    print(msg)   # implicit None return

print(add(2, 3))
print(log_message("done"))
```

### 12.2 Parameters and Arguments
Parameters and Arguments defines the semantics of Positional arguments, Keyword arguments, Default arguments, Variable-length arguments, `*args`, and `**kwargs`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Positional arguments
- Keyword arguments
- Default arguments
- Variable-length arguments
- `*args`
- `**kwargs`

**Concrete example:**
```python
def demo(a, b=0, *args, **kwargs):
    return a, b, args, kwargs

print(demo(1, 2, 3, 4, mode="fast"))
print(demo(a=5, b=6))
```

### 12.3 Advanced Parameter Kinds
Advanced Parameter Kinds defines the semantics of Positional-only parameters, Keyword-only parameters, and Parameter ordering rules, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Positional-only parameters
- Keyword-only parameters
- Parameter ordering rules

**Concrete example:**
```python
def f(a, b, /, c, *, d):
    return a, b, c, d

print(f(1, 2, 3, d=4))
# a and b are positional-only; d is keyword-only
```

### 12.4 Scope and Namespace Interaction
Scope and Namespace Interaction defines the semantics of Local scope, Global scope, Enclosing scope, Built-in scope, LEGB rule, `global`, and `nonlocal`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Local scope
- Global scope
- Enclosing scope
- Built-in scope
- LEGB rule
- `global`
- `nonlocal`

**Concrete example:**
```python
x = "global"

def outer():
    x = "enclosing"
    def inner():
        nonlocal x
        x = "changed in enclosing scope"
    inner()
    return x

print(outer())
print(x)
```

### 12.5 First-Class Functions
First-Class Functions defines the semantics of Functions as objects, Passing functions, Returning functions, and Assigning functions to names, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Functions as objects
- Passing functions
- Returning functions
- Assigning functions to names

**Concrete example:**
```python
def square(x):
    return x * x

def apply(func, value):
    return func(value)

fn = square
print(apply(fn, 5))
```

### 12.6 Lambda Functions
Lambda Functions defines the semantics of Anonymous functions, and Expression-only constraint, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Anonymous functions
- Expression-only constraint

**Concrete example:**
```python
pairs = [(1, "b"), (2, "a")]
print(sorted(pairs, key=lambda item: item[1]))
```

### 12.7 Recursion
Recursion defines the semantics of Recursive calls, Base case, and Recursion depth considerations, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Recursive calls
- Base case
- Recursion depth considerations

**Concrete example:**
```python
def factorial(n):
    if n == 0:          # base case
        return 1
    return n * factorial(n - 1)

print(factorial(5))
```

### 12.8 Function Annotations
Function Annotations defines the semantics of Parameter annotations, Return annotations, and `__annotations__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Parameter annotations
- Return annotations
- `__annotations__`

**Concrete example:**
```python
def area(radius: float) -> float:
    return 3.14159 * radius ** 2

print(area.__annotations__)
```

### 12.9 Built-in Functional Tools
Built-in Functional Tools defines the semantics of `map()`, `filter()`, `zip()`, `enumerate()`, `sorted()`, `any()`, and `all()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `map()`
- `filter()`
- `zip()`
- `enumerate()`
- `sorted()`
- `any()`
- `all()`

**Concrete example:**
```python
nums = [1, 2, 3, 4]
print(list(map(lambda x: x * 2, nums)))
print(list(filter(lambda x: x % 2 == 0, nums)))
print(list(zip(["a", "b"], nums)))
print(list(enumerate(["x", "y"], start=1)))
print(sorted(["pear", "kiwi", "apple"], key=len))
print(any([False, True, False]), all([True, True, True]))
```

### 12.10 Docstrings
Docstrings defines the semantics of Function docstrings, and Access via `__doc__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Function docstrings
- Access via `__doc__`

**Concrete example:**
```python
def greet(name):
    """Return a polite greeting for the supplied name."""
    return f"Hello, {name}"

print(greet.__doc__)
```

---

## 13. Comprehensions and Generator Expressions
Comprehensions and generator expressions provide compact syntax for deriving new collections or lazy streams from existing iterables.
They combine iteration, optional filtering, and expression evaluation into a single construct with well-defined local scope behavior.

### 13.1 List Comprehensions
List Comprehensions defines the semantics of Single-expression form, Conditional filtering, and Nested comprehensions, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Single-expression form
- Conditional filtering
- Nested comprehensions

**Concrete example:**
```python
squares = [x * x for x in range(6)]
evens = [x for x in range(10) if x % 2 == 0]
matrix = [[i * j for j in range(3)] for i in range(3)]
print(squares, evens, matrix)
```

### 13.2 Set Comprehensions
Set Comprehensions defines the semantics of Unique-value generation, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Unique-value generation

**Concrete example:**
```python
unique_lengths = {len(word) for word in ["cat", "apple", "dog", "pear"]}
print(unique_lengths)
```

### 13.3 Dictionary Comprehensions
Dictionary Comprehensions defines the semantics of Key-value generation, and Conditional generation, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Key-value generation
- Conditional generation

**Concrete example:**
```python
mapping = {x: x * x for x in range(5)}
evens = {x: x * x for x in range(6) if x % 2 == 0}
print(mapping, evens)
```

### 13.4 Generator Expressions
Generator Expressions defines the semantics of Lazy evaluation, and Parenthesized generator syntax, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Lazy evaluation
- Parenthesized generator syntax

**Concrete example:**
```python
gen = (x * x for x in range(5))
print(next(gen))
print(sum(gen))   # lazy evaluation of remaining items
```

### 13.5 Scope Behavior
Scope Behavior defines the semantics of Comprehension variable scope, and Temporary iteration variables, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Comprehension variable scope
- Temporary iteration variables

**Concrete example:**
```python
x = "outer"
items = [x * 2 for x in range(3)]
print(items)
print(x)   # still 'outer'; comprehension variable does not leak
```

---

## 14. Iterables, Iterators, and Generators
Python iteration is protocol-based: iterables produce iterators, iterators yield successive values, and generators implement resumable lazy execution.
This topic covers the mechanics of `iter`, `next`, `StopIteration`, custom iterators, and advanced generator control methods.

### 14.1 Iterable Protocol
Iterable Protocol defines the semantics of Iterable objects, and `iter()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Iterable objects
- `iter()`

**Concrete example:**
```python
items = [10, 20, 30]
it = iter(items)
print(next(it), next(it))
```

### 14.2 Iterator Protocol
Iterator Protocol defines the semantics of `__iter__()`, `__next__()`, `next()`, and `StopIteration`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__iter__()`
- `__next__()`
- `next()`
- `StopIteration`

**Concrete example:**
```python
it = iter([1, 2])
print(next(it))
print(next(it))
try:
    print(next(it))
except StopIteration:
    print("iterator exhausted")
```

### 14.3 Custom Iterators
Custom Iterators defines the semantics of Iterator classes, and Stateful iteration, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Iterator classes
- Stateful iteration

**Concrete example:**
```python
class CountDown:
    def __init__(self, start):
        self.current = start
    def __iter__(self):
        return self
    def __next__(self):
        if self.current < 0:
            raise StopIteration
        value = self.current
        self.current -= 1
        return value

print(list(CountDown(3)))
```

### 14.4 Generator Functions
Generator Functions defines the semantics of `yield`, Generator state, and Resumable execution, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `yield`
- Generator state
- Resumable execution

**Concrete example:**
```python
def countdown(n):
    while n >= 0:
        yield n
        n -= 1

for value in countdown(3):
    print(value)
```

### 14.5 Generator Methods
Generator Methods defines the semantics of `send()`, `throw()`, and `close()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `send()`
- `throw()`
- `close()`

**Concrete example:**
```python
def echo():
    try:
        while True:
            value = yield
            print("received:", value)
    except GeneratorExit:
        print("closing")

g = echo()
next(g)
g.send("hello")
g.close()
```

### 14.6 Generator Use Cases
Generator Use Cases defines the semantics of Lazy computation, Streaming data, and Memory efficiency, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Lazy computation
- Streaming data
- Memory efficiency

**Concrete example:**
```python
def read_numbers():
    for line in ["1", "2", "3"]:
        yield int(line)

total = sum(read_numbers())  # lazy, streaming-style processing
print(total)
```

---

## 15. Modules and Packages
Modules and packages organize Python code into reusable namespaces that can be imported, cached, and composed into larger applications.
This topic covers import forms, module metadata, standard-library modules, package layout, and module search-path resolution.

### 15.1 Modules
Modules defines the semantics of Module files, `import`, `from ... import`, and `as`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Module files
- `import`
- `from ... import`
- `as`

**Concrete example:**
```python
import math
from math import sqrt as root

print(math.pi)
print(root(16))
```

### 15.2 Module Attributes
Module Attributes defines the semantics of `__name__`, `__file__`, and `dir()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__name__`
- `__file__`
- `dir()`

**Concrete example:**
```python
import math
print(math.__name__)
print(hasattr(math, "__file__"))
print("sqrt" in dir(math))
```

### 15.3 Import System Basics
Import System Basics defines the semantics of Import resolution, Module caching, and Re-import behavior, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Import resolution
- Module caching
- Re-import behavior

**Concrete example:**
```python
import math
import math   # second import reuses cached module object
print(id(math))
```

### 15.4 Standard Library Modules
Standard Library Modules defines the semantics of `math`, `random`, `datetime`, `os`, `sys`, and `pathlib`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `math`
- `random`
- `datetime`
- `os`
- `sys`
- `pathlib`

**Concrete example:**
```python
import math, random, datetime, os, sys, pathlib

print(math.sqrt(25))
print(random.randint(1, 3))
print(datetime.date.today())
print(os.getcwd())
print(sys.version_info.major)
print(pathlib.Path(".").resolve())
```

### 15.5 User-Defined Modules
User-Defined Modules defines the semantics of Creating modules, and Organizing module code, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Creating modules
- Organizing module code

**Concrete example:**
```python
# mytools.py
def greet(name):
    return f"Hello, {name}"

# main.py
# import mytools
# print(mytools.greet("Ana"))
```

### 15.6 Packages
Packages defines the semantics of Package directories, `__init__.py`, and Subpackages, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Package directories
- `__init__.py`
- Subpackages

**Concrete example:**
```python
# project/
#   tools/
#     __init__.py
#     mathops.py

# from tools import mathops
# Subpackages are nested package directories.
```

### 15.7 Module Search Path
Module Search Path defines the semantics of `sys.path`, Current working directory, and Installed locations, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `sys.path`
- Current working directory
- Installed locations

**Concrete example:**
```python
import sys
print(sys.path[0])   # current working directory or script directory
```

---

## 16. File Handling and I/O
File handling and I/O provide stream-based access to persistent data through file objects that support reading, writing, appending, and seeking.
This topic covers mode flags, text versus binary semantics, read/write APIs, and deterministic cleanup using context management.

### 16.1 File Operations
File Operations defines the semantics of Open, Read, Write, Append, and Close, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Open
- Read
- Write
- Append
- Close

**Concrete example:**
```python
with open("sample.txt", "w", encoding="utf-8") as f:
    f.write("hello\n")

with open("sample.txt", "a", encoding="utf-8") as f:
    f.write("world\n")

with open("sample.txt", "r", encoding="utf-8") as f:
    print(f.read())
```

### 16.2 File Modes
File Modes defines the semantics of `r`, `w`, `a`, `x`, `b`, `t`, and `+`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `r`
- `w`
- `a`
- `x`
- `b`
- `t`
- `+`

**Concrete example:**
```python
# r  -> read text
# w  -> write text (truncate)
# a  -> append text
# x  -> exclusive creation
# b  -> binary mode
# t  -> text mode
# +  -> update mode (read/write)
```

### 16.3 Reading APIs
Reading APIs defines the semantics of `read()`, `readline()`, `readlines()`, and Iterating over files, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `read()`
- `readline()`
- `readlines()`
- Iterating over files

**Concrete example:**
```python
with open("sample.txt", "r", encoding="utf-8") as f:
    print(f.readline())
with open("sample.txt", "r", encoding="utf-8") as f:
    print(f.readlines())
with open("sample.txt", "r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())
```

### 16.4 Writing APIs
Writing APIs defines the semantics of `write()`, and `writelines()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `write()`
- `writelines()`

**Concrete example:**
```python
with open("out.txt", "w", encoding="utf-8") as f:
    f.write("first line\n")
    f.writelines(["second line\n", "third line\n"])
```

### 16.5 File Positioning
File Positioning defines the semantics of `tell()`, and `seek()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `tell()`
- `seek()`

**Concrete example:**
```python
with open("out.txt", "r", encoding="utf-8") as f:
    print(f.tell())
    print(f.read(5))
    f.seek(0)
    print(f.read(5))
```

### 16.6 Text vs Binary I/O
Text vs Binary I/O defines the semantics of Text streams, Binary streams, and Encoding considerations, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Text streams
- Binary streams
- Encoding considerations

**Concrete example:**
```python
with open("out.txt", "r", encoding="utf-8") as f:
    text_data = f.read()

with open("out.txt", "rb") as f:
    binary_data = f.read()

print(type(text_data), type(binary_data))
```

### 16.7 Context Management for Files
Context Management for Files defines the semantics of `with open()`, and Automatic cleanup, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `with open()`
- Automatic cleanup

**Concrete example:**
```python
with open("out.txt", "r", encoding="utf-8") as f:
    content = f.read()
print("file automatically closed after the with-block")
```

---

## 17. Exception Handling
Exception handling models abnormal conditions as exception objects that propagate up the call stack until caught or allowed to terminate execution.
This topic covers try/except control flow, matching rules, explicit raising, custom exception classes, and assertion semantics.

### 17.1 Exception Model
Exception Model defines the semantics of Syntax errors vs exceptions, Exception propagation, and Stack unwinding, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Syntax errors vs exceptions
- Exception propagation
- Stack unwinding

**Concrete example:**
```python
try:
    value = 10 / 0
except ZeroDivisionError as exc:
    print(type(exc).__name__, "was raised and propagated to this handler")
```

### 17.2 Exception Blocks
Exception Blocks defines the semantics of `try`, `except`, `else`, and `finally`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `try`
- `except`
- `else`
- `finally`

**Concrete example:**
```python
try:
    x = int("42")
except ValueError:
    print("bad input")
else:
    print("parsed:", x)
finally:
    print("cleanup always runs")
```

### 17.3 Exception Matching
Exception Matching defines the semantics of Specific exceptions, Multiple exception types, and Exception hierarchy basics, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Specific exceptions
- Multiple exception types
- Exception hierarchy basics

**Concrete example:**
```python
try:
    open("missing.txt")
except (FileNotFoundError, PermissionError) as exc:
    print("handled:", type(exc).__name__)
```

### 17.4 Raising Exceptions
Raising Exceptions defines the semantics of `raise`, and Re-raising exceptions, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `raise`
- Re-raising exceptions

**Concrete example:**
```python
def set_age(age):
    if age < 0:
        raise ValueError("age must be non-negative")
    return age
```

### 17.5 Custom Exceptions
Custom Exceptions defines the semantics of Inheriting from `Exception`, and Domain-specific error classes, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Inheriting from `Exception`
- Domain-specific error classes

**Concrete example:**
```python
class InvalidTokenError(Exception):
    pass

def authenticate(token):
    if token != "secret":
        raise InvalidTokenError("token rejected")
```

### 17.6 Assertions
Assertions defines the semantics of `assert`, and Assertion failure behavior, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `assert`
- Assertion failure behavior

**Concrete example:**
```python
value = 5
assert value > 0, "value must be positive"
```

---

## 18. Object-Oriented Programming
Python’s object-oriented model represents state and behavior with classes, instances, attributes, methods, inheritance, and method dispatch rules.
This topic covers initialization, attribute lookup, encapsulation conventions, abstraction tools, properties, dataclasses, and enums.

### 18.1 Classes and Objects
Classes and Objects defines the semantics of Class definition, Object instantiation, Attributes, and Methods, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Class definition
- Object instantiation
- Attributes
- Methods

**Concrete example:**
```python
class User:
    def __init__(self, name):
        self.name = name
    def greet(self):
        return f"Hello, {self.name}"

u = User("Ana")
print(u.name, u.greet())
```

### 18.2 Constructors and Initialization
Constructors and Initialization defines the semantics of `__new__`, and `__init__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__new__`
- `__init__`

**Concrete example:**
```python
class Point:
    def __new__(cls, x, y):
        return super().__new__(cls)
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(2, 3)
print(p.x, p.y)
```

### 18.3 Attribute Access
Attribute Access defines the semantics of Instance attributes, Class attributes, and Attribute lookup order, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Instance attributes
- Class attributes
- Attribute lookup order

**Concrete example:**
```python
class Demo:
    class_attr = "shared"
    def __init__(self):
        self.instance_attr = "per-object"

d = Demo()
print(d.instance_attr)
print(d.class_attr)
```

### 18.4 Method Types
Method Types defines the semantics of Instance methods, Class methods, and Static methods, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Instance methods
- Class methods
- Static methods

**Concrete example:**
```python
class Example:
    def instance_method(self):
        return "instance"
    @classmethod
    def class_method(cls):
        return cls.__name__
    @staticmethod
    def static_method():
        return "utility"

e = Example()
print(e.instance_method(), Example.class_method(), Example.static_method())
```

### 18.5 Encapsulation
Encapsulation defines the semantics of Public naming convention, Protected naming convention, and Name mangling for private members, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Public naming convention
- Protected naming convention
- Name mangling for private members

**Concrete example:**
```python
class Account:
    def __init__(self):
        self.public = "visible"
        self._protected = "internal convention"
        self.__private = "name-mangled"

a = Account()
print(a.public, a._protected)
print(a._Account__private)
```

### 18.6 Inheritance
Inheritance defines the semantics of Single inheritance, Multiple inheritance, Multilevel inheritance, and Hierarchical inheritance, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Single inheritance
- Multiple inheritance
- Multilevel inheritance
- Hierarchical inheritance

**Concrete example:**
```python
class Animal:
    def speak(self):
        return "sound"

class Dog(Animal):               # single inheritance
    pass

class Robot:
    def charge(self):
        return "charging"

class RoboDog(Dog, Robot):       # multiple inheritance
    pass

print(RoboDog().speak(), RoboDog().charge())
```

### 18.7 Method Resolution
Method Resolution defines the semantics of MRO, and `super()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- MRO
- `super()`

**Concrete example:**
```python
class A:
    def greet(self):
        return "A"

class B(A):
    def greet(self):
        return "B -> " + super().greet()

print(B.__mro__)
print(B().greet())
```

### 18.8 Polymorphism
Polymorphism defines the semantics of Method overriding, and Duck typing, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Method overriding
- Duck typing

**Concrete example:**
```python
class Cat:
    def speak(self):
        return "meow"

class Dog:
    def speak(self):
        return "woof"

for animal in [Cat(), Dog()]:   # duck-typed use of same method name
    print(animal.speak())
```

### 18.9 Abstraction
Abstraction defines the semantics of Abstract base classes, `abc` module, and `@abstractmethod`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Abstract base classes
- `abc` module
- `@abstractmethod`

**Concrete example:**
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Square(Shape):
    def __init__(self, side):
        self.side = side
    def area(self):
        return self.side ** 2

print(Square(4).area())
```

### 18.10 Properties
Properties defines the semantics of `@property`, Setter methods, and Deleter methods, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `@property`
- Setter methods
- Deleter methods

**Concrete example:**
```python
class Temperature:
    def __init__(self, c):
        self._c = c
    @property
    def celsius(self):
        return self._c
    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("below absolute zero")
        self._c = value
    @celsius.deleter
    def celsius(self):
        del self._c
```

### 18.11 Dataclasses
Dataclasses defines the semantics of `@dataclass`, Generated methods, and Field defaults, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `@dataclass`
- Generated methods
- Field defaults

**Concrete example:**
```python
from dataclasses import dataclass

@dataclass
class Product:
    name: str
    price: float = 0.0

p = Product("Pen", 1.5)
print(p)
```

### 18.12 Enums
Enums defines the semantics of `Enum`, and Named constants, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `Enum`
- Named constants

**Concrete example:**
```python
from enum import Enum

class Status(Enum):
    NEW = 1
    DONE = 2

print(Status.NEW.name, Status.NEW.value)
```

---

## 19. Special Methods and Data Model
Python’s data model lets user-defined classes integrate with built-in syntax by implementing special methods that the interpreter calls implicitly.
This topic covers string representation, comparisons, container operations, callability, context behavior, iteration hooks, and operator overloading.

### 19.1 Object Representation
Object Representation defines the semantics of `__str__`, and `__repr__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__str__`
- `__repr__`

**Concrete example:**
```python
class User:
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return f"User({self.name})"
    def __repr__(self):
        return f"User(name={self.name!r})"

u = User("Ana")
print(str(u))
print(repr(u))
```

### 19.2 Comparison Methods
Comparison Methods defines the semantics of `__eq__`, `__lt__`, and `__gt__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__eq__`
- `__lt__`
- `__gt__`

**Concrete example:**
```python
class Box:
    def __init__(self, size):
        self.size = size
    def __eq__(self, other):
        return self.size == other.size
    def __lt__(self, other):
        return self.size < other.size
    def __gt__(self, other):
        return self.size > other.size

print(Box(2) == Box(2), Box(1) < Box(3), Box(4) > Box(2))
```

### 19.3 Container Methods
Container Methods defines the semantics of `__len__`, `__getitem__`, `__setitem__`, and `__contains__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__len__`
- `__getitem__`
- `__setitem__`
- `__contains__`

**Concrete example:**
```python
class Bag:
    def __init__(self):
        self.items = []
    def __len__(self):
        return len(self.items)
    def __getitem__(self, index):
        return self.items[index]
    def __setitem__(self, index, value):
        self.items[index] = value
    def __contains__(self, value):
        return value in self.items

b = Bag()
b.items = [10, 20]
b[1] = 99
print(len(b), b[0], 99 in b)
```

### 19.4 Callable and Context Behavior
Callable and Context Behavior defines the semantics of `__call__`, `__enter__`, and `__exit__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__call__`
- `__enter__`
- `__exit__`

**Concrete example:**
```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor
    def __call__(self, value):
        return value * self.factor
    def __enter__(self):
        print("enter")
        return self
    def __exit__(self, exc_type, exc, tb):
        print("exit")

with Multiplier(3) as m:
    print(m(5))
```

### 19.5 Iteration Hooks
Iteration Hooks defines the semantics of `__iter__`, and `__next__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__iter__`
- `__next__`

**Concrete example:**
```python
class Counter:
    def __init__(self, n):
        self.n = n
        self.i = 0
    def __iter__(self):
        return self
    def __next__(self):
        if self.i >= self.n:
            raise StopIteration
        self.i += 1
        return self.i

print(list(Counter(3)))
```

### 19.6 Arithmetic Operator Overloading
Arithmetic Operator Overloading defines the semantics of `__add__`, `__sub__`, and `__mul__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__add__`
- `__sub__`
- `__mul__`

**Concrete example:**
```python
class Vector:
    def __init__(self, x):
        self.x = x
    def __add__(self, other):
        return Vector(self.x + other.x)
    def __sub__(self, other):
        return Vector(self.x - other.x)
    def __mul__(self, other):
        return Vector(self.x * other)
    def __repr__(self):
        return f"Vector({self.x})"

print(Vector(2) + Vector(3), Vector(5) - Vector(1), Vector(4) * 3)
```

---

## 20. Decorators
Decorators are higher-order callables that transform functions, methods, or classes at definition time without changing call sites.
This topic covers wrapper patterns, parameterized decorators, metadata preservation, composition order, and built-in decorator utilities.

### 20.1 Function Decorators
Function Decorators defines the semantics of Wrapper functions, and Higher-order decorator pattern, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Wrapper functions
- Higher-order decorator pattern

**Concrete example:**
```python
def log_calls(func):
    def wrapper(*args, **kwargs):
        print("calling", func.__name__)
        return func(*args, **kwargs)
    return wrapper

@log_calls
def add(a, b):
    return a + b

print(add(2, 3))
```

### 20.2 Decorator Factories
Decorator Factories defines the semantics of Decorators with arguments, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Decorators with arguments

**Concrete example:**
```python
def repeat(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            result = None
            for _ in range(n):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(2)
def greet():
    print("hello")
```

### 20.3 Metadata Preservation
Metadata Preservation defines the semantics of `functools.wraps`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `functools.wraps`

**Concrete example:**
```python
from functools import wraps

def log(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper
```

### 20.4 Stacked Decorators
Stacked Decorators defines the semantics of Multiple decorator order, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Multiple decorator order

**Concrete example:**
```python
def deco1(func):
    def wrapper():
        return "deco1(" + func() + ")"
    return wrapper

def deco2(func):
    def wrapper():
        return "deco2(" + func() + ")"
    return wrapper

@deco1
@deco2
def base():
    return "base"

print(base())
```

### 20.5 Class and Method Decorators
Class and Method Decorators defines the semantics of Class decorators, and Method decoration, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Class decorators
- Method decoration

**Concrete example:**
```python
def add_flag(cls):
    cls.flag = True
    return cls

@add_flag
class Demo:
    @staticmethod
    def ping():
        return "pong"

print(Demo.flag, Demo.ping())
```

### 20.6 Built-in Decorators
Built-in Decorators defines the semantics of `@staticmethod`, `@classmethod`, and `@property`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `@staticmethod`
- `@classmethod`
- `@property`

**Concrete example:**
```python
class Sample:
    @staticmethod
    def s():
        return "static"
    @classmethod
    def c(cls):
        return cls.__name__
    @property
    def p(self):
        return "property"

obj = Sample()
print(Sample.s(), Sample.c(), obj.p)
```

---

## 21. Context Managers
Context managers define structured acquisition and release of resources around a controlled execution block.
This topic covers the context-management protocol, `with` statements, custom managers, and helpers built on `contextlib`.

### 21.1 Context Management Protocol
Context Management Protocol defines the semantics of `__enter__`, and `__exit__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__enter__`
- `__exit__`

**Concrete example:**
```python
class Managed:
    def __enter__(self):
        print("resource acquired")
        return self
    def __exit__(self, exc_type, exc, tb):
        print("resource released")

with Managed():
    print("inside block")
```

### 21.2 Using `with`
Using `with` defines the semantics of Resource acquisition, and Guaranteed cleanup, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Resource acquisition
- Guaranteed cleanup

**Concrete example:**
```python
with open("demo.txt", "w", encoding="utf-8") as f:
    f.write("safe resource handling")
```

### 21.3 Custom Context Managers
Custom Context Managers defines the semantics of Class-based context managers, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Class-based context managers

**Concrete example:**
```python
class Timer:
    def __enter__(self):
        import time
        self.start = time.time()
        return self
    def __exit__(self, exc_type, exc, tb):
        import time
        print("elapsed:", round(time.time() - self.start, 4))

with Timer():
    sum(range(10000))
```

### 21.4 `contextlib`
`contextlib` defines the semantics of `contextmanager`, and Utility helpers, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `contextmanager`
- Utility helpers

**Concrete example:**
```python
from contextlib import contextmanager

@contextmanager
def managed_list():
    data = []
    print("open")
    try:
        yield data
    finally:
        print("close")

with managed_list() as items:
    items.append(1)
```

---

## 22. Regular Expressions
Regular expressions are declarative text-pattern specifications used for matching, extracting, splitting, and rewriting strings.
This topic covers regex syntax, the core `re` APIs, and the information returned by match objects.

### 22.1 Regex Syntax
Regex Syntax defines the semantics of Literals, Metacharacters, Character classes, Quantifiers, Groups, Alternation, and Anchors, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Literals
- Metacharacters
- Character classes
- Quantifiers
- Groups
- Alternation
- Anchors

**Concrete example:**
```python
import re
pattern = r"^\w+(\.\w+)*@\w+\.com$"
print(bool(re.fullmatch(pattern, "user.name@example.com")))
```

### 22.2 `re` Module APIs
`re` Module APIs defines the semantics of `match()`, `search()`, `fullmatch()`, `findall()`, `finditer()`, `sub()`, `split()`, and `compile()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `match()`
- `search()`
- `fullmatch()`
- `findall()`
- `finditer()`
- `sub()`
- `split()`
- `compile()`

**Concrete example:**
```python
import re
text = "cat bat cat"
print(re.match(r"cat", text))
print(re.search(r"bat", text))
print(re.fullmatch(r"cat bat cat", text))
print(re.findall(r"cat", text))
print([m.span() for m in re.finditer(r"cat", text)])
print(re.sub(r"cat", "dog", text))
print(re.split(r" ", text))
compiled = re.compile(r"cat")
print(compiled.findall(text))
```

### 22.3 Match Objects
Match Objects defines the semantics of Matched spans, Groups, and Group extraction, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Matched spans
- Groups
- Group extraction

**Concrete example:**
```python
import re
m = re.search(r"(\d+)-(\w+)", "Order 123-XYZ ready")
print(m.span(), m.group(0), m.group(1), m.group(2))
```

---

## 23. Memory Model and Object Semantics
Python’s memory model explains how objects are identified, compared, mutated, copied, and eventually reclaimed by the runtime.
This topic covers mutability, aliasing, copy depth, equality versus identity, and reference-counting-based garbage collection.

### 23.1 Mutability
Mutability defines the semantics of Mutable objects, and Immutable objects, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Mutable objects
- Immutable objects

**Concrete example:**
```python
a = [1, 2]
a.append(3)    # mutable object changes in place

s = "ab"
s = s + "c"    # new string object created because strings are immutable
```

### 23.2 Identity and Equality
Identity and Equality defines the semantics of `id()`, `is`, and `==`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `id()`
- `is`
- `==`

**Concrete example:**
```python
a = [1, 2]
b = [1, 2]
c = a
print(a == b)   # same value
print(a is c)   # same object
print(id(a), id(c))
```

### 23.3 References and Aliasing
References and Aliasing defines the semantics of Shared references, and Side effects of mutation, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Shared references
- Side effects of mutation

**Concrete example:**
```python
items = [1, 2]
alias = items
alias.append(3)
print(items)    # mutation via alias affects original reference
```

### 23.4 Copy Semantics
Copy Semantics defines the semantics of Shallow copy, Deep copy, and `copy` module, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Shallow copy
- Deep copy
- `copy` module

**Concrete example:**
```python
import copy
nested = [[1], [2]]
shallow = copy.copy(nested)
deep = copy.deepcopy(nested)
nested[0].append(99)
print(shallow)   # inner list shared
print(deep)      # fully independent
```

### 23.5 Garbage Collection
Garbage Collection defines the semantics of Reference counting basics, and Cyclic garbage collection, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Reference counting basics
- Cyclic garbage collection

**Concrete example:**
```python
import gc

class Node:
    def __init__(self):
        self.ref = None

a = Node()
b = Node()
a.ref = b
b.ref = a     # cyclic reference
del a, b
gc.collect()  # cyclic garbage collector can reclaim unreachable cycles
```

---

## 24. Namespaces, Closures, and Scope Mechanics
Namespaces map names to objects, while scope rules determine where Python looks up a name during execution.
This topic covers LEGB lookup, closure formation, free-variable capture, name shadowing, and rebinding enclosing names with `nonlocal`.

### 24.1 Namespaces
Namespaces defines the semantics of Local namespace, Global namespace, and Built-in namespace, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Local namespace
- Global namespace
- Built-in namespace

**Concrete example:**
```python
x = "global"

def demo():
    y = "local"
    print(locals())
print(globals()["x"])
demo()
```

### 24.2 Scope Resolution
Scope Resolution defines the semantics of LEGB lookup, and Name shadowing, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- LEGB lookup
- Name shadowing

**Concrete example:**
```python
value = "global"

def outer():
    value = "enclosing"
    def inner():
        print(value)   # LEGB finds enclosing scope
    inner()

outer()
```

### 24.3 Closures
Closures defines the semantics of Inner functions, Captured variables, and Free variables, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Inner functions
- Captured variables
- Free variables

**Concrete example:**
```python
def make_multiplier(n):
    def multiply(x):
        return x * n   # captured free variable n
    return multiply

times3 = make_multiplier(3)
print(times3(5))
```

### 24.4 `nonlocal` and Closure Mutation
`nonlocal` and Closure Mutation defines the semantics of Rebinding enclosing names, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Rebinding enclosing names

**Concrete example:**
```python
def counter():
    total = 0
    def inc():
        nonlocal total
        total += 1
        return total
    return inc

c = counter()
print(c(), c(), c())
```

---

## 25. Descriptors and Metaclasses
Descriptors customize attribute access by injecting logic into get, set, and delete operations during attribute lookup.
Metaclasses customize class creation itself, allowing advanced control over class namespaces, validation, and type construction.

### 25.1 Descriptor Protocol
Descriptor Protocol defines the semantics of `__get__`, `__set__`, and `__delete__`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `__get__`
- `__set__`
- `__delete__`

**Concrete example:**
```python
class Positive:
    def __get__(self, obj, objtype=None):
        return obj._value
    def __set__(self, obj, value):
        if value < 0:
            raise ValueError("must be non-negative")
        obj._value = value
    def __delete__(self, obj):
        del obj._value

class Score:
    value = Positive()
    def __init__(self, value):
        self.value = value
```

### 25.2 Descriptor Use Cases
Descriptor Use Cases defines the semantics of Managed attributes, Validation logic, and Computed access, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Managed attributes
- Validation logic
- Computed access

**Concrete example:**
```python
class UpperText:
    def __get__(self, obj, objtype=None):
        return obj._text.upper()
    def __set__(self, obj, value):
        obj._text = value.strip()

class Message:
    text = UpperText()
    def __init__(self, text):
        self.text = text

m = Message(" hello ")
print(m.text)
```

### 25.3 Metaclass Basics
Metaclass Basics defines the semantics of Class creation process, `type`, and Custom metaclasses, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Class creation process
- `type`
- Custom metaclasses

**Concrete example:**
```python
class UpperAttrMeta(type):
    def __new__(mcls, name, bases, namespace):
        new_ns = {k.upper() if not k.startswith("__") else k: v
                  for k, v in namespace.items()}
        return super().__new__(mcls, name, bases, new_ns)

class Demo(metaclass=UpperAttrMeta):
    value = 10

print(Demo.VALUE)
```

---

## 26. Standard Library Core Utilities
The standard library provides foundational modules for data structures, iteration, functional programming, date-time work, serialization, math, randomness, statistics, and logging.
This topic surveys common high-value utilities that support idiomatic Python solutions without external dependencies.

### 26.1 `collections`
`collections` defines the semantics of `Counter`, `defaultdict`, `namedtuple`, `deque`, and `OrderedDict`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `Counter`
- `defaultdict`
- `namedtuple`
- `deque`
- `OrderedDict`

**Concrete example:**
```python
from collections import Counter, defaultdict, namedtuple, deque, OrderedDict

print(Counter("banana"))
dd = defaultdict(int)
dd["x"] += 1
Point = namedtuple("Point", ["x", "y"])
q = deque([1, 2, 3])
od = OrderedDict([("a", 1), ("b", 2)])
print(dd, Point(1, 2), q.popleft(), od)
```

### 26.2 `itertools`
`itertools` defines the semantics of `count()`, `cycle()`, `repeat()`, `chain()`, `product()`, `permutations()`, and `combinations()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `count()`
- `cycle()`
- `repeat()`
- `chain()`
- `product()`
- `permutations()`
- `combinations()`

**Concrete example:**
```python
from itertools import count, cycle, repeat, chain, product, permutations, combinations

print(next(count(10)))
print(next(cycle("AB")))
print(list(repeat("x", 3)))
print(list(chain([1, 2], [3, 4])))
print(list(product([1, 2], ["a", "b"])))
print(list(permutations([1, 2, 3], 2)))
print(list(combinations([1, 2, 3], 2)))
```

### 26.3 `functools`
`functools` defines the semantics of `reduce()`, `partial()`, and `lru_cache()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `reduce()`
- `partial()`
- `lru_cache()`

**Concrete example:**
```python
from functools import reduce, partial, lru_cache

print(reduce(lambda a, b: a + b, [1, 2, 3, 4]))
base2 = partial(int, base=2)
print(base2("1010"))

@lru_cache(maxsize=None)
def fib(n):
    return n if n < 2 else fib(n - 1) + fib(n - 2)

print(fib(10))
```

### 26.4 `datetime`
`datetime` defines the semantics of `date`, `time`, `datetime`, `timedelta`, `strftime()`, and `strptime()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `date`
- `time`
- `datetime`
- `timedelta`
- `strftime()`
- `strptime()`

**Concrete example:**
```python
from datetime import date, time, datetime, timedelta

today = date.today()
now = datetime.now()
later = now + timedelta(days=2)
print(today, time(14, 30), later.strftime("%Y-%m-%d"))
print(datetime.strptime("2025-01-01", "%Y-%m-%d"))
```

### 26.5 `json`
`json` defines the semantics of `dump()`, `dumps()`, `load()`, and `loads()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `dump()`
- `dumps()`
- `load()`
- `loads()`

**Concrete example:**
```python
import json

data = {"name": "Ana", "score": 95}
text = json.dumps(data)
restored = json.loads(text)
print(text, restored)
```

### 26.6 `math`
`math` defines the semantics of Numeric functions, and Constants, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Numeric functions
- Constants

**Concrete example:**
```python
import math
print(math.sqrt(16), math.ceil(2.3), math.pi, math.e)
```

### 26.7 `random`
`random` defines the semantics of Pseudorandom generation, and Sampling APIs, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Pseudorandom generation
- Sampling APIs

**Concrete example:**
```python
import random
print(random.randint(1, 10))
print(random.choice(["a", "b", "c"]))
print(random.sample(range(10), 3))
```

### 26.8 `statistics`
`statistics` defines the semantics of Mean, Median, and Mode, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Mean
- Median
- Mode

**Concrete example:**
```python
import statistics
data = [1, 2, 2, 3, 4]
print(statistics.mean(data))
print(statistics.median(data))
print(statistics.mode(data))
```

### 26.9 `logging`
`logging` defines the semantics of Loggers, Handlers, Formatters, and Levels, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Loggers
- Handlers
- Formatters
- Levels

**Concrete example:**
```python
import logging

logging.basicConfig(level=logging.INFO, format="%(levelname)s:%(message)s")
logger = logging.getLogger("demo")
logger.info("application started")
```

---

## 27. Filesystem and System Interaction
Python exposes operating-system and interpreter interfaces for path manipulation, directory traversal, environment access, process state, and file operations.
This topic covers the most commonly used APIs for interacting with the host system in a portable, programmatic way.

### 27.1 `os`
`os` defines the semantics of Directory operations, File operations, and Environment variables, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Directory operations
- File operations
- Environment variables

**Concrete example:**
```python
import os

print(os.getcwd())
print(os.listdir(".")[:3])
print(os.environ.get("HOME") or os.environ.get("USERPROFILE"))
```

### 27.2 `sys`
`sys` defines the semantics of `sys.argv`, Interpreter information, Exit control, and Path inspection, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `sys.argv`
- Interpreter information
- Exit control
- Path inspection

**Concrete example:**
```python
import sys

print(sys.argv)
print(sys.version)
print(sys.path[:2])
# sys.exit(1) would terminate the interpreter with exit status 1
```

### 27.3 `pathlib`
`pathlib` defines the semantics of `Path`, Path joining, Existence checks, and Iterating directories, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `Path`
- Path joining
- Existence checks
- Iterating directories

**Concrete example:**
```python
from pathlib import Path

p = Path("data") / "input.txt"
print(p.exists())
for child in Path(".").iterdir():
    print(child)
    break
```

### 27.4 `shutil`
`shutil` defines the semantics of Copying, Moving, and Removing directory trees, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Copying
- Moving
- Removing directory trees

**Concrete example:**
```python
import shutil

# shutil.copy("a.txt", "b.txt")
# shutil.move("b.txt", "archive/b.txt")
# shutil.rmtree("old_folder")
```

---

## 28. Serialization and Data Exchange
Serialization converts in-memory Python objects into storable or transferable representations and reconstructs them later.
This topic focuses on JSON for interoperable text exchange and pickle for Python-specific object persistence, including compatibility and safety concerns.

### 28.1 JSON Serialization
JSON Serialization defines the semantics of Python to JSON, JSON to Python, and Text-based interchange, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Python to JSON
- JSON to Python
- Text-based interchange

**Concrete example:**
```python
import json

payload = {"name": "Ava", "active": True}
json_text = json.dumps(payload)   # Python -> JSON text
restored = json.loads(json_text)  # JSON text -> Python
print(json_text, restored)
```

### 28.2 Pickle Serialization
Pickle Serialization defines the semantics of `pickle.dump()`, `pickle.dumps()`, `pickle.load()`, and `pickle.loads()`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `pickle.dump()`
- `pickle.dumps()`
- `pickle.load()`
- `pickle.loads()`

**Concrete example:**
```python
import pickle

data = {"numbers": [1, 2, 3]}
blob = pickle.dumps(data)
restored = pickle.loads(blob)
print(restored)
```

### 28.3 Serialization Concerns
Serialization Concerns defines the semantics of Type compatibility, and Security concerns with pickle, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Type compatibility
- Security concerns with pickle

**Concrete example:**
```python
# JSON is language-neutral but supports fewer Python-specific types.
# Pickle preserves more Python object structure but must never be loaded from untrusted sources.
```

---

## 29. Command-Line Programming
Command-line programs receive configuration and operands from the shell when a script or module is invoked with arguments.
This topic covers raw argument access with `sys.argv` and structured parsing with `argparse`.

### 29.1 Command-Line Arguments
Command-Line Arguments defines the semantics of `sys.argv`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `sys.argv`

**Concrete example:**
```python
import sys
print(sys.argv)   # e.g. ['app.py', '--mode', 'fast']
```

### 29.2 `argparse`
`argparse` defines the semantics of Positional arguments, Optional arguments, Flags, Type conversion, Default values, and Help text, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Positional arguments
- Optional arguments
- Flags
- Type conversion
- Default values
- Help text

**Concrete example:**
```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("filename")
parser.add_argument("--count", type=int, default=1)
parser.add_argument("--verbose", action="store_true")
# args = parser.parse_args()
```

---

## 30. Testing and Debugging
Testing and debugging techniques validate program behavior, surface faults, and allow inspection of runtime state during failure analysis.
This topic covers unit-test structure, debugger-driven inspection, and disciplined practices for checking edge cases and error paths.

### 30.1 `unittest`
`unittest` defines the semantics of Test cases, Assertions, Fixtures, and Test discovery, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Test cases
- Assertions
- Fixtures
- Test discovery

**Concrete example:**
```python
import unittest

class TestMath(unittest.TestCase):
    def setUp(self):
        self.value = 2
    def test_square(self):
        self.assertEqual(self.value ** 2, 4)

# unittest.main() enables test discovery when run as a script
```

### 30.2 Debugging Tools
Debugging Tools defines the semantics of `pdb`, Breakpoints, Stack inspection, and Variable inspection, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `pdb`
- Breakpoints
- Stack inspection
- Variable inspection

**Concrete example:**
```python
def divide(a, b):
    breakpoint()   # enters debugger
    return a / b
```

### 30.3 Basic Testing Practices
Basic Testing Practices defines the semantics of Input/output validation, Edge case testing, and Exception testing, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Input/output validation
- Edge case testing
- Exception testing

**Concrete example:**
```python
def parse_age(text):
    age = int(text)
    if age < 0:
        raise ValueError("age must be non-negative")
    return age

# Test normal input, edge cases like "0", and bad input like "-1" or "abc".
```

---

## 31. Virtual Environments and Package Management
Virtual environments isolate project dependencies from the system interpreter, enabling reproducible development and deployment setups.
This topic covers creating and activating environments, installing or removing packages, and pinning dependency versions.

### 31.1 Virtual Environments
Virtual Environments defines the semantics of `venv`, Environment creation, Activation, and Isolation, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `venv`
- Environment creation
- Activation
- Isolation

**Concrete example:**
```python
# python -m venv .venv
# .venv\Scripts\activate    # Windows
# source .venv/bin/activate   # macOS/Linux
# Packages installed inside the environment remain isolated from global Python.
```

### 31.2 Package Installation
Package Installation defines the semantics of `pip install`, `pip uninstall`, `pip list`, and `pip show`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `pip install`
- `pip uninstall`
- `pip list`
- `pip show`

**Concrete example:**
```python
# pip install requests
# pip uninstall requests
# pip list
# pip show requests
```

### 31.3 Dependency Management
Dependency Management defines the semantics of `requirements.txt`, and Version pinning, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `requirements.txt`
- Version pinning

**Concrete example:**
```python
# requirements.txt
# requests==2.32.3
# pandas==2.2.3
#
# Version pinning helps reproduce the same environment across systems.
```

---

## 32. Concurrency Basics
Python supports multiple concurrency models for overlapping work, including threads, processes, and asynchronous coroutines.
This topic covers execution-unit lifecycle, shared-state coordination, process-based parallelism, and event-loop-driven cooperative multitasking.

### 32.1 Threading
Threading defines the semantics of `threading`, Thread lifecycle, Locks, and Race conditions, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `threading`
- Thread lifecycle
- Locks
- Race conditions

**Concrete example:**
```python
import threading

lock = threading.Lock()
counter = 0

def worker():
    global counter
    with lock:
        counter += 1

threads = [threading.Thread(target=worker) for _ in range(5)]
for t in threads: t.start()
for t in threads: t.join()
print(counter)
```

### 32.2 Multiprocessing
Multiprocessing defines the semantics of `multiprocessing`, Process pools, and Inter-process data exchange basics, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `multiprocessing`
- Process pools
- Inter-process data exchange basics

**Concrete example:**
```python
from multiprocessing import Pool

def square(x):
    return x * x

with Pool(2) as pool:
    print(pool.map(square, [1, 2, 3, 4]))
```

### 32.3 Async Programming
Async Programming defines the semantics of `asyncio`, Coroutines, `async`, `await`, Event loop, and Tasks, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `asyncio`
- Coroutines
- `async`
- `await`
- Event loop
- Tasks

**Concrete example:**
```python
import asyncio

async def fetch_value():
    await asyncio.sleep(0.1)
    return 42

async def main():
    task = asyncio.create_task(fetch_value())
    print(await task)

asyncio.run(main())
```

---

## 33. Type Hints and Typing Basics
Type hints annotate code with expected value shapes and callable signatures without changing Python’s dynamic runtime behavior.
This topic covers function annotations, generic collection forms, optional and union types, and the role of static type checkers.

### 33.1 Function Type Hints
Function Type Hints defines the semantics of Parameter hints, and Return type hints, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Parameter hints
- Return type hints

**Concrete example:**
```python
def add(a: int, b: int) -> int:
    return a + b
```

### 33.2 Common Typing Forms
Common Typing Forms defines the semantics of `list[int]`, `dict[str, int]`, `tuple[...]`, and `set[...]`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `list[int]`
- `dict[str, int]`
- `tuple[...]`
- `set[...]`

**Concrete example:**
```python
values: list[int] = [1, 2, 3]
scores: dict[str, int] = {"math": 95}
pair: tuple[int, str] = (1, "a")
unique: set[str] = {"x", "y"}
```

### 33.3 Optional and Union Types
Optional and Union Types defines the semantics of `Optional`, and `Union`, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- `Optional`
- `Union`

**Concrete example:**
```python
from typing import Optional, Union

def parse(value: Optional[str]) -> Union[int, None]:
    return int(value) if value is not None else None
```

### 33.4 Static Typing Concepts
Static Typing Concepts defines the semantics of Runtime vs static typing, and Type checker role, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Runtime vs static typing
- Type checker role

**Concrete example:**
```python
# Runtime typing: Python still executes values dynamically.
# Static typing: tools like mypy check type hints before runtime to detect mismatches.
```

---

## 34. Pythonic Practices
Pythonic practices emphasize readability, directness, and idiomatic use of built-in language constructs over clever but opaque code.
This topic covers style guidance from PEP 8, common idioms, and habits that improve maintainability and express intent clearly.

### 34.1 PEP 8
PEP 8 defines the semantics of Naming conventions, Layout rules, and Readability, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Naming conventions
- Layout rules
- Readability

**Concrete example:**
```python
def calculate_total(price: float, tax: float) -> float:
    return price + tax
# Uses readable naming, spacing, and layout conventions recommended by PEP 8.
```

### 34.2 Common Idioms
Common Idioms defines the semantics of Iterable unpacking, Swapping values, Using `enumerate()`, Using `zip()`, and Truthy/falsy checks, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Iterable unpacking
- Swapping values
- Using `enumerate()`
- Using `zip()`
- Truthy/falsy checks

**Concrete example:**
```python
a, b = 1, 2
a, b = b, a                     # swapping
for index, value in enumerate(["x", "y"], start=1):
    print(index, value)
for left, right in zip([1, 2], ["a", "b"]):
    print(left, right)
name = ""
if not name:                    # truthy/falsy check
    print("empty")
```

### 34.3 Readability Practices
Readability Practices defines the semantics of Explicitness, Simple control flow, and Avoiding unnecessary complexity, including how Python parses these constructs or applies them during runtime evaluation and object interaction.
It specifies the operational rules developers rely on to predict correctness, side effects, lookup behavior, control flow, or API results in real programs.

- Explicitness
- Simple control flow
- Avoiding unnecessary complexity

**Concrete example:**
```python
# Prefer clear, explicit logic over compressed cleverness.
if user is None:
    raise ValueError("user is required")
```
