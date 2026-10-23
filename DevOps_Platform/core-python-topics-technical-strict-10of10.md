# Core Python: Technical Topics and Subtopics

## 1. Python Execution Model
### 1.1 Source Code Execution
- Source file structure
- Interactive mode
- Script mode
- Module execution
- Bytecode generation
- `__pycache__`

### 1.2 Runtime Model
- Interpreter process
- Evaluation model
- Statement execution
- Expression evaluation
- Object model overview

### 1.3 Program Entry
- `__name__`
- `__main__`
- `if __name__ == "__main__":`

---

## 2. Lexical Structure and Syntax
### 2.1 Tokens
- Keywords
- Identifiers
- Literals
- Operators
- Delimiters

### 2.2 Indentation and Blocks
- Indentation rules
- Block formation
- Nested blocks

### 2.3 Statements
- Simple statements
- Compound statements
- Assignment statements
- Expression statements

### 2.4 Comments and Documentation
- Single-line comments
- Docstrings
- Inline documentation

### 2.5 Line Structure
- Logical lines
- Physical lines
- Implicit line continuation
- Explicit line continuation

---

## 3. Variables, Names, and References
### 3.1 Name Binding
- Assignment
- Rebinding
- Multiple assignment
- Sequence unpacking
- Extended unpacking

### 3.2 Identifiers
- Naming rules
- Case sensitivity
- Reserved keywords

### 3.3 References
- Names bound to objects
- Reference semantics
- Aliasing

### 3.4 Deletion
- `del`
- Unbinding names
- Deleting container elements

---

## 4. Data Types and Built-in Objects
### 4.1 Numeric Types
- `int`
- `float`
- `complex`
- Numeric literals

### 4.2 Boolean Type
- `bool`
- Truth values
- Truth testing

### 4.3 None Type
- `None`
- Null-like semantics

### 4.4 Sequence Types
- `str`
- `list`
- `tuple`
- `range`

### 4.5 Set Types
- `set`
- `frozenset`

### 4.6 Mapping Type
- `dict`

### 4.7 Binary Types
- `bytes`
- `bytearray`
- `memoryview`

### 4.8 Type Introspection
- `type()`
- `isinstance()`
- `issubclass()`

### 4.9 Type Conversion
- Implicit conversion
- Explicit conversion
- Constructor-based casting

---

## 5. Operators and Expressions
### 5.1 Arithmetic Operators
- `+`
- `-`
- `*`
- `/`
- `//`
- `%`
- `**`

### 5.2 Comparison Operators
- `==`
- `!=`
- `<`
- `>`
- `<=`
- `>=`

### 5.3 Assignment Operators
- `=`
- `+=`
- `-=`
- `*=`
- `/=`
- `//=`
- `%=` 
- `**=`

### 5.4 Logical Operators
- `and`
- `or`
- `not`

### 5.5 Bitwise Operators
- `&`
- `|`
- `^`
- `~`
- `<<`
- `>>`

### 5.6 Membership Operators
- `in`
- `not in`

### 5.7 Identity Operators
- `is`
- `is not`

### 5.8 Conditional Expressions
- Ternary expression
- Short-circuit behavior

### 5.9 Operator Precedence
- Precedence rules
- Associativity
- Parenthesized expressions

---

## 6. Strings and Text Processing
### 6.1 String Fundamentals
- String creation
- Immutability
- Character indexing
- Slicing

### 6.2 Slicing Mechanics
- Start, stop, step
- Negative indices
- Negative step
- Slice copying

### 6.3 String Operations
- Concatenation
- Repetition
- Membership
- Lexicographic comparison

### 6.4 String Methods
- Case conversion
- Trimming
- Search methods
- Replace methods
- Split and join
- Prefix and suffix checks
- Alignment and padding

### 6.5 String Formatting
- f-strings
- `str.format()`
- `%` formatting
- Format specifiers

### 6.6 Unicode and Encoding
- Unicode code points
- UTF-8 basics
- `encode()`
- `decode()`

### 6.7 Raw and Multiline Strings
- Raw strings
- Triple-quoted strings

---

## 7. Lists
### 7.1 List Fundamentals
- List creation
- Indexing
- Slicing
- Mutability

### 7.2 List Modification
- Item assignment
- Slice assignment
- Insertions
- Deletions

### 7.3 List Methods
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

### 7.4 Sorting
- In-place sorting
- `sorted()`
- `key`
- `reverse`

### 7.5 Nested Lists
- Accessing nested elements
- Updating nested elements

### 7.6 List Comprehensions
- Basic comprehensions
- Conditional comprehensions
- Nested comprehensions
- Multiple `for` clauses

---

## 8. Tuples
### 8.1 Tuple Fundamentals
- Tuple creation
- Single-element tuples
- Indexing
- Slicing
- Immutability

### 8.2 Tuple Operations
- Concatenation
- Repetition
- Membership

### 8.3 Tuple Methods
- `count()`
- `index()`

### 8.4 Packing and Unpacking
- Tuple packing
- Tuple unpacking
- Extended unpacking
- Swapping values

---

## 9. Sets
### 9.1 Set Fundamentals
- Set creation
- Unordered collection semantics
- Uniqueness

### 9.2 Set Operations
- Union
- Intersection
- Difference
- Symmetric difference
- Subset and superset tests

### 9.3 Set Methods
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

### 9.4 Frozenset
- Immutable set semantics
- Hashability

---

## 10. Dictionaries
### 10.1 Dictionary Fundamentals
- Dictionary creation
- Key-value mapping
- Hashable keys
- Insertion-order behavior

### 10.2 Access and Modification
- Key lookup
- Item assignment
- Updating values
- Deleting items

### 10.3 Dictionary Methods
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

### 10.4 Dictionary Iteration
- Iterating keys
- Iterating values
- Iterating items

### 10.5 Dictionary Comprehensions
- Basic dictionary comprehensions
- Conditional dictionary comprehensions

### 10.6 Nested Dictionaries
- Access
- Update
- Traversal

---

## 11. Control Flow
### 11.1 Conditional Statements
- `if`
- `if-else`
- `if-elif-else`
- Nested conditionals

### 11.2 Looping
- `for`
- `while`
- Nested loops

### 11.3 Loop Control Statements
- `break`
- `continue`
- `pass`

### 11.4 Loop `else`
- `for-else`
- `while-else`

### 11.5 Structural Pattern Matching
- `match`
- `case`
- Literal patterns
- Sequence patterns
- Mapping patterns
- Guard clauses

---

## 12. Functions
### 12.1 Function Definition
- `def`
- Function body
- Return values
- Implicit `None`

### 12.2 Parameters and Arguments
- Positional arguments
- Keyword arguments
- Default arguments
- Variable-length arguments
- `*args`
- `**kwargs`

### 12.3 Advanced Parameter Kinds
- Positional-only parameters
- Keyword-only parameters
- Parameter ordering rules

### 12.4 Scope and Namespace Interaction
- Local scope
- Global scope
- Enclosing scope
- Built-in scope
- LEGB rule
- `global`
- `nonlocal`

### 12.5 First-Class Functions
- Functions as objects
- Passing functions
- Returning functions
- Assigning functions to names

### 12.6 Lambda Functions
- Anonymous functions
- Expression-only constraint

### 12.7 Recursion
- Recursive calls
- Base case
- Recursion depth considerations

### 12.8 Function Annotations
- Parameter annotations
- Return annotations
- `__annotations__`

### 12.9 Built-in Functional Tools
- `map()`
- `filter()`
- `zip()`
- `enumerate()`
- `sorted()`
- `any()`
- `all()`

### 12.10 Docstrings
- Function docstrings
- Access via `__doc__`

---

## 13. Comprehensions and Generator Expressions
### 13.1 List Comprehensions
- Single-expression form
- Conditional filtering
- Nested comprehensions

### 13.2 Set Comprehensions
- Unique-value generation

### 13.3 Dictionary Comprehensions
- Key-value generation
- Conditional generation

### 13.4 Generator Expressions
- Lazy evaluation
- Parenthesized generator syntax

### 13.5 Scope Behavior
- Comprehension variable scope
- Temporary iteration variables

---

## 14. Iterables, Iterators, and Generators
### 14.1 Iterable Protocol
- Iterable objects
- `iter()`

### 14.2 Iterator Protocol
- `__iter__()`
- `__next__()`
- `next()`
- `StopIteration`

### 14.3 Custom Iterators
- Iterator classes
- Stateful iteration

### 14.4 Generator Functions
- `yield`
- Generator state
- Resumable execution

### 14.5 Generator Methods
- `send()`
- `throw()`
- `close()`

### 14.6 Generator Use Cases
- Lazy computation
- Streaming data
- Memory efficiency

---

## 15. Modules and Packages
### 15.1 Modules
- Module files
- `import`
- `from ... import`
- `as`

### 15.2 Module Attributes
- `__name__`
- `__file__`
- `dir()`

### 15.3 Import System Basics
- Import resolution
- Module caching
- Re-import behavior

### 15.4 Standard Library Modules
- `math`
- `random`
- `datetime`
- `os`
- `sys`
- `pathlib`

### 15.5 User-Defined Modules
- Creating modules
- Organizing module code

### 15.6 Packages
- Package directories
- `__init__.py`
- Subpackages

### 15.7 Module Search Path
- `sys.path`
- Current working directory
- Installed locations

---

## 16. File Handling and I/O
### 16.1 File Operations
- Open
- Read
- Write
- Append
- Close

### 16.2 File Modes
- `r`
- `w`
- `a`
- `x`
- `b`
- `t`
- `+`

### 16.3 Reading APIs
- `read()`
- `readline()`
- `readlines()`
- Iterating over files

### 16.4 Writing APIs
- `write()`
- `writelines()`

### 16.5 File Positioning
- `tell()`
- `seek()`

### 16.6 Text vs Binary I/O
- Text streams
- Binary streams
- Encoding considerations

### 16.7 Context Management for Files
- `with open()`
- Automatic cleanup

---

## 17. Exception Handling
### 17.1 Exception Model
- Syntax errors vs exceptions
- Exception propagation
- Stack unwinding

### 17.2 Exception Blocks
- `try`
- `except`
- `else`
- `finally`

### 17.3 Exception Matching
- Specific exceptions
- Multiple exception types
- Exception hierarchy basics

### 17.4 Raising Exceptions
- `raise`
- Re-raising exceptions

### 17.5 Custom Exceptions
- Inheriting from `Exception`
- Domain-specific error classes

### 17.6 Assertions
- `assert`
- Assertion failure behavior

---

## 18. Object-Oriented Programming
### 18.1 Classes and Objects
- Class definition
- Object instantiation
- Attributes
- Methods

### 18.2 Constructors and Initialization
- `__new__`
- `__init__`

### 18.3 Attribute Access
- Instance attributes
- Class attributes
- Attribute lookup order

### 18.4 Method Types
- Instance methods
- Class methods
- Static methods

### 18.5 Encapsulation
- Public naming convention
- Protected naming convention
- Name mangling for private members

### 18.6 Inheritance
- Single inheritance
- Multiple inheritance
- Multilevel inheritance
- Hierarchical inheritance

### 18.7 Method Resolution
- MRO
- `super()`

### 18.8 Polymorphism
- Method overriding
- Duck typing

### 18.9 Abstraction
- Abstract base classes
- `abc` module
- `@abstractmethod`

### 18.10 Properties
- `@property`
- Setter methods
- Deleter methods

### 18.11 Dataclasses
- `@dataclass`
- Generated methods
- Field defaults

### 18.12 Enums
- `Enum`
- Named constants

---

## 19. Special Methods and Data Model
### 19.1 Object Representation
- `__str__`
- `__repr__`

### 19.2 Comparison Methods
- `__eq__`
- `__lt__`
- `__gt__`

### 19.3 Container Methods
- `__len__`
- `__getitem__`
- `__setitem__`
- `__contains__`

### 19.4 Callable and Context Behavior
- `__call__`
- `__enter__`
- `__exit__`

### 19.5 Iteration Hooks
- `__iter__`
- `__next__`

### 19.6 Arithmetic Operator Overloading
- `__add__`
- `__sub__`
- `__mul__`

---

## 20. Decorators
### 20.1 Function Decorators
- Wrapper functions
- Higher-order decorator pattern

### 20.2 Decorator Factories
- Decorators with arguments

### 20.3 Metadata Preservation
- `functools.wraps`

### 20.4 Stacked Decorators
- Multiple decorator order

### 20.5 Class and Method Decorators
- Class decorators
- Method decoration

### 20.6 Built-in Decorators
- `@staticmethod`
- `@classmethod`
- `@property`

---

## 21. Context Managers
### 21.1 Context Management Protocol
- `__enter__`
- `__exit__`

### 21.2 Using `with`
- Resource acquisition
- Guaranteed cleanup

### 21.3 Custom Context Managers
- Class-based context managers

### 21.4 `contextlib`
- `contextmanager`
- Utility helpers

---

## 22. Regular Expressions
### 22.1 Regex Syntax
- Literals
- Metacharacters
- Character classes
- Quantifiers
- Groups
- Alternation
- Anchors

### 22.2 `re` Module APIs
- `match()`
- `search()`
- `fullmatch()`
- `findall()`
- `finditer()`
- `sub()`
- `split()`
- `compile()`

### 22.3 Match Objects
- Matched spans
- Groups
- Group extraction

---

## 23. Memory Model and Object Semantics
### 23.1 Mutability
- Mutable objects
- Immutable objects

### 23.2 Identity and Equality
- `id()`
- `is`
- `==`

### 23.3 References and Aliasing
- Shared references
- Side effects of mutation

### 23.4 Copy Semantics
- Shallow copy
- Deep copy
- `copy` module

### 23.5 Garbage Collection
- Reference counting basics
- Cyclic garbage collection

---

## 24. Namespaces, Closures, and Scope Mechanics
### 24.1 Namespaces
- Local namespace
- Global namespace
- Built-in namespace

### 24.2 Scope Resolution
- LEGB lookup
- Name shadowing

### 24.3 Closures
- Inner functions
- Captured variables
- Free variables

### 24.4 `nonlocal` and Closure Mutation
- Rebinding enclosing names

---

## 25. Descriptors and Metaclasses
### 25.1 Descriptor Protocol
- `__get__`
- `__set__`
- `__delete__`

### 25.2 Descriptor Use Cases
- Managed attributes
- Validation logic
- Computed access

### 25.3 Metaclass Basics
- Class creation process
- `type`
- Custom metaclasses

---

## 26. Standard Library Core Utilities
### 26.1 `collections`
- `Counter`
- `defaultdict`
- `namedtuple`
- `deque`
- `OrderedDict`

### 26.2 `itertools`
- `count()`
- `cycle()`
- `repeat()`
- `chain()`
- `product()`
- `permutations()`
- `combinations()`

### 26.3 `functools`
- `reduce()`
- `partial()`
- `lru_cache()`

### 26.4 `datetime`
- `date`
- `time`
- `datetime`
- `timedelta`
- `strftime()`
- `strptime()`

### 26.5 `json`
- `dump()`
- `dumps()`
- `load()`
- `loads()`

### 26.6 `math`
- Numeric functions
- Constants

### 26.7 `random`
- Pseudorandom generation
- Sampling APIs

### 26.8 `statistics`
- Mean
- Median
- Mode

### 26.9 `logging`
- Loggers
- Handlers
- Formatters
- Levels

---

## 27. Filesystem and System Interaction
### 27.1 `os`
- Directory operations
- File operations
- Environment variables

### 27.2 `sys`
- `sys.argv`
- Interpreter information
- Exit control
- Path inspection

### 27.3 `pathlib`
- `Path`
- Path joining
- Existence checks
- Iterating directories

### 27.4 `shutil`
- Copying
- Moving
- Removing directory trees

---

## 28. Serialization and Data Exchange
### 28.1 JSON Serialization
- Python to JSON
- JSON to Python
- Text-based interchange

### 28.2 Pickle Serialization
- `pickle.dump()`
- `pickle.dumps()`
- `pickle.load()`
- `pickle.loads()`

### 28.3 Serialization Concerns
- Type compatibility
- Security concerns with pickle

---

## 29. Command-Line Programming
### 29.1 Command-Line Arguments
- `sys.argv`

### 29.2 `argparse`
- Positional arguments
- Optional arguments
- Flags
- Type conversion
- Default values
- Help text

---

## 30. Testing and Debugging
### 30.1 `unittest`
- Test cases
- Assertions
- Fixtures
- Test discovery

### 30.2 Debugging Tools
- `pdb`
- Breakpoints
- Stack inspection
- Variable inspection

### 30.3 Basic Testing Practices
- Input/output validation
- Edge case testing
- Exception testing

---

## 31. Virtual Environments and Package Management
### 31.1 Virtual Environments
- `venv`
- Environment creation
- Activation
- Isolation

### 31.2 Package Installation
- `pip install`
- `pip uninstall`
- `pip list`
- `pip show`

### 31.3 Dependency Management
- `requirements.txt`
- Version pinning

---

## 32. Concurrency Basics
### 32.1 Threading
- `threading`
- Thread lifecycle
- Locks
- Race conditions

### 32.2 Multiprocessing
- `multiprocessing`
- Process pools
- Inter-process data exchange basics

### 32.3 Async Programming
- `asyncio`
- Coroutines
- `async`
- `await`
- Event loop
- Tasks

---

## 33. Type Hints and Typing Basics
### 33.1 Function Type Hints
- Parameter hints
- Return type hints

### 33.2 Common Typing Forms
- `list[int]`
- `dict[str, int]`
- `tuple[...]`
- `set[...]`

### 33.3 Optional and Union Types
- `Optional`
- `Union`

### 33.4 Static Typing Concepts
- Runtime vs static typing
- Type checker role

---

## 34. Pythonic Practices
### 34.1 PEP 8
- Naming conventions
- Layout rules
- Readability

### 34.2 Common Idioms
- Iterable unpacking
- Swapping values
- Using `enumerate()`
- Using `zip()`
- Truthy/falsy checks

### 34.3 Readability Practices
- Explicitness
- Simple control flow
- Avoiding unnecessary complexity
