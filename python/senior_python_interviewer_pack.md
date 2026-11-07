# Core Python Interview Pack — Product-Based Companies
## Target: 8–12 Years Experience

> **Scope discipline:** this pack is **core Python only** — questions and answers, nothing else. No frameworks, no AWS/DevOps, no system design, no interview-process scaffolding. The DSA section (Section 32) is included because product-based companies always gate on it, but every problem there is chosen for the **Python** judgment it exposes, not as a generic LeetCode dump.

---

## Purpose

A core Python interview bank for hiring **8–12 year engineers at product-based companies**.

At this experience level you are not testing whether someone knows Python. You are testing:

- Whether their mental model of the runtime is **correct**, not merely functional
- Whether they can **debug code they did not write**, under time pressure
- Whether they make **defensible tradeoffs** instead of reciting best practices
- Whether their code would **survive review by a strong team**
- Whether they can tell *clever* from *maintainable* — and choose correctly
- Whether they can still **write working code**, which a surprising number of 10-year engineers cannot

---

## Calibration for 8–12 Years

The most important section in this pack. **Mis-calibration is the main reason senior Python interviews produce bad signal.**

### What changes at this level

| | 5–7 years | 8–12 years |
|---|---|---|
| Knowing *what* a decorator is | Signal | **Assumed** — not worth a question |
| Writing one correctly under pressure | Good signal | Baseline |
| Knowing when **not** to use one | Bonus | **The actual question** |
| Fixing a bug in given code | Strong signal | Baseline |
| Naming the *class* of bug and preventing it systemically | Bonus | **The actual question** |
| Solving a DSA problem | Signal | Baseline — speed matters less, clean code matters more |
| Optimizing when pushed | Good signal | **Expected unprompted** |

**The rule:** at 8–12 years, definition questions are a *screen*, not a *test*. Spend ~10 minutes confirming fundamentals, then spend the rest on code, judgment, and tradeoffs.

### Difficulty tags

| Tag | Meaning at 8–12 years |
|---|---|
| `[M]` | **Screening floor.** Answered instantly and completely, or it's a red flag. Calibrate fast and move on — never spend a round here. |
| `[S]` | **The real bar.** ~70% of the interview belongs here. A strong 8–12 year candidate clears nearly all of these. |
| `[St]` | **Differentiator.** Separates a strong hire from a Staff/Principal signal. Missing these is fine; nailing them with *judgment* (not trivia) is a level-up indicator. |

### Hard calibration rules

1. **Fumbling `[M]` ends the interview early.** At 10 years, not knowing why mutable defaults break is disqualifying. Don't spend 45 more minutes being polite.
2. **`[St]` trivia without `[S]` judgment is a no-hire.** Someone who explains metaclasses but writes untestable code with no error handling has read a lot and shipped little.
3. **"I don't know, but here's how I'd find out" is a 3, not a 1.** Calibrated uncertainty is a *positive* at this level. Confident bluffing on something they're wrong about is worse than admitting the gap.
4. **Weight *how they handle being wrong* heavily.** Push back on a correct answer and watch. Senior engineers defend with reasoning; weak ones collapse or dig in. This is the single best Staff-level signal in the pack.
5. **Library-author `[St]` topics are low priority here.** Descriptors, metaclasses, `__getstate__`, `ParamSpec` matter if the role builds internal frameworks. For a product team, judgment, debugging, and testability matter more — re-weight accordingly.

---

## Core 25

The highest-signal set for an 8–12 year core Python candidate. **Model answers for all 25 are in Appendix A.**

If you only have one round, ask from here.

| # | Question | Tag |
|---|---|---|
| 1 | What is the difference between `is` and `==`, and when does the distinction bite in real code? | `[M]` |
| 2 | Explain names vs objects in Python. What actually happens on `a = b`? | `[M]` |
| 3 | What is the difference between mutable and immutable objects, and why does it matter? | `[M]` |
| 4 | Why are mutable default arguments dangerous, and how do you fix it? | `[M]` |
| 5 | What is the difference between shallow copy and deep copy? When is deepcopy wrong? | `[S]` |
| 6 | What is a closure, and how does late binding cause bugs in loops? | `[S]` |
| 7 | How does the iterator protocol work? What's the difference between iterable and iterator? | `[M]` |
| 8 | What problem do generators solve, and what do they cost? | `[S]` |
| 9 | What does `yield from` give you beyond a loop? | `[S]` |
| 10 | How does a decorator work? Write one that takes arguments. | `[S]` |
| 11 | Why does `functools.wraps` matter, and what breaks without it? | `[S]` |
| 12 | What is a context manager, and what does the return value of `__exit__` control? | `[S]` |
| 13 | What is the difference between `__new__` and `__init__`? | `[S]` |
| 14 | How does method resolution order work, and what does `super()` actually do? | `[S]` |
| 15 | How do `__eq__` and `__hash__` relate? What makes an object hashable? | `[S]` |
| 16 | **What is EAFP vs LBYL? Which is Pythonic, and when is the other one correct?** | `[S]` |
| 17 | **What is the difference between `sort()` and `sorted()`? Why does `sort()` return `None`?** | `[M]` |
| 18 | What is the GIL? What does it prevent, and what does it *not* prevent? | `[S]` |
| 19 | How do you choose between threading, multiprocessing, and asyncio? | `[S]` |
| 20 | Why are blocking calls inside async code a problem, and how do you detect it? | `[S]` |
| 21 | How do imports execute, and what makes import-time side effects dangerous? | `[S]` |
| 22 | What are common causes of unexpected memory growth in Python? | `[S]` |
| 23 | How would you debug Python code that got slower after a refactor? | `[S]` |
| 24 | **Is `True` an `int`? What does that let you do, and where does it surprise people?** | `[M]` |
| 25 | How would you make this function testable? *(point at code they just wrote)* | `[S]` |

> **Rebalanced:** descriptors, `__getattr__`/`__getattribute__`, and `__slots__` were moved out of the Core 25 into the full bank (Sections 14 and 17). They are library-author topics — valuable `[St]` differentiators, but wrong for the must-ask list at a product company. Everyday idioms replaced them.

---

# Full Question Bank

Each concept has exactly one canonical home; the Core 25 above points into this bank rather than repeating it.

## 1) Python Data Model and Core Semantics

1. `[M]` What does it mean that everything in Python is an object?
2. `[S]` How would you explain Python's object model to an experienced engineer coming from Java or C++?
3. `[M]` What is the practical difference between a name and an object?
4. `[M]` What actually happens during assignment in Python?
5. `[S]` Why is Python described as "call by sharing" rather than by-value or by-reference? Where does that distinction become visible?
6. `[M]` What is the difference between identity and equality?
7. `[M]` When should `is` be used instead of `==`?
8. `[M]` Why is comparing to `None` done with `is None`?
9. `[St]` Why does `256 is 256` behave differently from `257 is 257` in some contexts, and why should you never rely on either?
10. `[M]` What makes something truthy or falsy?
11. `[S]` How do `__bool__` and `__len__` influence truthiness, and which wins?
12. `[S]` What are the practical consequences of Python being dynamically typed in a large codebase?
13. `[S]` What are the tradeoffs of Python's runtime flexibility — monkeypatching, attribute injection, dynamic imports?
14. `[S]` How does Python resolve attribute access on an object? Walk the full lookup order.
15. `[S]` What happens when an attribute is not found on an instance?
16. `[S]` What is the role of the class dictionary versus the instance dictionary?
17. `[St]` What is `__init_subclass__`, and what problem does it solve more cheaply than a metaclass?

## 2) Data Types, Mutability, and Built-in Collections

18. `[M]` What is the difference between mutable and immutable objects?
19. `[M]` Which built-in types are mutable, and which are immutable?
20. `[M]` Why does mutability matter in real code? Give a bug you've actually hit.
21. `[M]` Explain the difference between a tuple and a list beyond syntax.
22. `[S]` Can a tuple contain mutable objects? What are the consequences for hashing?
23. `[M]` When would you prefer a set over a list?
24. `[M]` When would you prefer a dict over a list of tuples?
25. `[S]` Why must dictionary keys be hashable?
26. `[S]` What are common bugs caused by mutating a collection while iterating over it? How do you do it safely?
27. `[S]` How do list, tuple, set, and dict differ in memory and operation cost?
28. `[M]` What is the difference between `remove`, `discard`, and `pop` across collections?
29. `[M]` How does slicing behave on lists versus strings? What does a slice return?
30. `[S]` What are the tradeoffs of `collections.deque` versus `list`?
31. `[M]` What are common use cases for `defaultdict`? When is it a trap?
32. `[M]` When is `Counter` better than a plain dict?
33. `[S]` What does dict insertion-order preservation guarantee in modern Python, and what does it *not* guarantee?
34. `[S]` When does aliasing create surprising behavior with mutable collections?
35. `[St]` Why is `list.insert(0, x)` in a loop a performance trap, and what do you use instead?
36. `[St]` What does `dict`'s resize behavior mean for code that builds very large dicts?

## 3) Functions and Argument Handling

37. `[M]` In what sense are Python functions first-class objects?
38. `[M]` What kinds of arguments can a Python function accept?
39. `[M]` What is the difference between positional and keyword arguments?
40. `[S]` What are positional-only arguments (`/`), and why would an API author want them?
41. `[S]` What are keyword-only arguments (`*`), and when do they improve an API?
42. `[M]` What do `*args` and `**kwargs` really do?
43. `[S]` What are common mistakes when forwarding `*args` and `**kwargs` through wrappers?
44. `[M]` How do default argument values work? When are they evaluated?
45. `[M]` Why are mutable default arguments a classic source of bugs, and what are the fixes?
46. `[S]` How do annotations differ from runtime enforcement?
47. `[S]` How would you design a clear function signature for long-term maintainability?
48. `[S]` What makes a Python API feel "Pythonic" at the function level?
49. `[S]` What are the tradeoffs between returning a tuple, a dict, and an object?
50. `[S]` When should a function raise versus return `None` or a sentinel? How do you design a sentinel properly?
51. `[M]` How do `lambda` expressions differ from `def` in practice, and what are their limits?
52. `[S]` How do higher-order functions show up in real Python codebases?
53. `[S]` What does `functools.partial` solve that a lambda does not?

## 4) Scope, Namespaces, and Closures

54. `[M]` What are Python namespaces?
55. `[M]` What is the LEGB rule?
56. `[M]` When do you need `global`? When do you need `nonlocal`?
57. `[S]` What is a closure, and how does it work internally?
58. `[S]` What is late binding, why does it confuse people in loops, and how do you fix it?
59. `[S]` How do closures compare with classes for holding state? When is each better?
60. `[S]` What are realistic production use cases for closures?
61. `[S]` How can shadowing a built-in create a subtle bug?
62. `[St]` What are cell variables, and what does `__closure__` contain?
63. `[St]` Why is "name binding" a better mental model than "variables store values"?

## 5) Comprehensions and Expressions

64. `[M]` What is the difference between a list comprehension and a generator expression?
65. `[M]` When is a comprehension clearer than a loop, and when is it worse?
66. `[S]` What are the readability tradeoffs of nested comprehensions?
67. `[M]` How do set and dict comprehensions differ from list comprehensions?
68. `[S]` Why can generator expressions be more memory efficient? When are they *not*?
69. `[S]` What happens when you try to consume a generator expression twice?
70. `[S]` How do comprehension scopes behave in Python 3, and how did that change from Python 2?
71. `[S]` Give a real example where a comprehension hurt maintainability.
72. `[S]` When does the walrus operator `:=` genuinely improve a comprehension, and when is it showing off?

## 6) Iterables, Iterators, and Generators

73. `[M]` What is the difference between an iterable and an iterator?
74. `[M]` How does the iterator protocol work? Which methods define each role?
75. `[S]` Why is every iterator also an iterable, and why does that design matter?
76. `[M]` What do `iter()` and `next()` do? What exception signals exhaustion?
77. `[S]` How would you implement a custom iterator? When would you implement `__iter__` as a generator instead?
78. `[S]` What problem do generators solve well, and what do they cost?
79. `[S]` How does `yield` change function behavior? What is the difference between a generator function and a generator object?
80. `[S]` What is the difference between returning a list and yielding values, for the caller?
81. `[S]` How do generators help with streaming large data?
82. `[S]` What is `yield from` used for, beyond saving a loop?
83. `[St]` How do `send`, `throw`, and `close` work on generators, and when have you actually needed them?
84. `[S]` What are common pitfalls when generators have side effects?
85. `[S]` How would you explain lazy evaluation, and when should you prefer eager evaluation?
86. `[S]` Why can a generator make debugging harder than the equivalent list-building code?
87. `[St]` What happens to a generator's `finally` block if the generator is never exhausted?

## 7) Decorators

88. `[M]` What is a decorator, and how does the syntax map to ordinary function calls?
89. `[S]` Implement a simple function decorator from scratch.
90. `[S]` How do you write a decorator that accepts arguments? Why does that need an extra layer?
91. `[S]` Why is `functools.wraps` important, and what specifically breaks without it?
92. `[S]` What are practical production use cases for decorators?
93. `[S]` When do decorators make code harder to debug, and how do you mitigate that?
94. `[S]` How would you debug a stack of three decorators where behavior is wrong?
95. `[S]` How do you write a decorator that works on both functions and methods?
96. `[St]` How do class decorators differ from metaclasses in purpose?
97. `[St]` How would you write a decorator that preserves the wrapped function's signature for introspection?

## 8) Context Managers

98. `[M]` What is a context manager, and what problem does `with` solve?
99. `[S]` How do `__enter__` and `__exit__` work? What arguments does `__exit__` receive?
100. `[S]` What does the return value of `__exit__` control?
101. `[S]` Implement a class-based context manager. Now implement the same thing with `contextlib.contextmanager`.
102. `[S]` When is `contextlib.contextmanager` preferable, and when is a class better?
103. `[S]` What are real-world uses beyond file handling?
104. `[S]` What are common mistakes when writing custom context managers?
105. `[St]` What does `contextlib.ExitStack` solve?
106. `[St]` How does an async context manager differ, and when do you need one?

## 9) Exceptions and Error Handling

107. `[M]` How does exception handling work in Python?
108. `[M]` Why is catching broad exceptions dangerous? When is it acceptable?
109. `[S]` What is the role of `else` in a `try` block? What is the role of `finally`?
110. `[S]` What happens if `finally` contains a `return`?
111. `[S]` How should custom exceptions be designed? When do you need a hierarchy?
112. `[S]` What is exception chaining, and what does `raise ... from ...` solve?
113. `[S]` When should code fail fast instead of handling errors locally?
114. `[S]` What are signs of poor exception design in a codebase?
115. `[S]` How do you make exception messages useful for debugging without leaking sensitive data?
116. `[S]` What's the difference between validation errors, usage errors, and programmer errors? Should all three be exceptions?
117. `[St]` What are exception groups and `except*`, and what problem do they solve?
118. `[St]` Why is `except BaseException` almost always wrong?

## 10) OOP in Python

119. `[M]` How is OOP in Python different from OOP in more rigid languages?
120. `[M]` What is the difference between class attributes and instance attributes?
121. `[S]` What happens when an instance attribute shadows a class attribute? Show the failure mode with a mutable class attribute.
122. `[M]` What is the difference between instance methods, class methods, and static methods?
123. `[S]` When is `@classmethod` the right design choice? When is `@staticmethod` justified versus just namespacing?
124. `[S]` What is the difference between `__new__` and `__init__`? When would you override `__new__`?
125. `[M]` How does inheritance work in Python?
126. `[S]` When is composition preferable to inheritance?
127. `[S]` How does `super()` work? What does zero-argument `super()` actually compile to?
128. `[S]` What does method resolution order solve, and what is C3 linearization doing?
129. `[S]` How does multiple inheritance work, and when is it risky?
130. `[S]` What are mixins, and how should they be used?
131. `[S]` What makes a Python class easy to extend and test?
132. `[St]` Why can `super()` behave surprisingly in a diamond hierarchy, and how do you make cooperative multiple inheritance work?

## 11) Dunder Methods and Pythonic Object Behavior

133. `[M]` What are dunder methods, and why are they central to the data model?
134. `[S]` What makes a good `__repr__`? How does it differ from `__str__`?
135. `[S]` What is the difference between `__len__` and `__bool__` for truthiness?
136. `[S]` When would you implement `__contains__`, and what happens if you don't?
137. `[S]` How do rich comparison methods work? What does Python do if `__lt__` returns `NotImplemented`?
138. `[S]` How would you design equality for a domain object?
139. `[S]` How should ordering be defined for a custom class? What does `functools.total_ordering` buy you?
140. `[S]` When should an object be immutable?
141. `[S]` What are the risks of implementing magic methods just because you can?
142. `[St]` How do `__call__`, `__getitem__`, and `__setitem__` let you build DSL-ish APIs, and when is that a mistake?

## 12) Copying, Equality, and Hashing

143. `[M]` What is the difference between shallow copy and deep copy?
144. `[S]` What bugs appear when a shallow copy is mistaken for an independent copy?
145. `[S]` When is deep copy unnecessary or actively harmful?
146. `[S]` How do `copy.copy` and `copy.deepcopy` differ? How do circular references affect deep copy?
147. `[S]` What is the relationship between equality and hashing? Why must equal objects have equal hashes?
148. `[S]` What happens if you override `__eq__` but not `__hash__`?
149. `[S]` Why is mutable state inside a hash dangerous? Demonstrate the broken-set failure.
150. `[St]` How do you make a class support `copy`/`deepcopy` correctly via `__copy__`/`__deepcopy__`?

## 13) Dataclasses, Named Tuples, and Lightweight Modeling

151. `[M]` When would you use a `dataclass`, and how does it compare to a regular class?
152. `[S]` What does `dataclass(frozen=True)` provide, and what are the tradeoffs of frozen data?
153. `[S]` Why does `dataclass` need `field(default_factory=...)` for mutable defaults?
154. `[S]` How does `eq=True, frozen=True` interact with `__hash__` generation?
155. `[M]` When is `namedtuple` still useful? How does `typing.NamedTuple` differ from a dataclass?
156. `[S]` When should a dict stay a dict instead of becoming a class?
157. `[S]` What are signs that a Python data model is over-engineered?
158. `[St]` When do you reach for `__post_init__`, and what are its pitfalls?

## 14) Properties, Descriptors, and Attribute Access

159. `[S]` What problem does `@property` solve? When is it better than a public attribute?
160. `[S]` When does excessive use of properties hurt clarity or performance?
161. `[St]` What is a descriptor? Why are functions defined on classes descriptors?
162. `[St]` How do descriptors relate to bound methods?
163. `[St]` How do data descriptors and non-data descriptors differ, and how does that change lookup priority?
164. `[St]` When should you use a descriptor instead of a property?
165. `[St]` How do `__getattr__` and `__getattribute__` differ? What are common bugs when overriding `__getattribute__`?
166. `[S]` How can custom attribute access make debugging difficult?
167. `[St]` What is `functools.cached_property`, and what are its thread-safety and invalidation caveats?

## 15) Imports, Modules, and Execution Model

168. `[M]` How does the import system work at a high level? What happens on first import?
169. `[S]` Why are imports cached, and what lives in `sys.modules`?
170. `[M]` What is the role of `__name__`, and why do people write `if __name__ == "__main__":`?
171. `[S]` What causes circular imports, and how do you break one cleanly?
172. `[S]` Why are import-time side effects dangerous? How do they hurt testing and startup time?
173. `[S]` What makes a module easy to import safely and test?
174. `[M]` What are the practical differences between absolute and relative imports?
175. `[M]` Why should wildcard imports usually be avoided?
176. `[St]` What is the difference between a module, a package, and a namespace package?
177. `[St]` How would you implement a lazy import, and when is it worth it?

## 16) Typing

178. `[M]` What problem do type hints solve? What are their limits at runtime?
179. `[S]` How should type hints be introduced into an existing codebase pragmatically?
180. `[S]` What is the difference between `list[int]` and `Sequence[int]` in an API signature?
181. `[S]` How do `Optional` and `None` interact in annotations? What is wrong with `def f(x: int = None)`?
182. `[S]` When can overly strict typing hurt readability? Which typing practices add real value versus noise?
183. `[St]` What is a `Protocol`, and when is structural typing better than a concrete type or ABC?
184. `[St]` What are `TypeVar` and `Generic`? Write a generic container signature.
185. `[St]` What is covariance versus contravariance, and where does it bite in practice?
186. `[St]` What does `ParamSpec` solve for decorator authors?
187. `[St]` What is the difference between `Any`, `object`, and `Never`?
188. `[St]` What does `from __future__ import annotations` change, and why does it matter for runtime introspection?
189. `[S]` What is `typing.TYPE_CHECKING` used for?

## 17) Memory Model and Performance

190. `[S]` What does reference counting do in CPython? What is the cyclic GC responsible for beyond it?
191. `[S]` What kinds of objects can participate in reference cycles?
192. `[S]` What are common reasons memory grows unexpectedly in a long-running Python service?
193. `[S]` How can lingering references keep objects alive? Name three real sources.
194. `[St]` What is `weakref`, and what problem does it solve? What is `WeakValueDictionary` for?
195. `[S]` What is interning, and why does it sometimes surprise people?
196. `[S]` What tradeoffs does `__slots__` introduce? When is it useful versus premature?
197. `[S]` Why can localizing repeated global lookups affect performance? Why is this usually the wrong place to start?
198. `[S]` Why is algorithmic improvement usually more important than micro-optimization?
199. `[S]` How do you reason about performance *before* changing code? What tools do you reach for?
200. `[S]` What are common anti-patterns in premature Python optimization?
201. `[St]` What are the GC generations, and when would you tune or disable the cyclic collector?
202. `[St]` What is `memoryview`, and what does it avoid?
203. `[St]` Why is string concatenation in a loop a trap, and what is the idiomatic fix?

## 18) Concurrency: GIL, Threads, Processes, Asyncio

204. `[M]` What is the GIL? What does it prevent, and what does it *not* prevent?
205. `[S]` Why can threads still be useful in Python despite the GIL?
206. `[S]` Why does multiprocessing help CPU-bound work, and what costs does it introduce?
207. `[S]` How would you explain concurrency versus parallelism in Python terms?
208. `[S]` How do you choose among threading, multiprocessing, and asyncio for a given task?
209. `[S]` What is `concurrent.futures`, and when do you use `ThreadPoolExecutor` versus `ProcessPoolExecutor`?
210. `[S]` What happens to exceptions raised inside a `Future`? When do you find out?
211. `[S]` What is a race condition? Give a Python example that the GIL does *not* protect you from.
212. `[S]` What is the difference between `Lock` and `RLock`? When do you need `RLock`?
213. `[S]` Why is `queue.Queue` usually a better coordination tool than shared state plus locks?
214. `[S]` What problem does `asyncio` solve particularly well, and when is it a worse fit than threads?
215. `[S]` What is the difference between cooperative concurrency and preemptive scheduling?
216. `[S]` Why are blocking calls inside async code problematic? How would you detect one in production?
217. `[S]` What is the difference between a coroutine, a task, and a future?
218. `[S]` What does `asyncio.gather` do, and how does it handle exceptions? How does `return_exceptions` change it?
219. `[St]` What is a `TaskGroup`, and how does it improve on `gather`?
220. `[St]` How does cancellation work in asyncio? What is the correct way to handle `CancelledError`?
221. `[St]` What is the "fire and forget task gets garbage collected" trap?
222. `[St]` What are `contextvars`, and why does thread-local state break under asyncio?
223. `[S]` What makes async code harder to debug?
224. `[S]` What are realistic misconceptions engineers have about the GIL?
225. `[St]` What is free-threaded CPython (PEP 703), and what would it change for code you have written?

## 19) Strings, Bytes, Encoding, and Formatting

226. `[M]` What is the difference between `str` and `bytes`? When does the boundary matter?
227. `[S]` What is encoding versus decoding? What causes `UnicodeDecodeError`, and how do you handle it properly?
228. `[S]` Why is string immutability a performance consideration?
229. `[M]` What are the tradeoffs between f-strings, `.format()`, `%`, and `str.join`?
230. `[S]` Why should log statements usually use `logger.info("x=%s", x)` instead of an f-string?
231. `[S]` What does `str.split()` with no argument do differently from `str.split(" ")`?
232. `[St]` What is a code point versus a character versus a grapheme? Where does `len()` mislead you?
233. `[St]` What does `sys.intern` do, and when would you use it?

## 20) Stdlib Fluency: itertools, functools, heapq, bisect, operator

234. `[S]` Which `itertools` functions do you actually use, and for what?
235. `[S]` What does `itertools.groupby` require of its input, and what is the classic bug?
236. `[S]` What does `itertools.chain` solve that `+` does not?
237. `[S]` What is `functools.lru_cache`? What are its hazards — mutable arguments, unbounded growth, instance methods?
238. `[S]` When is `functools.reduce` clearer than a loop, and when is it worse?
239. `[St]` What does `functools.singledispatch` solve, and how does it compare to `if isinstance` chains?
240. `[S]` How does `sorted(key=...)` work? Why is `key` better than `cmp`?
241. `[S]` What does sort stability guarantee, and how do you exploit it for multi-key sorts?
242. `[S]` When do you reach for `heapq` instead of sorting? How do you get the top-k efficiently?
243. `[S]` What does `bisect` do, and what is the precondition?
244. `[S]` What does `operator.itemgetter` / `attrgetter` buy you over a lambda?
245. `[St]` How would you implement a priority queue with stable tie-breaking?

## 21) ABCs, Protocols, and Metaclasses

246. `[S]` What is `abc.ABC`, and what does `@abstractmethod` actually enforce — and when?
247. `[S]` When do you want an ABC versus duck typing versus a `Protocol`?
248. `[S]` What do `collections.abc` types like `Mapping` and `Sequence` give you for free?
249. `[St]` What is a metaclass, and what is `type` doing when a class is created?
250. `[St]` Name a legitimate metaclass use case, and explain why it could not be a class decorator or `__init_subclass__`.
251. `[St]` What is `__slots__`'s interaction with inheritance and ABCs?

## 22) Serialization and Persistence

252. `[S]` What are the risks of `pickle`? Why is unpickling untrusted data a security problem?
253. `[S]` What can `pickle` serialize that JSON cannot, and vice versa?
254. `[St]` How do `__getstate__` and `__setstate__` work, and when have you needed them?
255. `[S]` How do you round-trip a `datetime`, a `Decimal`, or a custom object through JSON safely?
256. `[S]` What happens when you pickle an object whose class definition has since changed?
257. `[St]` Why do objects sent to a `ProcessPoolExecutor` need to be picklable, and what commonly isn't?

## 23) Numerics and Precision

258. `[M]` Why does `0.1 + 0.2 != 0.3`? How do you compare floats correctly?
259. `[S]` When should you use `Decimal` instead of `float`? What about money specifically?
260. `[S]` What does Python's arbitrary-precision `int` buy you, and what does it cost?
261. `[S]` What is the difference between `/`, `//`, and how does `//` behave with negative numbers?
262. `[St]` What does `math.isclose` do, and what are `rel_tol` and `abs_tol` for?

## 24) Testing and Testability

263. `[M]` How do you decide what to test in a Python codebase?
264. `[S]` What makes a function hard to test, and how do you refactor it?
265. `[S]` What is a pytest fixture, and what do the different scopes mean?
266. `[S]` What is `@pytest.mark.parametrize` for, and when does it hurt readability?
267. `[S]` What is the difference between a mock, a stub, a fake, and a spy? When do you use each?
268. `[S]` What does `unittest.mock.patch` patch, and why does "patch where it's used, not where it's defined" matter?
269. `[S]` When is mocking a design smell rather than a tool?
270. `[S]` What is `monkeypatch` good for in pytest?
271. `[S]` How do you test code that depends on the current time, randomness, or the filesystem?
272. `[S]` How do you test a generator? How do you test an async function?
273. `[S]` What is dependency injection in a Python context, and what is the lightweight version of it?
274. `[St]` What does test coverage actually tell you, and what does it not?
275. `[St]` How would you write a test that catches a regression in memory usage or performance?

## 25) Modern Python (3.8 → 3.13)

276. `[S]` What is the walrus operator, and when does it genuinely help?
277. `[S]` What does structural pattern matching (`match`) give you beyond `if/elif`? When is it the wrong tool?
278. `[S]` What changed with `dict` union operators `|` and `|=`?
279. `[S]` What is `zoneinfo`, and why was it a significant addition?
280. `[S]` What do built-in generics (`list[int]` without `typing.List`) change?
281. `[St]` What are exception groups and `except*` for?
282. `[St]` What is `typing.Self`, and what did people do before it?
283. `[St]` What Python version features would you avoid in a library that must support older runtimes, and how do you manage that?

## 26) Logging, Packaging, and Environments

284. `[S]` How should logging be configured in a library versus an application?
285. `[S]` Why is `logging.basicConfig` in library code a problem?
286. `[S]` What is the difference between a logger, a handler, a filter, and a formatter?
287. `[S]` How do you add structured context to logs without threading it through every call?
288. `[M]` What is a virtual environment, and what problem does it solve?
289. `[S]` What is the difference between a lock file and a requirements file?
290. `[S]` What is the difference between `pyproject.toml` and `setup.py`?
291. `[St]` How do you debug a "works on my machine" dependency problem?

## 27) Debugging and Performance Investigation

292. `[S]` How do you debug a bug you cannot reproduce locally?
293. `[S]` What is your process for tracking down an unexpected `None`?
294. `[S]` How would you investigate a function that got slower after a refactor?
295. `[S]` What are common causes of accidental quadratic behavior in Python?
296. `[S]` How do you debug behavior caused by mutability and aliasing?
297. `[S]` How would you isolate a bug caused by shared state across threads?
298. `[S]` How do you read a Python traceback well? What does the chained "During handling of the above exception" section tell you?
299. `[S]` How do you use logging effectively without cluttering code?
300. `[S]` When is a debugger more appropriate than print statements? What do you use — `pdb`, `breakpoint()`, IDE?
301. `[S]` How would you reason about a memory leak in Python terms? What tools?
302. `[S]` What is the difference between `cProfile`, a sampling profiler, and `timeit`? When do you use each?
303. `[St]` How do you profile an async application, and why is it harder?
304. `[St]` How would you find which line is allocating memory using `tracemalloc`?

## 28) Design Judgment and Code Review

305. `[S]` What does a good Python refactor improve first: readability, speed, or abstraction?
306. `[S]` How do you identify over-clever Python during review?
307. `[S]` When is "Pythonic" a real argument, and when is it a rhetorical one?
308. `[S]` How do you decide whether something belongs in a function, a class, or a module?
309. `[S]` What is your rule for when to add an abstraction?
310. `[S]` How would you review a PR that is technically correct but hard to maintain?
311. `[S]` What is the most useful code review comment you've ever received or given?
312. `[St]` How do you design a Python library API that can evolve without breaking users?
313. `[St]` How do you handle deprecation in a Python codebase?

## 29) Everyday Python Idioms and Gotchas

> **The highest-frequency material in real product-company interviews.** These are the things a reviewer notices daily. A candidate strong on descriptors but weak here is a worse hire than the reverse.

### EAFP, LBYL, and duck typing

314. `[S]` What are EAFP and LBYL? Which is considered Pythonic, and why?
315. `[S]` When is LBYL actually the correct choice over EAFP?
316. `[S]` Why is `if key in d: return d[key]` worse than `try: return d[key]` in concurrent or I/O-backed code?
317. `[M]` What is duck typing, and how does it change how you write function parameters?
318. `[S]` When does EAFP become an anti-pattern?

### Truthiness, booleans, and comparison

319. `[M]` Is `True` an instance of `int`? What does `sum([True, True, False])` return, and where is that useful?
320. `[S]` Where does `bool` being an `int` subclass cause real bugs?
321. `[M]` What does `1 < x < 10` actually evaluate to? How does chained comparison differ from `(1 < x) and (x < 10)`?
322. `[S]` What does `a == b == c` do, and why can it surprise people?
323. `[M]` Why is `if x:` different from `if x is not None:`? When does that distinction cause a bug?
324. `[M]` What do `any()` and `all()` return for an empty iterable, and why?
325. `[S]` Do `any()` and `all()` short-circuit? How would you verify that?

### Sorting and sequence operations

326. `[M]` What is the difference between `list.sort()` and `sorted()`? Why does `sort()` return `None`?
327. `[S]` What are the other common "returns `None` because it mutates in place" methods, and what is the convention behind that?
328. `[M]` What does `reversed()` return, and how does it differ from `[::-1]`?
329. `[S]` What does slicing copy, and when does that matter for performance?

### Unpacking and iteration helpers

330. `[M]` What does `a, *rest = items` do? What about `first, *middle, last`?
331. `[S]` How do you use `*` and `**` to unpack into a function call, and how does that differ from `*args`/`**kwargs` in a definition?
332. `[M]` What does `enumerate()` give you, and what is its `start` parameter for?
333. `[M]` What does `zip()` do when the inputs are different lengths? What bug does that silently cause?
334. `[S]` What is `zip(strict=True)`, and why was it added?
335. `[S]` How do you transpose a matrix or unzip pairs using `zip(*rows)`?
336. `[S]` What does `itertools.pairwise` do, and what did you write before it existed?

### Loops, control flow, and assertions

337. `[S]` What does the `else` clause on a `for` or `while` loop do? When does it run?
338. `[S]` Give a legitimate use of `for...else`. Why do many style guides discourage it?
339. `[S]` What happens to `assert` statements when Python runs with `-O`? What does that mean for using them to validate input?
340. `[S]` Why should `assert` never be used for runtime validation in production code?

### Namespaces and introspection

341. `[S]` What is the difference between `globals()`, `locals()`, and `vars()`?
342. `[S]` Why is mutating the dict returned by `locals()` inside a function unreliable?
343. `[M]` What do `dir()` and `callable()` tell you, and when do you actually use them?

### String and modern-stdlib idioms

344. `[M]` What do `str.removeprefix` / `removesuffix` solve that `strip` does not? Why was `strip` a common bug source?
345. `[S]` What is the `=` specifier in an f-string (`f"{x=}"`) used for?

### Lifecycle and identity

346. `[S]` What is `__del__`, and why is relying on it for cleanup a mistake?
347. `[S]` Why can two objects have the same `id()` at different times in a program?
348. `[S]` How do you use multiple context managers in a single `with` statement, and what does the parenthesized form add?

---

---

# 30) Build Prompts

Short, self-contained tasks. Expect working code, then **always ask the three follow-ups**: *edge cases?*, *complexity?*, *how would you test it?*

At 8–12 years, a candidate should volunteer all three unprompted. Having to ask is itself a finding.

**Escalate with the scale ladder from Section 32** rather than moving to a new prompt — depth on one task beats breadth across three.

| # | Prompt | Tag | What you're really measuring |
|---|---|---|---|
| B1 | Remove duplicates from a list while preserving order. | `[M]` | Does the candidate reach for a set, or write O(n²)? |
| B2 | Group words by their starting character. | `[M]` | `defaultdict` / `setdefault` fluency |
| B3 | Count frequencies and return the top `k` items. | `[M]` | `Counter.most_common` vs `heapq.nlargest` vs full sort; ties |
| B4 | Write a generator that yields lines from multiple files lazily. | `[S]` | Laziness, resource cleanup, `yield from` |
| B5 | Implement a custom iterable yielding a sliding window over a sequence. | `[S]` | Iterator protocol, reusability, `deque(maxlen=)` |
| B6 | Write a decorator that logs calls and preserves metadata. | `[S]` | `functools.wraps`, `*args/**kwargs` forwarding |
| B7 | Write a context manager that measures execution time. | `[S]` | `__enter__`/`__exit__` or `@contextmanager`; exception-safety |
| B8 | Implement a retry decorator with configurable attempts and backoff. | `[S]` | Decorator-with-args, exception selection, jitter awareness |
| B9 | Implement a class with meaningful `__repr__`, `__eq__`, and `__hash__`. | `[S]` | The eq/hash contract; immutability of hashed fields |
| B10 | Implement an LRU cache without using `functools.lru_cache`. | `[St]` | `OrderedDict`/dict+linked list; eviction correctness |
| B11 | Write a function that flattens an arbitrarily nested iterable, without recursing into strings. | `[S]` | The classic `str` edge case |
| B12 | Implement a `chunked(iterable, n)` that works on any iterable, not just sequences. | `[S]` | Does it work on a generator? `islice` |
| B13 | Write an async function that fetches N URLs with a concurrency limit. | `[St]` | `gather` + `Semaphore`, error handling, cancellation |
| B14 | Implement a thread-safe counter and explain what makes it unsafe without the lock. | `[St]` | Knows `+= 1` is not atomic |

---

# 31) Snippet Bank

**These are the snippets v1 referred to but never provided.** Paste one into a shared editor and ask the candidate to predict the output *before* running it, then fix it.

Asking for a prediction first is the whole point — it separates people who have a model of Python from people who have memorized fixes.

---

### S1 — Mutable default argument `[M]`

```python
def add_item(item, basket=[]):
    basket.append(item)
    return basket

print(add_item("apple"))
print(add_item("banana"))
```

**Ask:** What prints? Why? Fix it.
**Output:** `['apple']` then `['apple', 'banana']`.
**Look for:** "the default is evaluated once, at function definition time, and stored on the function object" — bonus if they mention `add_item.__defaults__`.
**Fix:** `basket=None` + `if basket is None: basket = []`.
**Probe:** *"Is there ever a case where you'd want the shared default?"* (Yes — intentional memo caches, but it should be explicit.)

---

### S2 — Late-binding closure `[S]`

```python
fns = [lambda: i for i in range(3)]
print([f() for f in fns])
```

**Ask:** What prints? Now make it print `[0, 1, 2]` in three different ways and rank them.
**Output:** `[2, 2, 2]`.
**Look for:** closures capture the *variable*, not the value; `i` is looked up when the lambda is called.
**Fixes, best to worst:**
1. Factory function returning the closure — most explicit
2. `functools.partial(lambda x: x, i)` — explicit binding
3. `lambda i=i: i` — works, but abuses the default-argument mechanism and pollutes the signature

**Probe:** *"Does a generator expression behave the same way? What about a `for` loop that defines functions?"* (Yes, same issue.)

---

### S3 — Mutating while iterating `[M]`

```python
nums = [1, 2, 2, 3, 4]
for n in nums:
    if n % 2 == 0:
        nums.remove(n)
print(nums)
```

**Ask:** What prints? Why isn't it `[1, 3]`?
**Output:** `[1, 2, 3]` — a `2` survives.
**Look for:** list iteration is index-based; removing shifts elements left and the index skips one.
**Fix:** build a new list (`[n for n in nums if n % 2]`), or iterate over a copy (`for n in nums[:]`), or iterate in reverse.
**Probe:** *"What happens if you do this with a dict? With a set?"* (`RuntimeError: dictionary changed size during iteration` — louder failure, which is arguably better.)

---

### S4 — Aliasing in a "2D" list `[M]`

```python
grid = [[0] * 3] * 3
grid[0][0] = 1
print(grid)
```

**Ask:** What prints? Why?
**Output:** `[[1, 0, 0], [1, 0, 0], [1, 0, 0]]`.
**Look for:** the outer `* 3` replicated the *reference*, not the row. One list, three names for it.
**Fix:** `[[0] * 3 for _ in range(3)]`.
**Probe:** *"Why does the inner `[0] * 3` work fine?"* (Ints are immutable, so sharing is unobservable. The rule is: `*` on a list of mutables is a trap.)

---

### S5 — Shared mutable class attribute `[S]`

```python
class Cart:
    items = []

    def add(self, item):
        self.items.append(item)

a, b = Cart(), Cart()
a.add("apple")
print(b.items)
```

**Ask:** What prints? Fix it.
**Output:** `['apple']` — `b` sees `a`'s item.
**Look for:** `self.items.append(...)` never rebinds, so it mutates the *class* attribute. Contrast with `self.items = [...]`, which would create an instance attribute and shadow it.
**Fix:** assign in `__init__`, or use `dataclasses.field(default_factory=list)`.
**Probe:** *"When is a mutable class attribute actually correct?"* (Shared registries/caches — and it should be named to make that obvious.)

---

### S6 — Broken `__eq__` / `__hash__` contract `[S]`

```python
class Point:
    def __init__(self, x):
        self.x = x

    def __eq__(self, other):
        return isinstance(other, Point) and self.x == other.x

p = Point(1)
s = {p}
```

**Ask:** What happens on the last line?
**Output:** `TypeError: unhashable type: 'Point'` — defining `__eq__` sets `__hash__ = None`.
**Then show part two:**

```python
class Point:
    def __init__(self, x):
        self.x = x
    def __eq__(self, other):
        return isinstance(other, Point) and self.x == other.x
    def __hash__(self):
        return hash(self.x)

p = Point(1)
s = {p}
p.x = 2
print(p in s)        # ?
print(p in list(s))  # ?
```

**Output:** `False`, then `True`.
**Look for:** the object is now in the wrong hash bucket. Mutating a field that participates in `__hash__` corrupts every hash-based container holding it.
**Fix:** make hashed fields immutable — `frozen=True` dataclass, or hash only on a stable identity field.

---

### S7 — `super()` in multiple inheritance `[S]`

```python
class A:
    def ping(self):
        print("A")

class B(A):
    def ping(self):
        print("B")
        super().ping()

class C(A):
    def ping(self):
        print("C")
        super().ping()

class D(B, C):
    def ping(self):
        print("D")
        super().ping()

D().ping()
print([c.__name__ for c in D.__mro__])
```

**Ask:** Trace the call path.
**Output:** `D B C A`, then `['D', 'B', 'C', 'A', 'object']`.
**Look for:** `super()` is **not** "call my parent" — it's "call the next class in *this instance's* MRO". `B.ping`'s `super()` resolves to `C`, even though `B` doesn't inherit from `C`.
**Probe:** *"What breaks if `C.ping` doesn't call `super()`?"* (`A.ping` never runs — cooperative multiple inheritance requires everyone to cooperate.)
**Probe:** *"What happens if you write `class D(A, B)`?"* (`TypeError: Cannot create a consistent MRO`.)

---

### S8 — Where does the state live? `[S]`

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("below absolute zero")
        self._celsius = value

    @property
    def fahrenheit(self):
        return self._celsius * 9 / 5 + 32

t = Temperature(25)
print(t.fahrenheit)
print(t.__dict__)
```

**Ask:** Where is the state actually stored? What does `__dict__` show?
**Output:** `77.0`, then `{'_celsius': 25}`.
**Look for:** the assignment in `__init__` goes **through the setter**, so validation applies at construction too. `celsius` and `fahrenheit` exist on the class as descriptors, not in the instance dict. `fahrenheit` is derived and stores nothing.
**Probe:** *"How would you add a `fahrenheit` setter?"* *"What happens if someone sets `t._celsius = -500` directly?"* (Nothing stops them — properties are a convention, not enforcement.)

---

### S9 — When does a generator pipeline run? `[S]`

```python
def read(nums):
    for n in nums:
        print("read", n)
        yield n

def double(it):
    for n in it:
        print("double", n)
        yield n * 2

pipeline = double(read([1, 2, 3]))
print("pipeline built")
print(next(pipeline))
print("---")
print(list(pipeline))
```

**Ask:** Predict the full output.
**Output:**
```
pipeline built
read 1
double 1
2
---
read 2
double 2
read 3
double 3
[4, 6]
```
**Look for:** calling a generator function does zero work. Nothing runs until something pulls. The pipeline processes one item end-to-end at a time, not stage-by-stage — that's why it's memory-flat.
**Probe:** *"What's the memory profile versus building three lists?"* *"How would you debug this pipeline?"* (The laziness is exactly what makes stack traces confusing.)

---

### S10 — Async that still blocks `[S]`

```python
import asyncio, time
import requests

async def fetch(url):
    return requests.get(url).text      # (1)

async def process(data):
    time.sleep(0.5)                    # (2)
    return len(data)

async def main():
    results = await asyncio.gather(*(fetch(u) for u in urls))
    return [await process(r) for r in results]   # (3)
```

**Ask:** This "concurrent" code runs serially. Find every reason.
**Look for all three:**
1. `requests.get` is a blocking socket call — it holds the event loop. Needs `aiohttp`/`httpx.AsyncClient`, or `asyncio.to_thread`.
2. `time.sleep` blocks the loop. Needs `asyncio.sleep` — or if it's real CPU work, a process pool.
3. `[await process(r) for r in results]` awaits sequentially. Needs another `gather`.

**Probe:** *"How would you catch this in production?"* (`loop.set_debug(True)` warns on slow callbacks; `asyncio.get_event_loop().slow_callback_duration`.)
**Probe:** *"If `process` were genuinely CPU-bound, would async help at all?"* (No — wrong tool entirely.)

---

### S11 — Exception handling that hides bugs `[S]`

```python
import json

def load_config(path):
    try:
        with open(path) as f:
            data = json.load(f)
        return data["database"]["host"]
    except Exception:
        return "localhost"
```

**Ask:** What's wrong? Rewrite it.
**Look for:** this `except` swallows `FileNotFoundError`, `JSONDecodeError`, `KeyError`, `PermissionError`, `TypeError` — and typos in the code itself. A production outage becomes a silent fallback to `localhost`.
**Strong rewrite:** catch `FileNotFoundError` specifically if a default is genuinely intended; let malformed JSON and missing keys propagate (those are config bugs, not runtime conditions); log with context; `raise ... from e` if wrapping.
**Probe:** *"When is a bare fallback like this actually right?"* (When the default is a real product decision — and then it should log loudly, not silently.)

---

### S12 — `finally` swallowing `[S]`

```python
def f():
    try:
        return "try"
    finally:
        return "finally"

def g():
    try:
        raise ValueError("boom")
    finally:
        return "swallowed"

print(f())
print(g())
```

**Ask:** What prints?
**Output:** `finally`, then `swallowed` — and the `ValueError` vanishes entirely.
**Look for:** a `return` (or `break`) in `finally` discards both the pending return value and any in-flight exception. This is a genuine silent-data-loss pattern.
**Probe:** *"What should `finally` be used for?"* (Cleanup only. Never control flow.)
**Bonus signal:** on Python 3.14+ this emits `SyntaxWarning: 'return' in a 'finally' block` (PEP 765). A candidate who mentions the language now warns about it is tracking Python releases.

---

### S13 — Accidental quadratic `[S]`

```python
def dedupe(items):
    out = []
    for item in items:
        if item not in out:
            out.append(item)
    return out
```

**Ask:** This is fine for 100 items and times out at 100,000. Why?
**Look for:** `item not in out` is O(n) on a list, making the whole thing O(n²).
**Fix:** track membership in a `set` alongside the output list (preserving order), or use `dict.fromkeys(items)` which preserves insertion order in modern Python.
**Probe:** *"What does the set version require of the items?"* (Hashability — so the list version is the fallback for unhashable items, and you should say so in a comment.)
**Probe:** *"Name two other accidental-quadratic patterns you've seen."* (String `+=` in a loop; `list.pop(0)`; repeated `list.insert(0, ...)`; nested `in` over lists.)

---

### S14 — Decorator losing identity `[S]`

```python
import functools, inspect

def log_calls(fn):
    def wrapper(*args, **kwargs):
        print(f"calling {fn.__name__}")
        return fn(*args, **kwargs)
    return wrapper

@log_calls
def compute(x, y=2):
    """Multiply two numbers."""
    return x * y

print(compute.__name__)
print(compute.__doc__)
print(inspect.signature(compute))
```

**Ask:** What prints? What breaks downstream?
**Output:** `wrapper`, `None`, `(*args, **kwargs)`.
**Look for:** `functools.wraps(fn)` on `wrapper` fixes name, doc, `__module__`, `__qualname__`, `__dict__`, and sets `__wrapped__`.
**What actually breaks in production:** Sphinx docs, pytest fixture resolution, `click`/`FastAPI` signature introspection, pickling, and debugger stack readability.
**Probe:** *"Even with `wraps`, what's still wrong with the signature?"* (`inspect.signature` follows `__wrapped__` so it recovers — but static type checkers need `ParamSpec`.)

---

### S15 — `lru_cache` on a method `[St]`

```python
from functools import lru_cache

class Report:
    def __init__(self, rows):
        self.rows = rows

    @lru_cache(maxsize=None)
    def total(self):
        return sum(self.rows)
```

**Ask:** Two serious problems here. Find them.
**Look for:**
1. The cache is on the **class**, and `self` is part of the cache key — so every instance is held alive forever. This is a textbook memory leak in a long-running service.
2. The cache is keyed on `self`'s identity/equality, not on `self.rows` — mutating `rows` returns a stale result.

**Fix:** `functools.cached_property` for per-instance caching, or cache a module-level function keyed on an immutable value.
**Probe:** *"What if the class defines `__eq__` and `__hash__`?"* (Then two different objects can collide in the cache — worse.)
**Probe:** *"Is `cached_property` thread-safe?"* (Not for the computation; concurrent first-access can compute twice.)

---

### S16 — Circular import `[St]`

```python
# models.py
from services import send_email

class User:
    def register(self):
        send_email(self.email)

# services.py
from models import User

def send_email(addr): ...

def find_user(uid) -> User: ...
```

**Ask:** What happens on `import models`, and how do you fix it?
**Look for:** `ImportError: cannot import name 'User' from partially initialized module`. The candidate should explain that `sys.modules` gets a partially populated module object, so the second import sees an incomplete namespace — and that the error depends on *which module is imported first*, which is why it's so confusing.
**Fixes, ranked:**
1. Fix the layering — `models` should not depend on `services`. Invert it: register returns an event, the service layer sends.
2. Extract the shared piece into a third module both can import.
3. Import inside the function (defers to call time) — works, but it's a smell marker.
4. `if TYPE_CHECKING:` for annotation-only imports — the correct fix when the cycle exists *only* for typing.

**Probe:** *"Why does moving the import inside the function work?"* (By call time, both modules are fully initialized.)

---

### S17 — `zip` silently truncates `[M]`
```python
ids    = [1, 2, 3, 4]
names  = ["alice", "bob", "carol"]

records = dict(zip(ids, names))
print(records)
print(len(records), "of", len(ids))
```

**Ask:** What prints? What's the production bug here?
**Output:** `{1: 'alice', 2: 'bob', 3: 'carol'}` then `3 of 4`.
**Look for:** `zip` stops at the **shortest** input and says nothing. The fourth ID is silently dropped. This is a classic data-loss bug in ETL and config-parsing code — no exception, no warning, just missing rows.
**Fix:** `zip(ids, names, strict=True)` (Python 3.10+) raises `ValueError` on length mismatch. Pre-3.10, assert lengths explicitly or use `itertools.zip_longest` if padding is actually intended.
**Probe:** *"When is truncation the behavior you want?"* (Pairing an infinite or longer iterator with a finite one — `zip(itertools.count(), items)`. Then it's intentional and should be commented.)
**Probe:** *"What does `zip(*rows)` do?"* (Transposes — a good follow-on test of unpacking fluency.)

---

### S18 — `bool` is an `int` `[M]`

```python
print(isinstance(True, int))
print(True + True + False)
print(sum([True, False, True, True]))

counts = {1: "one", True: "yes"}
print(counts)

flags = [0, 1, True, False]
print(len(set(flags)))
```

**Ask:** Predict all five lines.
**Output:** `True`, `2`, `3`, `{1: 'yes'}`, `2`.
**Look for:** `bool` subclasses `int`; `True == 1` and `hash(True) == hash(1)`, so `True` **overwrites** the `1` key rather than adding a new one — and the surviving key is still `1`, only the value changed. Same reason the set collapses to 2 elements.
**Useful side:** `sum(predicate(x) for x in items)` to count matches is idiomatic and should be recognized as a feature, not a hack.
**Probe:** *"Where does this bite?"* (A dict keyed on mixed `int`/`bool` config values; `Counter` merging `1` and `True`; a function that takes `flag: bool` and silently accepts `2`.)
**Probe:** *"How do you check for an actual boolean?"* (`isinstance(x, bool)` — and note `isinstance(x, int)` is `True` for bools, which breaks naive type dispatch.)

---

### S19 — `for...else` `[S]`

```python
def find_user(users, target):
    for u in users:
        if u == target:
            print("found")
            break
    else:
        print("not found")

find_user(["a", "b"], "b")
find_user(["a", "b"], "z")
find_user([], "z")
```

**Ask:** What prints, and when does the `else` run?
**Output:** `found`, `not found`, `not found`.
**Look for:** the `else` runs when the loop completes **without hitting `break`** — including when the iterable is empty. It is *not* "else if the loop body didn't run". The keyword is widely considered misnamed; `nobreak` would have been clearer.
**Probe:** *"Legitimate use?"* (Search-and-fallback without a sentinel flag; validating that no element violated a condition.)
**Probe:** *"Why do style guides discourage it?"* (Most readers misread it. If the team doesn't know the construct, a flag variable or extracting to a function with an early `return` is clearer — and a senior engineer should weigh readability over cleverness.)
**Probe:** *"Does `try...else` mean the same thing?"* (No — `try`'s `else` runs when **no exception** was raised. Same keyword, unrelated semantics.)

---

### S20 — `assert` disappears under `-O` `[S]`
```python
def withdraw(balance, amount):
    assert amount > 0, "amount must be positive"
    assert amount <= balance, "insufficient funds"
    return balance - amount

print(withdraw(100, 30))
print(withdraw(100, -500))
```

**Ask:** What does this print normally? What happens if the service runs under `python -O`?
**Output normally:** `70`, then `AssertionError: amount must be positive`.
**Under `-O`:** `70`, then **`600`** — both assertions are stripped at compile time, and the function happily *increases* the balance.
**Look for:** `-O` sets `__debug__ = False` and removes every `assert` from the bytecode. Assertions are a **development** tool for conditions that should be impossible, never a validation mechanism for untrusted input.
**Fix:** `if amount <= 0: raise ValueError(...)` for anything that validates real input.
**Probe:** *"So when is `assert` right?"* (Internal invariants, post-conditions, test code, and "this branch is unreachable" markers — things whose failure means a programmer bug, not bad input.)
**Probe:** *"What's the security angle?"* (Auth and permission checks written as asserts vanish in optimized mode — a real CVE pattern.)
**Probe:** *"What else does `-O` change?"* (Strips docstrings with `-OO`.) Ties back to Q339–340.

---

**Suggested 45-minute pairing:** one build prompt (B1/B6/B7) + two snippets from different competency rows. Never run two snippets that map to the same row — you'd be measuring the same thing twice.

---

# 32) The 50 DSA Questions

Every problem below is chosen for the **Python judgment it exposes**, not as a generic LeetCode dump. The *Python angle* column is what you are actually listening for — it keeps this section inside the pack's core-Python scope.

## The scale escalation ladder

Attach these follow-ups to **every** problem. They convert a medium LeetCode question into a senior-appropriate conversation.

| Rung | Follow-up question | What it tests |
|---|---|---|
| 1 | "Works on 1,000 items. What's the complexity?" | Baseline analysis |
| 2 | "Now 10 million. What breaks first?" | Constant factors, memory, GC pressure |
| 3 | "The input doesn't fit in memory — it's a stream." | Generators, chunking, `itertools` |
| 4 | "It's called 10,000 times a second. Now what?" | Caching, precomputation, amortization |
| 5 | "Make it concurrent." | **GIL awareness** — do they correctly say threads won't help CPU-bound work? |

Rung 5 is the single best question here for connecting DSA back to core Python. Most candidates reach for threads reflexively; a strong one says *"this is CPU-bound, so threads buy nothing under the GIL — I'd use a process pool, and only if the chunk size justifies the pickling cost."*

## Python-specific traps to listen for

These core-Python failures show up *inside* DSA answers. They are more diagnostic than the algorithm itself.

| Trap | What a strong candidate does |
|---|---|
| `x in some_list` inside a loop | Knows it's O(n); converts to `set` |
| Hand-rolled binary search | Uses `bisect`; knows hand-rolling is an off-by-one bug farm |
| `list.pop(0)` / `insert(0, x)` | Uses `collections.deque` |
| `s += char` in a loop | Builds a list, then `"".join()` |
| Deep recursion | Knows the ~1000-frame default limit; converts to iterative rather than raising the limit |
| `[[0]*n]*m` grid | Knows this aliases rows *(Snippet S4)* |
| Mutable default in a recursive helper | Passes the accumulator explicitly *(Snippet S1)* |
| Slicing in a loop (`arr[i:]`) | Knows slicing copies — O(k) each time, silently quadratic |
| `sorted()` when a heap suffices | Uses `heapq.nlargest` for top-k: O(n log k), not O(n log n) |
| Worrying about integer overflow | Knows Python ints are arbitrary-precision, so `mid = (lo+hi)//2` is safe unlike in Java/C++ |
| Using a `list` as a dict key | Knows it's unhashable; converts to `tuple` or `frozenset` |
| Mutating a dict/set while iterating | Iterates over a copy or collects keys first |

## A. Hashing, Counting, and Dict/Set Mastery

> The highest-yield category for Python interviews. Most "optimize this" answers reduce to *use a dict or set*.

| # | Problem | Expected approach | Complexity | **Python angle** | Tag |
|---|---|---|---|---|---|
| D1 | **Two Sum** — indices of two numbers summing to target | One-pass dict of value→index | O(n) / O(n) | Opening move: do they sort (O(n log n), loses indices) or reach for a dict? `dict.get` vs `in` vs `try/except KeyError` — all valid, ask which and why | `[M]` |
| D2 | **Group Anagrams** | Dict keyed by `tuple(sorted(word))` or a 26-count tuple | O(n·k log k) / O(n·k) | **Why `tuple` and not `list`?** Hashability. The count-tuple variant drops the log k. `defaultdict(list)` fluency | `[M]` |
| D3 | **Top K Frequent Elements** | `Counter` + `heapq.nlargest`, or bucket sort | O(n log k) / O(n) | Three valid answers — `Counter.most_common(k)`, `heapq.nlargest`, bucket sort O(n). Ask them to compare. Knowing `most_common` sorts everything internally is a `[S]` signal | `[S]` |
| D4 | **First Unique Character in a String** | `Counter`, then scan in order | O(n) / O(1) alphabet | Relies on **dict insertion order** (guaranteed 3.7+). Do they know it's a language guarantee now, not an implementation detail? | `[M]` |
| D5 | **Subarray Sum Equals K** | Prefix sum + `defaultdict(int)` | O(n) / O(n) | The `defaultdict` trap: a plain *lookup* inserts a key. Matters if you later check `len()` or iterate. Also: why the `{0: 1}` seed | `[S]` |
| D6 | **Longest Consecutive Sequence** | Set + only start counting at sequence heads | O(n) / O(n) | Sorting is the instinct (O(n log n)); the set solution is O(n). The "only start if `num-1 not in s`" guard is what makes it linear — many candidates write an O(n²) set solution and *think* it's linear | `[S]` |
| D7 | **Valid Sudoku** | Sets of `(row, val)`, `(col, val)`, `(box, val)` tuples | O(1) fixed 81 | One pass with a single set of tuples is the elegant version. Set comprehension and tuple-key fluency | `[S]` |
| D8 | **Intersection / Union / Dedupe preserving order** | `set` ops; `dict.fromkeys` for ordered dedupe | O(n) / O(n) | `dict.fromkeys(x)` for order-preserving dedupe is the idiom. Also: what if elements are **unhashable**? Falls back to O(n²) — and they should say so | `[M]` |

---

## B. Two Pointers and Sliding Window

| # | Problem | Expected approach | Complexity | **Python angle** | Tag |
|---|---|---|---|---|---|
| D9 | **Longest Substring Without Repeating Characters** | Sliding window + dict of last-seen index | O(n) / O(min(n,charset)) | The canonical window problem. Watch for the `left = max(left, seen[c]+1)` subtlety — without `max`, stale indices move the window backwards | `[S]` |
| D10 | **Minimum Window Substring** | Window + `Counter` + a `missing` counter | O(n) / O(k) | `Counter` arithmetic (`-`, `&`) is elegant but **O(k) per op** — using it inside the loop is accidentally quadratic. Great trap | `[St]` |
| D11 | **Container With Most Water** | Two pointers from the ends | O(n) / O(1) | Tests whether they can *prove* the greedy move is safe. At 8–12 years, "it works" isn't enough — ask why moving the shorter wall can't lose the optimum | `[S]` |
| D12 | **3Sum** | Sort, then fix one + two pointers | O(n²) / O(1) | Dedup is the real test. Many reach for `set(tuple(sorted(t)))` — works, but costs memory and hides the logic. Skipping duplicates inline is the senior answer | `[S]` |
| D13 | **Trapping Rain Water** | Two pointers tracking left/right max | O(n) / O(1) | Ask for the prefix-array O(n) space version first, then push to O(1). Tests whether they can optimize space, not just time | `[St]` |
| D14 | **Longest Repeating Character Replacement** | Window + count of most-frequent char | O(n) / O(1) | The window never shrinks — subtle and worth probing. Why is not recomputing `maxCount` still correct? | `[St]` |
| D15 | **Max Consecutive Ones III** (flip at most k zeros) | Window with a zero counter | O(n) / O(1) | Clean template check. Can they write the generic sliding-window skeleton without bugs? | `[S]` |

---

## C. Sorting, Searching, and Binary Search

| # | Problem | Expected approach | Complexity | **Python angle** | Tag |
|---|---|---|---|---|---|
| D16 | **Sort records by multiple keys** (e.g. dept asc, salary desc, name asc) | `sorted(key=...)`, possibly multi-pass | O(n log n) | **Core Python gold.** Tests `key=` vs `cmp`, tuple keys, `operator.itemgetter`, negation for mixed directions, and **sort stability** for the multi-pass approach. Ask how they'd sort descending on a *string* field — negation doesn't work, so stability or `reverse=` per pass is required | `[S]` |
| D17 | **Merge Intervals** | Sort by start, then merge | O(n log n) / O(n) | `key=lambda x: x[0]` vs `itemgetter(0)`. Edge case: are touching intervals `[1,2],[2,3]` merged? Ask them to state the assumption rather than guess | `[M]` |
| D18 | **Binary Search** + find first/last occurrence | `bisect_left` / `bisect_right` | O(log n) / O(1) | **The key signal:** do they hand-roll it or use `bisect`? Hand-rolling is fine if correct, but a senior should know `bisect_left`/`bisect_right` and that `key=` was added in 3.10. Ask what the precondition is (sorted!) and what happens if it isn't | `[S]` |
| D19 | **Search in Rotated Sorted Array** | Modified binary search | O(log n) / O(1) | Can't use `bisect` here — tests real binary-search reasoning. Watch the duplicate-elements variant degrading to O(n) | `[S]` |
| D20 | **Find Peak Element** | Binary search on the slope | O(log n) / O(1) | Counter-intuitive application of binary search to unsorted input. Good "can you think, not just recall" problem | `[S]` |
| D21 | **Kth Largest Element** | `heapq.nlargest`, min-heap of size k, or quickselect | O(n log k) or O(n) avg | Three answers; ask them to compare. `heapq.nlargest(k, arr)[-1]` is the one-liner; a size-k min-heap is the streaming answer; quickselect is O(n) average but O(n²) worst. Senior answer names the tradeoff | `[S]` |
| D22 | **Largest Number** (arrange ints to form the largest number) | Custom comparator `a+b vs b+a` | O(n log n) | **Python 3 removed `cmp`.** Requires `functools.cmp_to_key` — or the clever `key=lambda x: x*10` trick. Excellent test of knowing *why* `key` replaced `cmp` and what you lose | `[St]` |

---

## D. Heaps and Priority Queues

| # | Problem | Expected approach | Complexity | **Python angle** | Tag |
|---|---|---|---|---|---|
| D23 | **Merge K Sorted Lists/Iterables** | Min-heap of k heads, or `heapq.merge` | O(n log k) / O(k) | `heapq.merge` exists and is lazy — knowing it is a strong stdlib signal. **Tie-breaking:** if two values are equal, Python compares the next tuple element, which may be a non-comparable object → `TypeError`. Fix: push `(val, index, item)` | `[S]` |
| D24 | **Task Scheduler** (cooldown between identical tasks) | Counter + max-heap, or direct math | O(n log n) or O(n) | **Python has no max-heap.** Watch for the `-count` negation idiom. The closed-form math solution is a `[St]` bonus | `[S]` |
| D25 | **Find Median from Data Stream** | Two heaps (max-heap low, min-heap high) | O(log n) add / O(1) find | The negation trick again, now on one side only — easy to get sign-confused. Tests careful implementation under mild complexity | `[St]` |
| D26 | **K Closest Points to Origin** | `heapq.nsmallest(k, pts, key=...)` | O(n log k) / O(k) | `nsmallest` with a `key` is the clean answer. Bonus: avoid `sqrt` — compare squared distances. Do they notice? | `[S]` |
| D27 | **Top K Frequent Words** (ties broken lexicographically) | Heap with composite key | O(n log k) | **Mixed sort directions**: count descending, word ascending. Can't just negate a string. Requires `(-count, word)` in a min-heap, or `cmp_to_key`. One of the best Python-specific sorting tests available | `[St]` |
| D28 | **Sliding Window Maximum** | Monotonic `deque` of indices | O(n) / O(k) | **`collections.deque` with O(1) `popleft`.** If they use a list and `pop(0)`, that's the trap firing. Heap solution is O(n log n) — acceptable but ask them to compare | `[St]` |

---

## E. Stacks, Queues, and Design

| # | Problem | Expected approach | Complexity | **Python angle** | Tag |
|---|---|---|---|---|---|
| D29 | **Valid Parentheses** | Stack + dict of pairs | O(n) / O(n) | Warm-up. Listen for the dict-mapping idiom instead of if/elif chains, and for the "stack must be empty at the end" check | `[M]` |
| D30 | **Min Stack** (O(1) `getMin`) | Stack of `(val, running_min)` tuples | O(1) all ops | Tuple-packing vs two parallel stacks. Ask about memory tradeoff, then about handling duplicates of the minimum | `[S]` |
| D31 | **Daily Temperatures** | Monotonic decreasing stack of indices | O(n) / O(n) | Many "for each element find the next greater" problems collapse to this. Tests pattern recognition over memorization | `[S]` |
| D32 | **Evaluate Reverse Polish Notation / Basic Calculator** | Stack | O(n) / O(n) | **Integer division trap:** Python's `//` floors toward negative infinity, but the problem usually wants truncation toward zero. `-7 // 2 == -4`, not `-3`. Requires `int(a/b)` or `math.trunc`. Superb core-Python catch | `[S]` |
| D33 | **LRU Cache** — O(1) get and put | `OrderedDict`, or dict + doubly-linked list | O(1) both | **The single most-asked design-y DSA question at product companies.** `OrderedDict.move_to_end()` + `popitem(last=False)` is the Python answer. Then ask them to do it *without* `OrderedDict` to prove they understand the DLL. Then: is it thread-safe? (No — `functools.lru_cache` is, for the GIL-atomic parts) | `[S]` |

---

## F. Linked Lists

> Lower value in Python than in C++/Java — Python has no built-in linked list and production Python rarely uses one. Included because product companies still ask. **Use sparingly at 8–12 years.**

| # | Problem | Expected approach | Complexity | **Python angle** | Tag |
|---|---|---|---|---|---|
| D34 | **Reverse a Linked List** (iterative + recursive) | Three-pointer walk | O(n) / O(1) | `prev, curr = curr, prev` tuple-unpacking is the Pythonic swap. For the recursive version: **what's the recursion depth limit?** ~1000 by default — so recursion is wrong for a long list, and they should say so | `[M]` |
| D35 | **Detect Cycle + find cycle start** | Floyd's tortoise/hare | O(n) / O(1) | Ask for the O(n)-space set-based version first — it's more Pythonic and often the right production answer. Floyd's is the O(1)-space optimization | `[S]` |
| D36 | **Merge Two Sorted Lists** | Dummy head + walk | O(n+m) / O(1) | The dummy-head idiom. Can they do it without special-casing the first node? | `[M]` |
| D37 | **Remove Nth Node From End** | Two pointers n apart | O(n) / O(1) | One-pass with a gap. Edge case: removing the head — the dummy node makes it uniform | `[S]` |

---

## G. Trees and Graphs

| # | Problem | Expected approach | Complexity | **Python angle** | Tag |
|---|---|---|---|---|---|
| D38 | **Binary Tree Level Order Traversal** | BFS with `collections.deque` | O(n) / O(w) | `deque` vs list again. Level separation via `len(queue)` snapshot at each level is the clean idiom | `[M]` |
| D39 | **Validate BST** | DFS carrying `(low, high)` bounds | O(n) / O(h) | **Classic wrong answer:** checking only `left < node < right` locally. Tests careful thinking. Bonus: in-order traversal must be strictly increasing — and a generator makes that elegant and early-exiting | `[S]` |
| D40 | **Lowest Common Ancestor** (BST and general tree) | Recursive descent | O(n) / O(h) | Ask both variants — BST is O(h) and much simpler. Do they notice the BST property lets them prune? | `[S]` |
| D41 | **Serialize and Deserialize Binary Tree** | Pre-order with null markers | O(n) / O(n) | Open-ended design. Watch for `iter()` + `next()` on the token stream — an elegant Python-specific deserialization. Probe: **why not `pickle`?** (Security, version coupling, interop) — ties straight back to Section 22 | `[St]` |
| D42 | **Number of Islands** | BFS or DFS over the grid | O(m·n) / O(m·n) | **Recursion depth:** a 1000×1000 grid blows the stack with DFS. A senior candidate flags this and uses an explicit stack or BFS. Also: mutate the grid in place vs keep a `visited` set — a real tradeoff worth discussing | `[S]` |
| D43 | **Course Schedule** (cycle detection / topological sort) | Kahn's BFS with in-degrees, or DFS with colors | O(V+E) / O(V+E) | `defaultdict(list)` for the adjacency list; `deque` for the queue. Ask them to also *return the order*, not just a bool | `[S]` |
| D44 | **Clone Graph** | DFS/BFS with a dict memo old→new | O(V+E) / O(V) | The memo dict keyed by **node object** — requires hashable nodes, which ties to `__hash__`/`__eq__` (Core 25 #15). Excellent bridge between DSA and core Python | `[S]` |
| D45 | **Word Ladder** | BFS over a wildcard-pattern adjacency map | O(n·k²) / O(n·k) | Naive adjacency building is O(n²); the `h*t` pattern bucket with `defaultdict(list)` makes it tractable. Bidirectional BFS is the `[St]` bonus | `[St]` |

---

## H. Intervals and Greedy

| # | Problem | Expected approach | Complexity | **Python angle** | Tag |
|---|---|---|---|---|---|
| D46 | **Meeting Rooms II** (min rooms needed) | Min-heap of end times, or sweep line | O(n log n) / O(n) | Two clean solutions. The sweep-line with sorted start/end events is often more Pythonic. Ask which they'd ship and why | `[S]` |
| D47 | **Non-overlapping Intervals / Insert Interval** | Sort by end; greedy | O(n log n) / O(1) | Greedy proof matters at this level — *why* sort by end rather than start? For Insert Interval, `bisect` can find the position | `[S]` |

---

## I. Dynamic Programming

> Keep DP light for core-Python rounds. Two or three is plenty — DP measures practice more than engineering judgment, and 8–12 year candidates are often rusty on it without that predicting job performance.

| # | Problem | Expected approach | Complexity | **Python angle** | Tag |
|---|---|---|---|---|---|
| D48 | **Climbing Stairs / House Robber** | 1-D DP, then roll to O(1) space | O(n) / O(1) | **The best `functools` question in the set.** Naive recursion is exponential; `@lru_cache` makes it linear with one line. Then ask: what are the costs? (Unbounded cache growth, recursion depth, `self` pinning if it's a method — Snippet S15.) Then convert to iterative | `[S]` |
| D49 | **Coin Change** (min coins) | Bottom-up 1-D DP | O(amount·coins) / O(amount) | Initializing with `float('inf')` vs `amount+1` as the sentinel. Why does the greedy approach fail here? Ask for a counterexample — tests whether they can disprove, not just recall | `[S]` |
| D50 | **Longest Increasing Subsequence** | O(n²) DP, then patience sorting with `bisect` | O(n log n) / O(n) | **The payoff problem.** The O(n log n) solution is essentially `bisect.bisect_left` in a loop. Critical nuance: the tails array is **not** the actual LIS — just its length. A candidate who claims otherwise hasn't understood it. Closes the loop on D18 | `[St]` |

---

---

# Appendix A — Model Answers for the Core 25

Each entry gives the **expected answer**, a **follow-up probe** to separate 3s from 4s, and **anchors** describing what weak and strong actually sound like.

These are calibration aids for the interviewer, not a key to grade against word-for-word. A candidate who reaches the same place by a different route is doing fine.

---

### 1. `is` vs `==` `[M]`

**Expected:** `==` calls `__eq__` and asks "are these equivalent in value?". `is` compares object identity — the same `id()`, the same object in memory. `is` never calls user code, so it can't be overridden and can't raise.

Use `is` only for singletons: `None`, `True`/`False`, `NotImplemented`, and sentinel objects you created. Use `==` for everything else.

The trap is that small ints and short strings are cached/interned by CPython, so `a is b` *appears* to work for values — and then silently fails at 257, or on a value read from a file, or on another interpreter.

**Probe:** *"Where does this actually bite in real code?"*
→ `if x is 0` passing in tests and failing on a parsed value; comparing strings with `is` after they stop being literals; `numpy` arrays where `==` returns an array and `is` is the only thing that doesn't explode in a boolean context.

| Weak (1–2) | Strong (3–4) |
|---|---|
| "`is` compares references, `==` compares values" and stops | Explains `__eq__` dispatch, names the interning trap and *why* you can't rely on it, gives a real incident |

---

### 2. Names vs objects — what happens on `a = b` `[M]`

**Expected:** Python variables are **names bound to objects**, not boxes holding values. `a = b` makes `a` refer to the same object `b` refers to. Nothing is copied.

That single rule explains: why passing a list to a function lets the function mutate your list, why `a = a + [1]` and `a += [1]` differ for lists, and why "pass by value vs reference" is the wrong frame — the term is **call by sharing**. The callee gets its own name bound to the caller's object. Rebinding the name is invisible to the caller; mutating the object is not.

**Probe:** *"So what does `+=` do on a list versus a tuple?"*
→ On a list, `+=` calls `__iadd__` and mutates in place, then rebinds to the same object. On a tuple there's no `__iadd__`, so it builds a new tuple and rebinds. This is why `t = ([],); t[0] += [1]` both raises `TypeError` **and** successfully mutates the list — a genuinely great 4-level question.

| Weak | Strong |
|---|---|
| "Python is pass by reference" | Uses "name", "binding", "object" precisely; derives mutation behavior from the model rather than reciting cases |

---

### 3. Mutable vs immutable `[M]`

**Expected:** Immutable objects cannot change state after creation: `int`, `float`, `str`, `bytes`, `tuple`, `frozenset`, `bool`, `None`. Mutable ones can: `list`, `dict`, `set`, `bytearray`, most user classes.

Why it matters practically:
- Immutables can be safely shared, cached, interned, and used as dict keys
- Mutables shared across scopes create action-at-a-distance bugs
- Hashability is tied to immutability of the hashed fields
- Immutables are safe to share across threads without locks

"Immutable" is shallow: a tuple is immutable, but a tuple *containing* a list can still have its contents change.

**Probe:** *"Is a tuple always hashable?"*
→ No. `hash((1, [2]))` raises, because hashing recurses into elements. This is the cleanest demonstration that immutability and hashability are related but not identical.

| Weak | Strong |
|---|---|
| Lists the types, can't say why it matters | Connects to thread safety, caching, dict keys; knows immutability is shallow |

---

### 4. Mutable default arguments `[M]` — *see Snippet S1*

**Expected:** Default values are evaluated **once**, when the `def` statement executes, and stored on the function object (`fn.__defaults__`). A mutable default is therefore shared across every call that doesn't supply the argument — it accumulates state.

Fix: `def f(x, items=None): items = [] if items is None else items`.

**Probe:** *"Is this always a bug?"*
→ No. It's a bug when unintentional. Deliberate uses exist (memo caches), and `functools.lru_cache` is the supported version. The problem is that the syntax makes the sharing invisible.

**Probe:** *"How would you catch this in review?"* → linters flag it (`B006` in flake8-bugbear, `W0102` in pylint, ruff's `B006`). A strong candidate mentions tooling.

| Weak | Strong |
|---|---|
| Knows the fix, explains it as "Python caches it" | Says "evaluated at definition time, stored on the function object", can point at `__defaults__`, mentions linting |

---

### 5. Shallow vs deep copy `[S]`

**Expected:** A shallow copy (`copy.copy`, `list(x)`, `x[:]`, `dict(x)`) creates a new outer container whose elements are the *same objects*. A deep copy (`copy.deepcopy`) recursively copies everything.

Shallow is right most of the time and is much cheaper. Deep copy is the correct answer surprisingly rarely.

When deep copy is **wrong**:
- The object graph is large → it's slow and memory-doubling
- The object holds non-copyable resources (sockets, file handles, DB connections, locks, thread handles)
- Identity matters — deep-copying a registry or a cache breaks the invariant that there's one of them
- Shared-by-design state gets silently forked, which is usually a worse bug than the aliasing you were avoiding

`deepcopy` does handle cycles correctly via a memo dict, so it won't infinite-loop — but it will happily copy half your application if an object has a back-reference to a parent.

**Probe:** *"You need an independent copy of a nested config dict. What do you actually do?"*
→ Best answer is often "don't copy — make it immutable" or "re-parse from source". Reaching for `deepcopy` as the first tool is a 2.

| Weak | Strong |
|---|---|
| Defines both, picks deepcopy "to be safe" | Treats deepcopy as a code smell; names the resource/identity/perf failure modes; prefers immutability |

---

### 6. Closures and late binding `[S]` — *see Snippet S2*

**Expected:** A closure is a function that captures names from an enclosing scope, keeping them alive after the enclosing function returns. CPython stores these in cell objects, reachable via `fn.__closure__`.

The key point: closures capture the **variable (cell)**, not the value at creation time. So a loop that creates functions referencing the loop variable gives every function the *final* value — late binding.

Fixes: a factory function (clearest), `functools.partial` (explicit), or a default argument `lambda i=i: i` (works, but overloads the signature).

**Probe:** *"When would you use a closure instead of a class?"*
→ One method and a little state → closure. More than one method, or state that should be inspectable/testable → class. A strong candidate notes that closures are harder to debug because the state isn't visible in a repr.

**Probe:** *"`nonlocal` vs `global`?"* → `nonlocal` rebinds in the nearest enclosing function scope; `global` rebinds at module level. Neither is needed to *read*, only to *rebind*.

| Weak | Strong |
|---|---|
| Defines closure, surprised by `[2,2,2]` | Predicts `[2,2,2]`, explains cells, ranks the three fixes by clarity not cleverness |

---

### 7. Iterable vs iterator `[M]`

**Expected:**
- **Iterable:** implements `__iter__`, which returns a fresh iterator. A list is iterable — you can loop over it many times.
- **Iterator:** implements `__next__` (and `__iter__` returning `self`). It is stateful and single-use; it raises `StopIteration` when exhausted.

Every iterator is an iterable (so `for` works uniformly on both), but not every iterable is an iterator. `iter(x)` gets an iterator; `next(it)` advances it.

`for x in y` is sugar for: call `iter(y)`, call `next()` repeatedly, stop on `StopIteration`.

**Probe:** *"Why does `__iter__` on an iterator return `self`?"*
→ So the uniform protocol works — `for` can accept either without the caller knowing which it got.

**Probe:** *"What goes wrong if you make your class's `__iter__` return `self` while holding the position on the instance?"*
→ You've made it single-use. Two nested loops over the same object interfere. This is *the* custom-iterator bug and a great 3-vs-4 separator.

| Weak | Strong |
|---|---|
| Uses the words interchangeably | Explains the single-use contract, knows the reusability bug, can desugar a `for` loop |

---

### 8. What generators solve, and what they cost `[S]` — *see Snippet S9*

**Expected:** Generators produce values lazily, one at a time, holding only current state instead of a full materialized result. They let you work with infinite or larger-than-memory sequences, express pipelines cleanly, and start producing output before all input is read.

The costs are real and candidates often skip them:
- **Single-use** — consuming it twice silently yields nothing
- **No `len()`, no indexing, no slicing**
- **Harder to debug** — a traceback shows the consumption site, not where the pipeline was built
- **Deferred errors** — an exception in a generator surfaces at the point of iteration, often far from the cause
- **Cleanup is not guaranteed** — if a generator is abandoned mid-iteration, its `finally` block runs only when the generator is finalized

**Probe:** *"When would you deliberately build a list instead?"*
→ Small data; needs multiple passes; needs length; wants errors to surface immediately; wants a readable repr while debugging.

| Weak | Strong |
|---|---|
| "Saves memory" and stops | Names the debuggability and single-use costs; knows when *not* to use one |

---

### 9. `yield from` `[S]`

**Expected:** At minimum it delegates iteration: `yield from sub` is roughly `for x in sub: yield x`. But it does more — it establishes a transparent two-way channel to the subgenerator, forwarding `send()`, `throw()`, and `close()`, and propagating the subgenerator's `return` value as the result of the `yield from` expression.

So for plain iteration it's a readability win. For coroutine-style generators it's doing real work that the manual loop does *not* replicate.

**Probe:** *"When is the loop and the `yield from` actually different?"*
→ Whenever the consumer calls `send`/`throw`, or whenever the subgenerator returns a value. A candidate who only knows the loop equivalence is a solid 3; one who knows the delegation semantics is a 4.

| Weak | Strong |
|---|---|
| "It's shorthand for a for loop" | Explains delegation of send/throw/close and the return-value capture |

---

### 10. Decorators, including with arguments `[S]`

**Expected:** A decorator is a callable that takes a function and returns a replacement. `@d` above `def f` is exactly `f = d(f)`, evaluated at definition time.

A decorator **with arguments** needs one more layer, because `@d(arg)` means `f = d(arg)(f)` — `d(arg)` must return the actual decorator:

```python
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
```

**Probe:** *"How would you let it be used both as `@retry` and `@retry(attempts=5)`?"*
→ Detect whether the first positional arg is callable. A 4 will note that this convenience makes the decorator harder to read and is often not worth it.

**Probe:** *"What are the costs of decorators?"* → an extra frame in every traceback, surprising behavior under introspection, and ordering dependence when stacked (bottom-up application, top-down execution).

| Weak | Strong |
|---|---|
| Can use decorators, can't write one with args | Writes the three-level version without prompting; explains why the extra layer exists; names the debuggability cost |

---

### 11. `functools.wraps` `[S]` — *see Snippet S14*

**Expected:** Without it, the wrapper function replaces the original's identity. `__name__` becomes `"wrapper"`, `__doc__` becomes `None`, `__qualname__`, `__module__`, and `__dict__` are all the wrapper's. `wraps` copies those across and sets `__wrapped__` pointing at the original.

What actually breaks in production: generated documentation; `inspect.signature` (which follows `__wrapped__`, so `wraps` fixes it); frameworks that introspect signatures for dependency injection or routing (FastAPI, click, pytest fixtures); pickling; and stack traces that all look identical.

**Probe:** *"Does `wraps` fix everything?"*
→ No. Static type checkers still see `(*args, **kwargs)` unless you use `ParamSpec`. And `wraps` copies metadata but doesn't make the wrapper behave like the original for `isinstance`/attribute checks beyond `__dict__` update.

| Weak | Strong |
|---|---|
| "It copies the docstring" | Names 3+ concrete downstream breakages; knows `__wrapped__`; mentions `ParamSpec` for typing |

---

### 12. Context managers and the `__exit__` return value `[S]`

**Expected:** A context manager guarantees setup and teardown around a block, including on exception, early `return`, and `break`. `__enter__` runs on entry and its return value is bound by `as`. `__exit__(exc_type, exc_value, traceback)` runs on exit — always.

**The key detail most candidates miss:** if `__exit__` returns a **truthy** value, the exception is **suppressed** and execution continues after the `with` block. Returning `None` (the common case) lets it propagate. Accidentally returning something truthy from `__exit__` silently swallows exceptions — a nasty, hard-to-find bug.

Class-based vs `@contextlib.contextmanager`: the generator form is more readable for simple setup/teardown, but you must wrap the `yield` in `try/finally` or cleanup is skipped on exception. The class form is better when you need state, reuse, inheritance, or re-entrancy.

**Probe:** *"Write the generator version and tell me what's wrong if you omit the try/finally."*
→ On exception, the code after `yield` never runs — the resource leaks.

**Probe:** *"Real uses beyond files?"* → DB transactions, locks, temporary directories, patching/mocking in tests, timing, changing working directory, suppressing warnings, `decimal` precision contexts.

| Weak | Strong |
|---|---|
| "It closes files for you" | Knows the truthy-suppression rule; knows the `@contextmanager` try/finally requirement; names non-file uses |

---

### 13. `__new__` vs `__init__` `[S]`

**Expected:** `__new__` is a static method that **creates and returns** the instance. `__init__` is an instance method that **initializes** an already-created instance and must return `None`. `__init__` is only called if `__new__` returned an instance of the class.

Override `__new__` when you need to control creation itself: subclassing immutables (`int`, `str`, `tuple`) where state must be set before the object exists; implementing singletons or instance caching/interning; returning a different type or a cached instance; some metaclass and `__init_subclass__` interactions.

**Probe:** *"What's the classic bug?"*
→ Overriding `__new__`, returning an instance, and then being surprised that `__init__` runs *again* on a cached instance every time the class is called. Singleton-via-`__new__` re-initializes unless you guard it.

| Weak | Strong |
|---|---|
| "`__new__` is the real constructor" with no use case | Names immutable subclassing specifically; knows the re-`__init__` trap |

---

### 14. MRO and `super()` `[S]` — *see Snippet S7*

**Expected:** The MRO is the linearized order Python searches classes for attributes, computed by **C3 linearization**, which guarantees: a class precedes its parents, parent order is preserved, and the result is monotonic. If no consistent order exists, the class definition itself raises `TypeError`.

`super()` does **not** mean "my parent". It means "the next class after this one in the MRO **of the instance's type**". That's why in a diamond, `B`'s `super()` can resolve to `C` even though `B` knows nothing about `C`. This is what makes cooperative multiple inheritance work — and it only works if *every* class in the chain calls `super()`.

Zero-argument `super()` is compiler magic: it uses an implicit `__class__` cell and the first positional argument, which is why it only works lexically inside a class body.

**Probe:** *"What breaks if one class in the chain doesn't call `super()`?"* → the rest of the chain is silently skipped.
**Probe:** *"Why do mixins go to the left?"* → they need to precede the base class in the MRO to override it.

| Weak | Strong |
|---|---|
| "`super()` calls the parent class" | Traces D→B→C→A correctly; explains cooperative inheritance; knows `__mro__` is inspectable |

---

### 15. `__eq__`, `__hash__`, and hashability `[S]` — *see Snippet S6*

**Expected:** The contract: **if `a == b` then `hash(a) == hash(b)`**. The converse need not hold — equal hashes with unequal objects is just a collision, which hash containers handle.

Defining `__eq__` sets `__hash__ = None`, making instances unhashable. This is deliberate: Python assumes your new equality has invalidated the default identity hash. You must define `__hash__` explicitly (typically `hash((self.a, self.b))` over the same fields `__eq__` uses).

An object is hashable if it has a `__hash__` that returns an int and its hash is **stable for its lifetime**. The real rule is not "immutable" but "the fields that participate in the hash never change". Mutating a hashed field after insertion puts the object in the wrong bucket — lookups fail, and `obj in container` returns `False` for an object that's demonstrably inside it.

**Probe:** *"How do you get this right with minimal code?"* → `@dataclass(frozen=True)` generates both correctly and prevents mutation. That's the answer for 90% of cases.

**Probe:** *"Should `__eq__` return `False` for an unrelated type?"* → Return `NotImplemented`, not `False`, so Python can try the reflected operation. Returning `False` silently breaks symmetric comparison with types that *could* compare equal.

| Weak | Strong |
|---|---|
| Knows "they go together" | States the contract directionally; knows `__eq__` nulls `__hash__`; reaches for frozen dataclass; mentions `NotImplemented` |

---

### 16. Descriptors vs `@property` `[St]`

**Expected:** A descriptor is any object defining `__get__`, `__set__`, or `__delete__`, placed as a **class attribute**. It intercepts attribute access on instances of that class.

- **Data descriptor:** defines `__set__` or `__delete__`. Takes priority **over** the instance `__dict__`.
- **Non-data descriptor:** only `__get__`. The instance `__dict__` takes priority over it.

That priority rule is the whole lookup order: data descriptors → instance dict → non-data descriptors and class attributes → `__getattr__`.

`@property` **is** a data descriptor — a convenience wrapper for the one-attribute case. The reason to drop to a raw descriptor is **reuse**: a property's logic is bound to one attribute on one class. A descriptor class (`PositiveNumber`, `ValidatedString`) can be instantiated for many attributes across many classes without repetition.

Functions are non-data descriptors — that's precisely how `obj.method` becomes a bound method: `function.__get__` returns a bound method object. This also explains why assigning a function to an *instance* doesn't create a bound method.

**Probe:** *"Why are functions descriptors?"* — the single best test of whether someone understands this or memorized it.
**Probe:** *"When is a descriptor over-engineering?"* → fewer than ~3 uses. A property is clearer.

| Weak | Strong |
|---|---|
| Can't define a descriptor | Explains data vs non-data priority; derives bound methods from it; knows when it's overkill |

---

### 17. `__getattr__` vs `__getattribute__` `[St]`

**Expected:**
- `__getattribute__` is called for **every** attribute access, unconditionally. It implements the normal lookup.
- `__getattr__` is called **only as a fallback**, when normal lookup raised `AttributeError`.

So `__getattr__` is safe and common — proxies, lazy loading, attribute-style dict access. `__getattribute__` is dangerous: any attribute access inside it recurses infinitely unless you delegate to `super().__getattribute__(name)`. It also sits on the hot path of every single access, so it's a real performance cost.

**Probe:** *"What are the debugging consequences?"*
→ Typos stop raising `AttributeError` and start returning something plausible. `hasattr` becomes a lie. IDE autocomplete and static type checking stop working. `repr` can recurse. A 4 brings these up unprompted — the judgment that *dynamic attribute access trades debuggability for convenience* is the real signal.

**Probe:** *"Why does `__getattr__` on a class with `__slots__` behave differently?"* → missing slots raise `AttributeError`, so `__getattr__` fires.

| Weak | Strong |
|---|---|
| Thinks they're the same | Knows the fallback-vs-always distinction and the recursion trap; argues about maintainability cost |

---

### 18. The GIL `[S]`

**Expected:** The Global Interpreter Lock is a mutex in **CPython** (not in the language spec) ensuring only one thread executes Python bytecode at a time. It exists to make reference counting safe without per-object locks.

**What it prevents:** true parallel execution of pure-Python CPU-bound code across threads. N threads on N cores give you roughly 1 core of Python throughput, plus contention overhead.

**What it does NOT prevent:**
- **Concurrency during I/O** — the GIL is released during blocking I/O, so threads genuinely overlap network and disk waits
- **Parallelism in C extensions** — NumPy, compression, crypto, and many libraries release the GIL around heavy native work
- **Race conditions.** This is the most important correction. The GIL guarantees bytecode-level atomicity, not operation-level. `counter += 1` is load/add/store — three bytecodes, interruptible between them. You still need locks.
- **Multiprocessing parallelism** — separate processes, separate GILs

**Probe:** *"So does the GIL make Python slow?"* → No. It makes *multithreaded CPU-bound Python* non-parallel. Single-threaded speed is unaffected; Python's single-thread performance is a separate topic.
**Probe:** *"What's changing?"* → PEP 703 free-threaded builds (3.13+, experimental) remove it, at a single-thread performance cost and with extension-compatibility work required. A 4 knows this exists without overclaiming that it's production-ready.

| Weak | Strong |
|---|---|
| "The GIL means Python can't do threads" | Separates I/O from CPU; explicitly says the GIL doesn't give you thread safety; knows it's CPython-specific |

---

### 19. Threading vs multiprocessing vs asyncio `[S]`

**Expected:** Start by classifying the workload, not by preference.

| Workload | Choice | Why |
|---|---|---|
| I/O-bound, moderate concurrency, blocking libraries | **Threads** / `ThreadPoolExecutor` | GIL released during I/O; works with any existing sync library; no rewrite |
| I/O-bound, very high concurrency (thousands) | **asyncio** | No per-connection thread stack; cheap tasks; explicit scheduling |
| CPU-bound pure Python | **Multiprocessing** / `ProcessPoolExecutor` | Separate GILs = real parallelism |
| CPU-bound in NumPy/native code | **Threads can work** | Those libraries release the GIL |
| Embarrassingly parallel, large scale | **Separate processes or machines** | Past a point, it's an architecture question |

Costs that must be named: processes pay serialization (pickle) and startup cost, and can't share objects cheaply — so they're wrong for small, frequent tasks. Asyncio is viral: it requires async-native libraries top to bottom, and one blocking call poisons the loop.

**Probe:** *"You have an existing sync codebase and an I/O bottleneck. Threads or asyncio?"*
→ Threads, almost always. Rewriting to async is a large, risky migration for a benefit that only materializes at high concurrency. A candidate who says "asyncio, it's faster" is a 2.

| Weak | Strong |
|---|---|
| Picks by familiarity | Classifies the workload first; names serialization and migration costs; gives the pragmatic answer over the fashionable one |

---

### 20. Blocking calls in async code `[S]` — *see Snippet S10*

**Expected:** Asyncio is **cooperative** — a single thread runs one task until it voluntarily yields at an `await`. A blocking call (`time.sleep`, `requests.get`, a sync DB driver, CPU-heavy work, even a large `json.loads`) never yields, so the entire event loop stops. Every other task — including heartbeats, timeouts, and health checks — is frozen. Your "concurrent" server silently serializes, and the symptom is latency spikes, not an error.

**Detection:**
- `loop.set_debug(True)` logs callbacks exceeding `slow_callback_duration`
- `asyncio.all_tasks()` / task introspection to see what's stuck
- Latency that scales linearly with concurrency is the tell
- Static checks: linters that flag known-sync calls inside `async def` (e.g. `flake8-async`)

**Fixes:** use an async-native library; `await asyncio.to_thread(fn, ...)` for blocking I/O; `run_in_executor` with a **process** pool for CPU work.

**Probe:** *"Why is this worse than the same bug in a threaded server?"*
→ With threads, the OS preempts — one slow call degrades one request. With asyncio, it stops everything. The blast radius is categorically different, and that's the point.

| Weak | Strong |
|---|---|
| "You should use async libraries" | Explains cooperative scheduling as the root cause; names a detection method; knows `to_thread` vs process pool |

---

### 21. Imports and import-time side effects `[S]` — *see Snippet S16*

**Expected:** On first import, Python finds the module, compiles it (caching bytecode in `__pycache__`), creates a module object, **inserts it into `sys.modules` before executing it**, then executes the body top to bottom. Subsequent imports return the cached `sys.modules` entry without re-executing.

That "insert before execute" ordering is exactly what makes circular imports fail with a *partially initialized module* rather than infinite recursion.

Import-time side effects are dangerous because module bodies run at import, which means: connecting to a database or reading config on import makes the module unimportable in CI; the work happens even when the module is imported only for a type; it slows startup; import order becomes load-bearing; and it's untestable because you can't import without triggering it.

The rule: **module bodies should define things, not do things.** Side effects belong behind a function call or `if __name__ == "__main__":`.

**Probe:** *"How do you fix a circular import, in order of preference?"*
→ (1) Fix the dependency direction — the cycle is usually a design message. (2) Extract shared code to a third module. (3) Function-level import. (4) `if TYPE_CHECKING:` when the cycle exists only for annotations.

**Probe:** *"What's in `sys.modules`?"* → the cache of already-imported modules, keyed by full dotted name; mutating it is how import mocking works.

| Weak | Strong |
|---|---|
| "Imports run the file" | Knows the sys.modules-before-execute ordering and derives circular-import behavior from it; ranks the fixes |

---

### 22. Unexpected memory growth `[S]`

**Expected:** Python's memory is managed by reference counting plus a cyclic garbage collector for reference cycles. Memory "leaks" in Python are almost always **unintended retention**, not true leaks.

Common causes, roughly by frequency:
- **Unbounded caches** — a module-level dict, `lru_cache(maxsize=None)`, or a memo that never evicts
- **Lingering references** — appending to a module-level list, registries, observer/callback lists that never unregister, long-lived closures capturing big objects
- **`lru_cache` on methods** — `self` becomes part of the key, pinning every instance *(Snippet S15)*
- **Exception tracebacks held in a variable** — a traceback references every frame and every local in them; storing one keeps a whole call stack alive
- **Reference cycles containing objects the GC can't collect quickly**, or cycles created faster than the collector runs
- **Fragmentation** — freed objects return to Python's allocator, not always to the OS, so RSS stays high even when Python-visible usage drops
- **Native extension leaks** — invisible to Python-level tooling
- **Thread/task accumulation** — threads that never exit, or `asyncio` tasks created and never awaited

**Tooling:** `tracemalloc` for allocation sites with line numbers, `gc.get_objects()` / `objgraph` for retention paths, `sys.getrefcount`, and simply plotting RSS over time to distinguish growth from a plateau.

**Probe:** *"RSS is flat for a week then climbs. Where do you look first?"* → correlate with deploys, traffic shape, or a cache that only fills under a rare code path. A strong candidate says "reproduce and measure" before naming a cause.

| Weak | Strong |
|---|---|
| "Python has GC so no leaks" or just "circular references" | Names caches and lingering references first; knows tracemalloc; distinguishes retention from fragmentation |

---

### 23. Debugging a post-refactor slowdown `[S]`

**Expected:** The method matters more than the answer. A strong response is:

1. **Quantify it.** How much slower, on what input size, measured how? "Feels slow" isn't a starting point.
2. **Reproduce deterministically** with a benchmark you can re-run.
3. **Bisect.** The refactor is a known-good/known-bad boundary — `git bisect` makes this mechanical.
4. **Profile, don't guess.** `cProfile` for a function-level breakdown, `timeit` for micro-comparisons, a sampling profiler (`py-spy`) for a live service you can't instrument.
5. **Read the diff for complexity changes**, not style changes.
6. **Fix, then re-measure** to confirm the hypothesis was right.

Common refactor-induced regressions: a list replaced by a generator that's now consumed multiple times (or vice versa); a set membership check turned into a list check; an `lru_cache` removed or made ineffective by making an argument unhashable/mutable; a loop-invariant query moved inside the loop (N+1); eager materialization of something previously lazy; a `__eq__`/`__hash__` change degrading dict performance; an added property turning a cheap attribute read into a method call inside a hot loop.

**Probe:** *"Profiler says 80% of time is in a function you can't change. Now what?"* → call it less (cache, batch, hoist out of the loop), call it with less data, or change the algorithm around it.

| Weak | Strong |
|---|---|
| Starts guessing at optimizations | Measures → bisects → profiles → fixes → re-measures; names concrete refactor regressions they've seen |

---

### 24. `__slots__` `[St]`

**Expected:** `__slots__` replaces the per-instance `__dict__` with a fixed array of descriptors. Benefits: significantly less memory per instance (often 40–50% for small objects) and slightly faster attribute access.

Costs:
- No `__dict__` — you cannot add attributes not declared in slots (sometimes a feature)
- No `__weakref__` unless you declare it — breaks `weakref`, and anything built on it
- Inheritance is fiddly: a subclass without `__slots__` reintroduces `__dict__`, erasing the savings; multiple inheritance from two slotted classes with nonempty slots raises
- Interacts awkwardly with default `pickle`, `copy`, and some ORMs/serializers
- Makes the class less introspectable and less monkeypatchable — which hurts testing

**When it's worth it:** you have measured that instance count is a memory problem — typically millions of small objects (points, rows, graph nodes, parsed tokens). Otherwise it's premature.

**Probe:** *"What would you try first?"* → `@dataclass(slots=True)` gets you both ergonomics and the memory win. Or reconsider the data model entirely: a few arrays (or a DataFrame) often beat millions of objects by far more than `__slots__` would.

| Weak | Strong |
|---|---|
| "Makes it faster" with no numbers | Leads with "only after measuring"; knows the inheritance and weakref caveats; suggests a better data layout first |

---

### 25. "How would you make this testable?" `[S]`

**Ask this about code the candidate just wrote.** It's the highest-signal question in the pack because it's unrehearsable and reveals real habits.

**Expected:** They should identify what makes the code hard to test and propose seams:
- **Hidden dependencies** — `datetime.now()`, `random`, `uuid4()`, env vars, module-level config, direct network/DB/filesystem calls. Fix: inject them as parameters with sensible defaults, or isolate them behind one thin boundary.
- **Doing too much** — a function that fetches, transforms, and writes can't have its transform tested. Fix: separate I/O from logic. Pure functions are trivially testable.
- **Side effects at import time** — makes the module untestable in isolation.
- **Global/shared state** — makes tests order-dependent and flaky.

Then they should name the actual tests: happy path, empty input, boundary values, the error path, and the specific bug they'd be worried about.

**Probe:** *"Would you mock here?"*
→ A strong answer is "mock at the boundary, not in the middle." Mocking your own internals couples tests to implementation and makes refactoring painful. Heavy mocking is usually a signal that the design needs a seam, not that the test needs more patches.

**Probe:** *"How do you test the time-dependent bit?"* → inject a clock, use `freezegun`, or pass the timestamp in. "Patch `datetime`" works but is more brittle, and they should know *why* `patch` targets where the name is **used**, not where it's defined.

| Weak | Strong |
|---|---|
| "Add unit tests" with no structural change | Identifies specific seams; separates pure logic from I/O; treats heavy mocking as a design smell; names concrete cases including edge cases |

---

# Appendix B — Model Answers for Sections 19–29

Compact answer keys for the sections that most often get asked by non-specialist panel members. Format: **question number → the answer you're listening for → the probe that separates a 3 from a 4.**

---

## Section 19 — Strings, Bytes, Encoding

**226. `str` vs `bytes`** — `str` is a sequence of Unicode *code points*; `bytes` is a sequence of 0–255 integers. They never implicitly convert in Python 3. The boundary is every I/O edge: files, sockets, subprocess output, hashing, and crypto all want `bytes`.
*Probe:* "Which does `open()` give you?" → text mode decodes using a platform-dependent default; `encoding=` should always be explicit. `errors='strict'` is the default.

**227. Encode vs decode / `UnicodeDecodeError`** — `encode` goes `str → bytes`, `decode` goes `bytes → str`. The error means the byte sequence isn't valid in the assumed codec — usually because the real encoding differs from the assumed one.
*Strong answer:* fix the source assumption, don't paper over it. `errors='replace'` or `'ignore'` silently corrupts data; `errors='surrogateescape'` is the right tool for round-tripping unknown bytes (and is what Python uses for filesystem paths).

**228. String immutability and performance** — every "modification" allocates a new string, so `s += x` in a loop is O(n²). Use a list plus `"".join()`, which is O(n). *(CPython has an in-place optimization for the refcount-1 case, but it's an implementation detail — don't rely on it.)*

**229. Formatting tradeoffs** — f-strings: fastest and clearest, evaluated eagerly at the call site. `.format()`: works with externally supplied templates. `%`: legacy, still required by `logging`. `str.join`: for concatenating sequences, not interpolation.

**230. Why `logger.info("x=%s", x)` not an f-string** — the f-string is formatted *before* the call, so the cost is paid even when the log level filters the record out. The `%s` form defers formatting until a handler actually emits it. Secondary benefit: the raw template stays available for structured logging and log aggregation/grouping.

**231. `split()` vs `split(" ")`** — bare `split()` splits on *runs* of any whitespace and drops leading/trailing empties. `split(" ")` splits on each single space, producing empty strings for runs. `"a  b".split()` → `['a','b']`; `"a  b".split(" ")` → `['a','','b']`.

**232. Code point vs character vs grapheme `[St]`** — `len()` counts code points, not what a user perceives as a character. Combining marks and emoji (ZWJ sequences, skin-tone modifiers) make one visible glyph span several code points. Normalization (`unicodedata.normalize`, NFC/NFD) matters for comparison; true grapheme segmentation needs a third-party library.

**233. `sys.intern` `[St]`** — forces a string into the interned table so equal strings share one object, making `==` short-circuit on identity and cutting memory. Worth it only for large numbers of repeated, long-lived keys (e.g. parsed field names). Python auto-interns identifier-like literals.

---

## Section 20 — Stdlib Fluency

**234. `itertools` in practice** — `chain` (flatten one level), `islice` (slice any iterable), `groupby`, `product` (replaces nested loops), `combinations`/`permutations`, `count`/`cycle`/`repeat`, `accumulate` (running totals), `pairwise`, `tee`. All lazy.

**235. `groupby` precondition** — **the input must already be sorted by the key**; it only groups *consecutive* equal keys. The classic bug is calling it on unsorted data and getting fragmented groups with no error. Also: each group is an iterator that becomes invalid once you advance to the next group — materialize with `list()` if you need it later. If you just want grouping, `defaultdict(list)` is usually the better tool.

**236. `chain` vs `+`** — `chain` is lazy and works on any iterables including generators and mixed types; `+` requires same-type sequences and builds a new concatenated object in memory. Use `chain.from_iterable` for a sequence of sequences.

**237. `lru_cache` hazards** — arguments must be hashable (so no list/dict/set args); `maxsize=None` grows without bound; keyword vs positional calls produce *different* cache entries; it pins arguments and return values in memory; on a method it pins `self` forever *(Snippet S15)*; it's wrong for anything non-deterministic or time-sensitive. Inspect with `.cache_info()`, reset with `.cache_clear()`.

**238. `reduce`** — clearer than a loop only for genuinely associative folds where a built-in doesn't exist. `sum`, `min`, `max`, `math.prod`, `any`, `all`, and `itertools.accumulate` cover most real uses and read far better. Guido deliberately moved it out of builtins.

**239. `singledispatch` `[St]`** — type-based dispatch on the first argument, registered out-of-band. Better than an `isinstance` chain when the set of types is open and extensible (third parties can register their own). Worse when logic is tightly coupled — then it just scatters a single decision across the file. `singledispatchmethod` is the method form.

**240–241. `key=`, stability** — `key` computes a sort value once per element (Schwartzian transform, O(n) calls); `cmp` needed O(n log n) pairwise calls and was removed in Python 3. **Timsort is stable**, so equal keys keep their original order. Exploit it with multi-pass sorting: sort by the *least* significant key first, then the most significant. For mixed directions on a single pass, use a tuple key with negation for numbers — strings can't be negated, which is why multi-pass or `cmp_to_key` is sometimes required *(see D16, D27)*.

**242. `heapq` vs sorting** — top-k is O(n log k) with a heap versus O(n log n) for a full sort, and the heap needs only O(k) space. `heapq.nlargest`/`nsmallest` accept a `key`. For k close to n, just sort. For k == 1, use `max`/`min`.

**243. `bisect`** — binary search over a **sorted** sequence. `bisect_left` returns the leftmost valid insertion point, `bisect_right` the rightmost; the pair gives you the range of equal elements. `insort` inserts in order, but note insertion itself is O(n) on a list. The `key=` parameter was added in 3.10.

**244. `itemgetter` / `attrgetter`** — implemented in C, so measurably faster than an equivalent lambda in hot sorts, and they support multiple fields (`itemgetter(1, 0)`) returning a tuple. Mostly a readability and micro-performance win.

**245. Stable priority queue `[St]`** — push `(priority, counter, item)` with a monotonically increasing counter. This guarantees FIFO ordering among equal priorities **and** prevents Python from ever comparing the payload objects, which would raise `TypeError` if they're not orderable *(see D23)*.

---

## Section 21 — ABCs, Protocols, Metaclasses

**246. `abc.ABC` / `@abstractmethod`** — enforcement happens at **instantiation**, not at class definition. Subclassing without implementing all abstract methods is legal; the `TypeError` fires when you try to create an instance. Also: it's a runtime check, not a static one.

**247. ABC vs duck typing vs `Protocol`** — duck typing for internal, fast-moving code. An **ABC** when you want a shared base with some default implementation plus runtime enforcement, and you control the subclasses. A **`Protocol`** when you want structural typing checked statically — the implementer doesn't need to know your type exists, which is the right answer for library boundaries.

**248. `collections.abc`** — inheriting `Mapping` gives you `__contains__`, `keys`, `items`, `values`, `get`, `__eq__` for free from just `__getitem__`, `__iter__`, `__len__`. Also makes `isinstance(x, Mapping)` work correctly. Big win over hand-rolling a dict-like.

**249–250. Metaclasses `[St]`** — a metaclass is the class *of* a class; `type(name, bases, namespace)` is what runs on every `class` statement. Legitimate uses: framework-level class registration, ORM/field declarative APIs, enforcing API invariants across a hierarchy. **The honest senior answer is that `__init_subclass__` (3.6+) and class decorators cover almost all of these more simply**, so a metaclass should be justified by something they can't do — typically controlling the namespace *during* class body execution via `__prepare__`.

**251. `__slots__` and inheritance `[St]`** — a subclass that doesn't define `__slots__` gets a `__dict__`, silently undoing the memory saving. Inheriting from two classes that both define non-empty `__slots__` raises `TypeError`. Declaring the same slot in parent and child wastes space. ABCs work fine with slots as long as the chain stays consistent.

---

## Section 22 — Serialization

**252. `pickle` risks** — unpickling executes arbitrary code via `__reduce__`, so untrusted input is remote code execution. It is not a security boundary of any kind. Also: not portable across languages, fragile across Python and class versions, and the format differs by protocol.

**253. `pickle` vs JSON** — pickle handles arbitrary objects, cycles, custom classes, tuples-vs-lists distinction, and non-string dict keys. JSON is human-readable, cross-language, safe on untrusted input, but limited to its six types — it silently converts tuples to lists and coerces dict keys to strings.

**254. `__getstate__` / `__setstate__` `[St]`** — control what gets serialized. Needed when an object holds unpicklable resources (sockets, file handles, locks, DB connections, thread objects): drop them in `__getstate__`, re-establish in `__setstate__`. Also used to migrate old pickles into a new attribute layout.

**255. JSON round-trip for rich types** — JSON has no datetime or Decimal. Options: a custom `JSONEncoder.default` plus an `object_hook` for the way back; ISO-8601 strings for datetimes (and keep the timezone — naive datetimes are the real bug); `Decimal` as a *string*, never a float, or you've thrown away the precision you chose Decimal for.

**256. Class changed since pickling** — pickle stores the module path and class name, then restores attributes into a new instance, normally bypassing `__init__`. Renamed or moved classes raise on load; removed attributes linger; newly added attributes are simply absent, so code reading them hits `AttributeError`. `__setstate__` with a version field is the migration tool.

**257. Why process pools need picklability `[St]`** — arguments and return values are serialized to cross the process boundary. Commonly unpicklable: lambdas, locally-defined functions and classes, open file handles, locks, generators, and most closures. This also means large arguments are copied, which is often the hidden cost that makes a process pool slower than the serial version.

---

## Section 23 — Numerics

**258. `0.1 + 0.2 != 0.3`** — IEEE-754 binary floats can't represent most decimal fractions exactly; the sum is `0.30000000000000004`. Compare with `math.isclose(a, b)` rather than `==`. For display, format to fixed precision.

**259. `Decimal` vs `float`** — `Decimal` is base-10 with configurable precision and explicit rounding modes, so it represents decimal fractions exactly. **Use it for money**, always — or use integer minor units (paise/cents), which is what many payment systems do. `float` is for measurement and scientific work where relative error is acceptable.

**260. Arbitrary-precision `int`** — no overflow, ever, so algorithms that would need `BigInteger` elsewhere just work and `(lo+hi)//2` is safe in binary search. Cost: large ints are slower and use more memory than machine words, and they can't be used directly where a fixed-width type is required (C interop, `struct`, numpy dtypes — where numpy *will* wrap around silently).

**261. `/` vs `//`** — `/` is always true division returning a float (even for ints); `//` is floor division, rounding toward **negative infinity**. So `-7 // 2 == -4`, not `-3`, and `-7 % 2 == 1` since the sign follows the divisor. For truncation toward zero use `int(a/b)` or `math.trunc` — but beware float precision on very large ints. *(See D32.)*

**262. `math.isclose` `[St]`** — `rel_tol` is relative to the larger magnitude (default 1e-9); `abs_tol` is an absolute floor, defaulting to 0. **You must set `abs_tol` when comparing against zero**, because relative tolerance is meaningless there.

---

## Section 24 — Testing and Testability

**263. What to test** — behavior at boundaries, not implementation. Prioritize: complex pure logic, bug-fix regressions, anything with many branches, public API contracts. Don't test framework internals, trivial getters, or mocks mocking mocks.

**264. What makes a function hard to test** — hidden dependencies (clock, randomness, network, filesystem, env vars, globals), doing I/O and logic in one place, import-time side effects, and deep call chains with no seam. Fix by injecting dependencies and separating pure computation from I/O.

**265. Fixtures and scopes** — a fixture provides setup/teardown as a dependency, requested by parameter name. Scopes: `function` (default, safest), `class`, `module`, `session` (fastest, but shared state leaks between tests). The tradeoff is isolation vs. speed; a session-scoped mutable fixture is a classic source of order-dependent flakiness. Teardown goes after `yield`.

**266. `parametrize`** — one test function, many input/expected pairs; each runs as a separate reported test. It hurts when the parameter list encodes several *different* behaviors, when `ids` are unreadable, or when the test body sprouts conditionals on the parameters — that's a sign it should be two tests.

**267. Mock vs stub vs fake vs spy** — **Stub**: returns canned values. **Fake**: a real but simplified implementation (in-memory repository). **Spy**: records calls while delegating to the real thing. **Mock**: pre-programmed with expectations and asserts on *how* it was called. Prefer fakes for behavior, stubs for inputs; reserve call-assertions for genuine interaction contracts.

**268. `patch` where it's used** — `patch` rebinds a name in a namespace. If `mymodule` does `from utils import now`, then `mymodule.now` is a *separate* reference, so patching `utils.now` has no effect on it. You must patch `mymodule.now`. This single rule is the top source of "my mock didn't work".

**269. When mocking is a design smell** — when a unit test needs five or more patches, when patching private internals, when tests break on every refactor despite unchanged behavior, or when the mock setup is longer than the assertion. The fix is a seam, not more patches.

**270. `monkeypatch`** — pytest's scoped, auto-undone patching for env vars (`setenv`), attributes (`setattr`), dict entries (`setitem`), `sys.path`, and cwd (`chdir`). Preferred over manual save/restore because teardown is automatic even on failure.

**271. Time, randomness, filesystem** — inject a clock function or pass the timestamp; seed the RNG or inject a `Random` instance; use `tmp_path`/`tmp_path_factory` for real files, or an in-memory fake. Patching the library (`freezegun`, `time-machine`) works but is more brittle than injection.

**272. Testing generators and async** — for a generator, assert on `list(gen())` for the full output, or drive it with `next()` to test laziness and ordering, and use `pytest.raises(StopIteration)` for exhaustion. For async, use `pytest-asyncio` with `@pytest.mark.asyncio` (or `anyio`), and `AsyncMock` for awaited dependencies. Test cancellation explicitly if the code handles it.

**273. Dependency injection, lightweight** — in Python this is usually just *default parameters*: `def process(items, now=datetime.now, http=requests.get)`. No container or framework needed. Constructor injection for classes. The goal is a seam, not architecture.

**274. What coverage tells you `[St]`** — which lines *executed*, not which behaviors were *verified*. 100% coverage with no assertions is worthless. It's a good inverse signal (uncovered code is definitely untested) and a poor positive one. Branch coverage is more informative than line coverage.

**275. Regression tests for performance/memory `[St]`** — assert on complexity-sensitive observables rather than wall-clock time (which is flaky in CI): count queries or function calls, measure allocated objects with `tracemalloc`, or assert the result on an input size large enough that a quadratic regression times out. `pytest-benchmark` with explicit thresholds if you must measure time.

---

## Section 25 — Modern Python

**276. Walrus `:=`** — assigns inside an expression. Genuinely useful for `while (chunk := f.read(8192)):`, avoiding a double call in comprehension filters (`[y for x in xs if (y := f(x)) is not None]`), and keeping a match object. Overused, it hurts readability.

**277. `match`** — structural pattern matching destructures *and* branches: sequence, mapping, and class patterns with capture, guards, and `|` alternatives. Strong for parsing ASTs, protocol messages, and tagged unions. Wrong tool for simple equality chains (a dict dispatch is clearer) and it is **not** a C-style switch — a bare name is a capture pattern that always matches, which is the classic beginner bug.

**278. `|` and `|=` on dicts** — `d1 | d2` returns a new merged dict (right wins on conflicts); `|=` updates in place. Replaces `{**d1, **d2}`.

**279. `zoneinfo`** — IANA timezone support in the stdlib, removing the hard `pytz` dependency. Critically, it uses the normal `datetime` API (`datetime(..., tzinfo=ZoneInfo("Asia/Kolkata"))`), avoiding pytz's `localize()` footgun where constructing directly gave a wrong historical offset.

**280. Built-in generics** — `list[int]` instead of `typing.List[int]`, no import needed (PEP 585). Along with `X | None` (PEP 604) replacing `Optional[X]`.

**281. Exception groups / `except*` `[St]`** — represent *multiple* simultaneous exceptions, which is what concurrent code produces. `asyncio.TaskGroup` raises an `ExceptionGroup` when several children fail; `except*` matches and handles a subset while letting the rest propagate.

**282. `typing.Self` `[St]`** — annotates methods returning an instance of the actual (possibly sub-) class — builders, `from_*` constructors, fluent APIs. Before it, you needed a `TypeVar` bound to the class and had to remember to annotate `cls`/`self`.

**283. Version-support discipline `[St]`** — target the lowest supported version in CI, not just the newest; gate newer syntax behind version checks or avoid it; `from __future__ import annotations` for newer annotation syntax on older runtimes; set `requires-python` in `pyproject.toml`; test the full matrix. Tools like `ruff`'s target-version or `vermin` catch accidental use of too-new features.

---

## Section 26 — Logging and Packaging

**284–285. Library vs application logging** — a **library** gets `logging.getLogger(__name__)` and adds *no* handlers and no `basicConfig`; optionally a `NullHandler` to suppress "no handler" warnings. The **application** owns configuration: handlers, levels, formatters, `dictConfig`. A library calling `basicConfig` hijacks the host application's global logging setup — it's a side effect on someone else's program.

**286. Logger / handler / filter / formatter** — **Logger**: the named entry point, arranged in a dotted hierarchy with propagation to parents. **Handler**: where a record goes (file, stream, HTTP). **Filter**: fine-grained include/exclude, and a common place to inject context. **Formatter**: how the record is rendered. One record can reach many handlers.

**287. Structured context without threading it through** — `LoggerAdapter` for static context, a `Filter` that injects fields, `extra={...}` per call, and **`contextvars`** for request-scoped values that work correctly under both threads and asyncio (thread-locals don't). Then a JSON formatter to emit it.

**288. Virtual environments** — an isolated `site-packages` and interpreter symlink per project, so projects can't conflict and you never install into the system Python.

**289. Lock file vs requirements** — a requirements file typically states *direct* dependencies with loose constraints; a lock file pins **every** transitive dependency to an exact version with hashes, making builds reproducible and verifiable. Applications should commit a lock file; libraries should specify ranges and not lock.

**290. `pyproject.toml` vs `setup.py`** — `pyproject.toml` is the declarative standard (PEP 517/518/621) covering build backend, metadata, and tool config in one file. `setup.py` is executable code, which made builds non-deterministic and required running arbitrary code to read metadata. `setup.py` is now only needed for genuinely dynamic builds like C extensions.

**291. "Works on my machine" `[St]`** — compare `pip freeze` / `pip list` between environments; check the actual interpreter (`sys.executable`) and that the venv is really active; look for transitive version drift from an unlocked dependency; check platform-specific wheels vs source builds; check `sys.path` order and a stale local module shadowing an installed one; verify no `PYTHONPATH` surprise. Then fix it properly with a lock file.

---

## Section 27 — Debugging

**292. Can't reproduce locally** — close the environment gap first: version, data, concurrency, timezone/locale, scale. Add targeted logging with correlation IDs; capture the full traceback plus inputs; try to reproduce in a test with production-shaped data. Prefer making it reproducible over guessing a fix — a fix you can't verify isn't a fix.

**293. Unexpected `None`** — `None` almost always comes from an implicit `return` with no value, a mutating method used as if it returned (`x = lst.sort()`), a `dict.get` miss, a regex that didn't match, an early return on an error path, or an uninitialized attribute. Work backwards from the traceback; set a conditional breakpoint on `is None` rather than scattering prints.

**294. Slower after refactor** — see Core 25 #23: quantify → reproduce → `git bisect` → profile → fix → re-measure.

**295. Accidental quadratic** — `in` against a list inside a loop; string `+=`; `list.pop(0)`/`insert(0)`; building a new list by concatenation; re-slicing in a loop; nested loops over the same data; repeated `sorted()` inside a loop; an N+1 query pattern.

**296. Mutability and aliasing bugs** — compare `id()` to confirm sharing; check for a shared mutable default or class attribute; look for a shallow copy mistaken for a deep one; add a defensive copy at the boundary to confirm the hypothesis, then fix the design rather than leaving the copy.

**297. Shared state across threads** — reduce to a deterministic reproduction by increasing contention; add logging with thread IDs; look for non-atomic read-modify-write; test the single-threaded path to confirm the logic itself is correct; prefer handing data through a `queue.Queue` over adding more locks.

**298. Reading tracebacks** — read **bottom-up**: the last frame is where it raised, the exception type and message say what. "During handling of the above exception, another exception occurred" means an exception was raised *inside* an `except` block — the first one is usually the real cause. "The above exception was the direct cause" means explicit `raise ... from`. Chained tracebacks are a feature; don't truncate them in logs.

**299. Effective logging** — log at boundaries (entry/exit of a request, external calls, state changes), not every line. Use levels meaningfully; include identifiers, not prose; use `%s` lazy formatting; never log secrets or PII. If you need a debugger to understand a flow you log heavily, the logging is decorative.

**300. Debugger vs prints** — a debugger when you need to inspect many variables, when the failure is deep in a call stack, when you want conditional breakpoints, or when you don't yet know what to print. `breakpoint()` (respects `PYTHONBREAKPOINT`) is the modern entry point. Prints are fine for tight loops, async or multi-process code where debuggers are awkward, and production where you can't attach.

**301. Memory leak** — see Core 25 #22. Confirm growth is real (RSS trend, not one spike), snapshot with `tracemalloc` and diff, find the retention path with `gc.get_referrers`/`objgraph`, check caches and registries first.

**302. `cProfile` vs sampling vs `timeit`** — **`cProfile`**: deterministic, per-function call counts and cumulative time, but significant overhead that distorts fast functions. **Sampling** (`py-spy`, `austin`): low overhead, attaches to a *running* process, correct for production. **`timeit`**: microbenchmarks a small snippet with repetition and the GC disabled; the right tool for "is A faster than B", the wrong tool for finding a bottleneck.

**303. Profiling async `[St]`** — wall-clock profiling shows time parked in `await`, which is usually not the problem; function-level profilers attribute time to the event loop rather than to your logic. Use async-aware tooling (`py-spy` with thread/task view, `yappi` in wall vs CPU mode), the loop's debug mode for slow callbacks, and task-level instrumentation.

**304. `tracemalloc` `[St]`** — `tracemalloc.start()`, take a `snapshot()`, take a second one later, then `compare_to(..., 'lineno')` to get allocation deltas by source line. Increase `start(nframes)` to capture the call path, not just the final line.

---

## Section 28 — Design Judgment

**305. What a refactor improves first** — **readability**. Speed should be driven by measurement, and abstraction is the one most often added prematurely. Code is read far more than written, and clear code makes the other two safe to do later.

**306. Spotting over-clever code** — dense one-liners and nested comprehensions, needless metaclasses or decorators, `exec`/`eval`, dynamic attribute magic, operator overloading with non-obvious meaning, clever `__getattr__` proxies. The test: can a competent teammate modify it safely at 2am? Cleverness is a cost paid by every future reader.

**307. When "Pythonic" is a real argument** — when it means *using the language's own mechanisms* (context managers for cleanup, iteration protocol, EAFP, the stdlib) so behavior is predictable and composable. It's rhetorical when used to mean "the way I like it" or to justify terseness over clarity. Ask for the underlying reason — a 4 will give one.

**308. Function vs class vs module** — function for a stateless operation; class when behavior and state belong together or you need polymorphism; module for a cohesive namespace of related functions. Python does **not** require a class for everything — a class with one method and no state should be a function, and that recognition is itself a seniority signal.

**309. When to add an abstraction** — after the third concrete case, not before the first. Premature abstraction locks in the wrong shape and is harder to remove than duplication. Duplication is cheaper than the wrong abstraction.

**310. Reviewing correct-but-unmaintainable code** — separate blocking from non-blocking comments; explain the future cost concretely ("when we add X, this breaks because Y") rather than asserting a rule; propose a specific alternative; be willing to approve with a follow-up issue if it's not load-bearing. Reviewing the *approach* before the code was written is the real fix.

**311. Most useful review comment** — open-ended; listen for whether they value feedback on **design and naming** over style, and whether they describe giving feedback kindly and specifically. Mentorship signal.

**312. Evolvable library API `[St]`** — keyword-only arguments so you can add parameters without breaking positional callers; narrow public surface with `_private` internals and explicit `__all__`; accept abstract types (`Iterable`, `Mapping`) and return concrete ones; avoid leaking implementation types; version the API; never change behavior silently under the same name.

**313. Deprecation `[St]`** — emit `DeprecationWarning` (and make sure it's visible — it's hidden by default outside `__main__`), document the replacement and removal version, keep the old path working for at least one release, provide a migration note or codemod, then remove on a major version. Silent removal breaks trust; warning forever trains people to ignore warnings.

---

## Section 29 — Everyday Idioms

**314–315. EAFP vs LBYL** — **EAFP** ("easier to ask forgiveness than permission") attempts the operation and handles the exception; **LBYL** ("look before you leap") tests preconditions first. EAFP is idiomatic Python because it's **race-free** (no gap between check and use), it's faster in the common success case (exceptions cost only when raised), and it works with duck typing.
**LBYL is correct when** the exception would be expensive or destructive to trigger, when you'd have to catch something overly broad, when the check is genuinely cheap and the failure common (making the exception a control-flow cost), or when validating user input up-front to report *all* problems at once.

**316. The TOCTOU argument** — `if key in d: return d[key]` does two lookups and leaves a window where another thread, a signal handler, or a `__missing__`/property side effect can change the state in between. `os.path.exists(p)` followed by `open(p)` is the classic file-system race — the file can vanish in the gap. `try/except` performs the operation atomically from the caller's view.

**317. Duck typing** — behavior determines compatibility, not declared type. Practically: annotate and check against *capabilities* (`Iterable`, `Sequence`, a `Protocol`) rather than concrete classes, and avoid `isinstance` gatekeeping that rejects a perfectly good substitute.

**318. When EAFP becomes an anti-pattern** — when the `try` block is large enough that you can't tell which statement raised; when it catches broad exceptions and masks genuine bugs *(Snippet S11)*; when exceptions drive ordinary control flow in a hot loop; when it silently swallows and continues.

**319–320. `bool` is an `int`** — `bool` subclasses `int`, `True == 1`, `hash(True) == hash(1)`. So `sum(...)` over booleans counts them, which is a genuinely useful idiom. It bites when `True` and `1` collide as dict keys or set members *(Snippet S18)*, when `isinstance(x, int)` accepts a bool in type dispatch, and when a function taking a `bool` silently accepts `2`. Check with `isinstance(x, bool)` or `x is True`.

**321–322. Chained comparison** — `1 < x < 10` evaluates `x` **once** and short-circuits, unlike `(1 < x) and (x < 10)` which evaluates `x` twice — this matters if `x` is an expensive call or a property with side effects. `a == b == c` means `a == b and b == c`, *not* `(a == b) == c`; the surprise is that it does not compare `a` to `c` directly, which differs for types with non-transitive equality.

**323. `if x:` vs `if x is not None:`** — truthiness treats `0`, `0.0`, `""`, `[]`, `{}`, `set()`, and `None` as false. So `if not count:` wrongly fires when `count == 0`, and `if not items:` can't distinguish "empty list" from "no list provided". Use `is None` whenever the absent case and the empty/zero case are semantically different — which, for optional parameters, is almost always.

**324–325. `any` / `all`** — `any([])` is `False`, `all([])` is `True` (vacuous truth: no element violates the condition). The `all([])` result surprises people and is a real source of validation bugs on empty input. Both **short-circuit**, returning at the first `True`/`False` respectively — verifiable by passing a generator with a side effect and observing how far it advanced. Pass a generator expression, not a list comprehension, or you forfeit the short-circuit.

**326–327. `sort()` vs `sorted()`** — `list.sort()` sorts in place and returns `None`; `sorted(iterable)` returns a new list and accepts any iterable. Returning `None` is deliberate: it prevents `x = y.sort()` from looking like it produced a sorted copy, and signals mutation. Same convention for `list.append/extend/reverse/insert/remove`, `dict.update`, `set.add/discard`, and `random.shuffle` — **mutating methods return `None` in Python**. The classic bug is `for x in lst.sort():` → `TypeError: 'NoneType' object is not iterable`.

**328. `reversed()` vs `[::-1]`** — `reversed()` returns a lazy iterator with O(1) memory and needs a sequence (or `__reversed__`); `[::-1]` builds a full reversed **copy**. Use `reversed()` for iteration, slicing when you need an actual reversed sequence. `reversed()` on a dict iterates keys in reverse insertion order (3.8+).

**329. Slicing copies** — `a[1:]` creates a new list (shallow copy) in O(k). Doing it inside a loop — the common recursive `process(arr[1:])` pattern — makes an O(n) algorithm O(n²). Pass indices instead, or use `memoryview` for bytes. Note slicing is shallow: the *elements* are still shared.

**330–331. Unpacking** — `a, *rest = items` binds `a` to the first element and `rest` to a **list** of the remainder (always a list, even for tuple input, and `[]` if nothing is left). `first, *middle, last` works with at most one starred target. On the **call** side, `f(*args, **kwargs)` *unpacks* a sequence/mapping into arguments — the mirror image of collecting them in a definition. Also valid in literals: `[*a, *b]`, `{**d1, **d2}`.

**332. `enumerate`** — yields `(index, value)` lazily, replacing manual counters and `range(len(x))` indexing. `enumerate(x, start=1)` for 1-based output such as line numbers or rankings.

**333–335. `zip`** — stops at the **shortest** input, silently discarding the rest *(Snippet S17)* — a data-loss bug in ETL code. `zip(strict=True)` (3.10+) raises `ValueError` on length mismatch and should be the default choice when lengths are expected to match. `itertools.zip_longest(fillvalue=...)` pads instead. `zip(*rows)` transposes, and is also the idiom for unzipping pairs into two sequences.

**336. `itertools.pairwise`** — yields overlapping consecutive pairs: `pairwise([1,2,3])` → `(1,2), (2,3)`. Added in 3.10. Before it, people wrote `zip(x, x[1:])` (which copies and only works on sequences) or an explicit `tee`-based recipe.

**337–338. `for...else`** — the `else` runs when the loop finishes **without executing `break`**, including on an empty iterable *(Snippet S19)*. It is *not* "if the loop didn't run". Legitimate use: search-with-fallback without a found-flag. Discouraged because the keyword is misleading — most readers assume it means "if the loop body never ran" — so unless the team knows the construct, extracting the search into a function with an early `return` is clearer.

**339–340. `assert` under `-O`** — `python -O` sets `__debug__ = False` and **removes every `assert` from the compiled bytecode** *(Snippet S20)*. Any validation written as an assert silently disappears in optimized mode, so input checks, permission checks, and invariant enforcement on untrusted data must use explicit `if ... raise`. Asserts are for conditions that should be *impossible* — internal invariants, postconditions, unreachable branches, and test code. `-OO` additionally strips docstrings.

**341–342. `globals()` / `locals()` / `vars()`** — `globals()` returns the module namespace dict and **is** writable. `locals()` returns a snapshot of the local namespace inside a function; writing to it is **not** guaranteed to affect the actual locals, because CPython stores function locals in a fast array rather than a dict. (At module or class scope, `locals()` *is* the real namespace.) `vars(obj)` returns `obj.__dict__`; bare `vars()` is `locals()`.

**343. `dir()` / `callable()`** — `dir(obj)` lists attribute names (including inherited ones) for interactive exploration and tab completion; it's a debugging aid, not an API. `callable(x)` reports whether `x()` is valid — true for functions, classes, and anything with `__call__`.

**344. `removeprefix` / `removesuffix`** — remove an exact prefix/suffix, added in 3.9. They fix the long-standing `strip` bug: **`strip`/`lstrip`/`rstrip` take a *set of characters*, not a substring.** So `"example.com".strip(".com")` happens to give `"example"` and looks correct — but `"commerce.com".strip(".com")` returns **`"erce"`**, because it also eats the leading `c`, `o`, `m`, `m`. `removesuffix(".com")` does what people always meant.
*Probe:* "Why does the first example work and the second not?" A candidate who can explain that it strips *characters* from both ends until one isn't in the set understands it; one who memorized "use removesuffix" doesn't.

**345. f-string `=` specifier** — `f"{x=}"` renders as `x=5`, printing the expression source alongside its value. Added in 3.8 purely for debugging; it replaces `print("x =", x)` and keeps the two in sync. Combine with format specs: `f"{value=:.2f}"`.

**346. `__del__`** — called when the refcount hits zero, which is **not guaranteed to happen** — reference cycles, interpreter shutdown, and held tracebacks can delay or skip it. Exceptions inside it are swallowed and merely printed, and it can resurrect objects. Never use it for releasing files, sockets, or locks: use a **context manager**, with `weakref.finalize` as the fallback safety net.

**347. `id()` reuse** — `id()` is unique only among *simultaneously alive* objects. Once an object is collected, CPython can reuse the address, so a temporary's id can equal a later unrelated object's id. This is why `id(a) == id(b)` on freshly computed temporaries — and comparisons like `x is some_expression()` — produce confusing results, and why `is` is only for singletons.

**348. Multiple context managers** — `with open(a) as f, open(b) as g:` enters left-to-right and exits right-to-left, and is equivalent to nesting. Since 3.10 (officially; 3.9 in CPython) the parenthesized form allows clean multi-line splitting:
```python
with (
    open(a) as f,
    open(b) as g,
):
    ...
```
Use `contextlib.ExitStack` when the number of managers is dynamic.

---

# Appendix C — Model Answers for Sections 1–18

Answer keys for the classical core. Written from the perspective of a senior Python engineer at a product company: what a *correct* answer contains, what a **3 vs 4** sounds like, and the probe that separates them.

Where a question is already covered in Appendix A, the entry cross-references instead of repeating.

---

## Section 1 — Data Model and Core Semantics

**1. Everything is an object** — every value has a type, an identity, and attributes: ints, functions, classes, modules, and types themselves. Practical consequences: functions can be stored in dicts and passed around, classes can be created at runtime, modules can be introspected and patched. *Probe:* "Name something that is **not** an object." (Nothing, in user-visible Python. Keywords and syntax aren't values.)

**2. Explaining the object model to a Java/C++ engineer** — the three things that trip them up: (a) variables are *names bound to objects*, not typed storage — there is no "declaring a variable"; (b) there is no `new`, no value semantics, and no copy constructor — assignment never copies; (c) types live on objects, not names, so a name can be rebound to any type. Follow with: attribute access is a *runtime dict lookup* with a defined order, not a compile-time offset, which is why monkeypatching works and why attribute access is comparatively slow.

**3–4. Names vs objects, assignment** — see Appendix A #2. Short version: `x = obj` binds the name `x` in the current namespace to `obj`. Nothing is copied; the object's refcount increments. Rebinding a name never affects other names bound to the same object; mutating the object affects all of them.

**5. Call by sharing** — the callee receives its own *name* bound to the caller's *object*. So rebinding inside the function is invisible to the caller (unlike by-reference), but mutating the object is visible (unlike by-value).
```python
def f(lst, n):
    lst.append(1)   # visible to caller  - mutation
    lst = [99]      # invisible          - rebinding
    n += 1          # invisible          - int is immutable, so this rebinds
```
*Probe:* "So is Python pass-by-reference?" A 4 refuses the framing and explains why the question itself is malformed for Python.

**6–8. Identity, equality, `is None`** — see Appendix A #1. Additionally: `is None` is correct because `None` is a guaranteed singleton, and because a custom `__eq__` could return `True` for `== None` or raise. `is` cannot be overridden, so it's the only safe check.

**9. `256 is 256` vs `257 is 257` `[St]`** — CPython pre-allocates and caches small integers (−5 to 256), so identical small-int literals yield the same object. Beyond that range, each literal creates a new object — *except* that the compiler may also constant-fold or intern values within a single code object, which is why behavior differs between the REPL (statement-at-a-time) and a module (compiled as one unit). **Never rely on either.** It is a CPython implementation detail, not a language guarantee, and it differs on PyPy. *Strong answer names the real lesson:* this is exactly why `is` is reserved for singletons.

**10–11. Truthiness, `__bool__` vs `__len__`** — an object is truthy unless it defines `__bool__` returning `False`, or defines `__len__` returning `0`. **`__bool__` wins** — `__len__` is only consulted if `__bool__` is absent. Falsy builtins: `None`, `False`, `0`, `0.0`, `0j`, `""`, `()`, `[]`, `{}`, `set()`, `range(0)`. *Probe:* "Why is defining `__len__` on a non-container risky?" (You accidentally make it falsy when empty — the classic bug with custom collection wrappers and ORM querysets.) See also Q323.

**12. Dynamic typing in a large codebase** — benefits: less ceremony, duck typing, fast iteration, easy test doubles. Costs: whole classes of errors move from compile time to runtime; refactoring is riskier because you can't find all call sites reliably; IDE assistance degrades; `AttributeError` in production for a typo. Mitigation is *not* "be careful" — it's type hints + a checker in CI + tests at the boundaries. A 4 frames this as a tradeoff they actively manage, not a flaw they tolerate.

**13. Runtime flexibility tradeoffs** — monkeypatching, attribute injection, and dynamic imports make testing and adapters easy but destroy local reasoning: you can no longer tell what a class does by reading it. Practical rule: dynamism is acceptable *at boundaries and in tests*, dangerous in core domain logic. Library code that monkeypatches the host application is a serious smell.

**14–16. Attribute lookup order** — for `obj.x`:
1. `type(obj).__mro__` searched for a **data descriptor** (`__get__` + `__set__`/`__delete__`) → wins
2. `obj.__dict__['x']` → instance dict
3. `type(obj).__mro__` for a **non-data descriptor** (e.g. functions → bound methods) or a plain class attribute
4. `__getattr__` on the type, if defined
5. `AttributeError`

All of that is orchestrated by `__getattribute__`. The **class dict** holds things shared by all instances (methods, class attributes, descriptors); the **instance dict** holds per-object state. An instance attribute shadows a plain class attribute but **not** a data descriptor — which is precisely why `@property` can't be clobbered by assigning to the instance. See Appendix A #16–17.

**17. `__init_subclass__` `[St]`** — a hook called on the *parent* whenever a subclass is defined, receiving the new class. Use it for subclass registration, validating that required attributes exist, or auto-wiring plugins. It replaces the most common metaclass use case with an ordinary classmethod — no custom metaclass, no metaclass conflicts in multiple inheritance. *Probe:* "When do you still need a real metaclass?" (When you must control the namespace *during* class body execution via `__prepare__`, or change the class object's own type.)

---

## Section 2 — Data Types, Mutability, Collections

**18–20. Mutability** — see Appendix A #3. For Q20, push for a *real* bug: the shared-default accumulator, the class-level `[]` shared across instances, a cached config dict mutated by a caller, or a list passed to a function that sorted it in place. A candidate who can't produce one from experience is reciting.

**21–22. Tuple vs list** — beyond immutability: tuples signal *fixed structure* (a record — heterogeneous, positional meaning) while lists signal *homogeneous sequence of varying length*. Tuples are hashable **if all elements are**, so they work as dict keys; they're slightly smaller and faster to construct. A tuple containing a list is unhashable — `hash((1, [2]))` raises — because hashing recurses into elements. That's the cleanest demonstration that immutability is shallow.

**23–24. Set vs list, dict vs list of tuples** — set when you need membership testing (O(1) vs O(n)), dedup, or set algebra, and order doesn't matter. Dict when you look up *by key*; a list of tuples forces an O(n) scan. The senior addition: both require hashable elements, and a list-of-tuples is sometimes still right when you need order, duplicates, or unhashable keys.

**25. Why keys must be hashable** — a dict stores entries in buckets indexed by `hash(key)`. Without a stable hash it cannot find the entry again. Hence the real requirement is **hash stability for the key's lifetime**, not immutability per se. See Appendix A #15.

**26. Mutating while iterating** — for a list, iteration is index-based, so removal shifts elements and silently skips them *(Snippet S3)*. For a dict or set, it raises `RuntimeError: dictionary changed size during iteration` — a louder and better failure. Safe approaches: build a new collection via comprehension (preferred), iterate over a copy (`for x in list(d)`), or collect keys to delete and remove afterwards.

**27. Memory and cost** — list: dynamic array, O(1) index and append (amortized), O(n) insert/delete at the front, over-allocates on growth. Tuple: fixed array, smaller, no growth slack. Set/dict: hash tables, O(1) average membership and lookup, O(n) worst case on pathological collisions, noticeably more memory per element; dicts have been compact (split keys/values) since 3.6, which also gives them insertion order.

**28. `remove` / `discard` / `pop`** — `list.remove(v)` deletes the first matching *value*, raising `ValueError` if absent; `list.pop(i)` removes by *index* and returns it (default last). `set.remove(v)` raises `KeyError` if absent; `set.discard(v)` does not — that's the entire distinction and it's the "EAFP vs no-op" choice. `dict.pop(k[, default])` removes by key and returns the value, raising only if no default is given. `set.pop()` removes an arbitrary element.

**29. Slicing** — returns a **new object of the same type**: a shallow-copied list, or a new string. Out-of-range bounds clamp silently rather than raising (unlike indexing), which hides bugs. Supports a step, and `[::-1]` reverses. See Q329 for the quadratic trap.

**30. `deque` vs `list`** — `deque` gives O(1) append/pop at *both* ends and supports `maxlen` for a bounded ring buffer; `list` is O(n) at the front. `deque` has O(n) random access, so it's wrong when you index frequently. Use `deque` for queues, sliding windows, and recent-N buffers.

**31. `defaultdict`** — great for grouping (`defaultdict(list)`) and counting (`defaultdict(int)`). **The trap:** a plain *read* of a missing key inserts it, so membership checks, `len()`, and iteration silently mutate the dict. Also, passing one to code expecting a `dict` can mask `KeyError` bugs. Use `dict.setdefault` or `dict.get` when you only want a default on read, not insertion.

**32. `Counter`** — when you're counting hashables and want `most_common`, counter arithmetic (`+`, `-`, `&`, `|`), or a missing key to read as `0` without inserting. A plain dict is better when you need strict `KeyError` behavior or non-count values.

**33. Dict ordering** — insertion order is **guaranteed by the language** since 3.7 (an implementation detail in 3.6). What it does **not** give you: sorted order, order-insensitive equality (two dicts with different insertion order still compare equal), or stable ordering after deletes and reinsertions — reinserting a deleted key moves it to the end. Use `OrderedDict` when you need order-*sensitive* equality or `move_to_end`.

**34. Aliasing surprises** — `b = a` then mutating `b`; a mutable default; a class attribute; a nested structure shallow-copied; the same list stored under several dict keys; `[[0]*3]*3` *(Snippet S4)*. The tell is always: one object, several names.

**35. `insert(0, x)` in a loop `[St]`** — each insert shifts every existing element right, so it's O(n) per call and O(n²) overall. Use `deque.appendleft`, or append and reverse at the end.

**36. Dict resize `[St]`** — dicts grow by reallocating and rehashing every entry when they exceed about two-thirds full, so building a very large dict incurs repeated O(n) rehash work (amortized O(1) per insert, but with latency spikes and transient 2× memory). If the size is known, building from a generator with `dict(...)` or pre-sizing via a comprehension avoids some churn; for truly huge mappings, reconsider the data structure.

---

## Section 3 — Functions and Arguments

**37. First-class functions** — functions are objects: assignable, storable in containers, passable as arguments, returnable, and attribute-carrying (`fn.custom = 1`). They have `__name__`, `__doc__`, `__defaults__`, `__closure__`. That's what makes decorators, callbacks, and dispatch tables possible.

**38–39. Argument kinds** — positional-only (`/`), positional-or-keyword, var-positional (`*args`), keyword-only (`*`), var-keyword (`**kwargs`), in that order. Positional arguments bind by position; keyword by name. Keyword arguments at the call site improve readability for booleans and numbers (`resize(img, upscale=True)`).

**40. Positional-only `/` `[S]`** — parameters before `/` cannot be passed by name. API authors want this to keep parameter *names* out of the public contract (so renaming isn't a breaking change), to match C-implemented builtins, and to allow `**kwargs` to contain a key that collides with a parameter name.

**41. Keyword-only `*` `[S]`** — forces call sites to name the argument. Use for optional flags and anything where a bare literal would be unreadable (`sorted(data, reverse=True)`), and to allow adding parameters later without breaking positional callers. This is one of the highest-value API habits in Python.

**42–43. `*args` / `**kwargs` and forwarding** — in a *definition* they collect extra positional/keyword arguments into a tuple/dict; at a *call site* `*`/`**` unpack. Forwarding mistakes: losing the signature for introspection and type checkers (needs `ParamSpec`); swallowing typos silently — a misspelled keyword lands in `kwargs` instead of raising `TypeError`; mutating the `kwargs` dict that you then pass on; forgetting `functools.wraps`; and ordering bugs when mixing with explicit parameters.

**44–45. Defaults and mutable defaults** — evaluated **once** at definition time and stored in `fn.__defaults__`. See Appendix A #4. Fixes: `None` sentinel (standard), `field(default_factory=...)` for dataclasses, or a frozen/immutable default like a tuple.

**46. Annotations vs enforcement** — annotations are metadata stored in `__annotations__`; Python does **not** check them at runtime. Enforcement requires an external checker (mypy, pyright) or a runtime validator (pydantic, `typeguard`). A 4 notes that annotations can even be arbitrary expressions, and that `from __future__ import annotations` makes them strings, which breaks naive runtime introspection.

**47–48. Signature design / "Pythonic"** — few positional parameters; keyword-only for options; no boolean traps (prefer two functions or an enum over `do(x, True, False)`); sensible immutable defaults; consistent return types (don't return a value *or* `None` *or* a list); verbs for functions; accept the broadest reasonable input type and return the most concrete. Pythonic at the function level means: do one thing, use the protocols the language already has, raise rather than return error codes, and don't surprise the caller.

**49. Tuple vs dict vs object return** — tuple: fine for 2–3 values consumed immediately; positional meaning degrades fast, and adding a field breaks unpacking. Dict: flexible, self-describing keys, but no schema, typos are silent, and no IDE support. Object/dataclass/NamedTuple: self-documenting, typed, extensible — the right default past two fields. `NamedTuple` is a good middle ground because it's both a tuple and named.

**50. Raise vs `None` vs sentinel** — raise when the caller cannot reasonably continue or when it's a programming error; return `None` when "absent" is a normal, expected outcome the caller will obviously check. Use a **sentinel** when `None` is itself a valid value:
```python
_MISSING = object()          # or: class _Missing: pass; _MISSING = _Missing()
def get(key, default=_MISSING): ...
```
Compare with `is`. A 4 mentions giving the sentinel a `__repr__` for debuggability, and that `enum` or `typing.Optional` may express intent better.

**51. `lambda` vs `def`** — a lambda is a single *expression* with no statements, no annotations, no docstring, and `__name__ == '<lambda>'` (which makes tracebacks and profiler output worse). Use it only for trivial inline callables — `key=`, a default factory. If you're naming it (`f = lambda x: ...`), just use `def`; PEP 8 says so explicitly.

**52. Higher-order functions in practice** — decorators, `key=` functions, callbacks and event handlers, retry/backoff wrappers, dependency injection via callables, `map`/`filter` (though comprehensions usually read better), strategy patterns as plain functions instead of classes.

**53. `partial` vs lambda** — `functools.partial` pre-binds arguments and returns a `partial` object that is **introspectable** (`.func`, `.args`, `.keywords`), picklable, and has no late-binding problem — it captures values at creation time, whereas a lambda captures the *variable* and re-reads it when called. That last point is the real answer: in a loop, `partial(f, i)` is correct while `lambda: f(i)` is the late-binding bug *(Snippet S2)*.

---

## Section 4 — Scope, Namespaces, Closures

**54–55. Namespaces and LEGB** — a namespace maps names to objects; most are dicts. Name resolution searches **Local → Enclosing → Global (module) → Built-in**, and stops at the first hit. Note class bodies are *not* an enclosing scope for methods — a method cannot see class-level names without `self.`/`ClassName.`, which surprises people.

**56. `global` vs `nonlocal`** — both are only needed to **rebind**, never to read. `global` rebinds at module level; `nonlocal` rebinds in the nearest enclosing *function* scope and fails at compile time if there isn't one. Heavy use of `global` is a design smell — it's usually a missing class or parameter.

**57–58. Closures and late binding** — see Appendix A #6 and Snippet S2.

**59. Closures vs classes for state** — closure when there's one operation and a little state (a counter, a configured callback); the state is private and there's less ceremony. Class when there are multiple operations, when the state should be inspectable in a debugger or `repr`, when you need serialization, or when it should be subclassable. A 4 notes that closure state is *hard to introspect*, which hurts debugging — that's the main practical cost.

**60. Production closure uses** — decorators (the canonical case), configured callbacks and `key=` functions, memoization wrappers, factory functions that bake in configuration, and deferred computation. Anywhere you'd write a one-method class.

**61. Shadowing builtins** — `list = [1,2]`, `id`, `type`, `input`, `sum`, `filter`, `dict`, `str`. The bug surfaces *later*, when something else in the same scope calls the builtin and gets a `TypeError: 'list' object is not callable`. Especially nasty at module level, where it poisons the whole module. Linters flag it (`A001`/`W0622`).

**62. Cell variables `[St]`** — when an inner function references an enclosing local, CPython promotes that variable to a **cell**: a small indirection object shared between the frames. `fn.__closure__` is a tuple of those cells; `cell.cell_contents` reads the current value. This is exactly *why* late binding happens — the closure holds the cell, not a snapshot. `fn.__code__.co_freevars` names them.

**63. "Name binding" as a mental model `[St]`** — "variables store values" predicts copying on assignment and breaks immediately for mutables. "Names are bound to objects" correctly predicts aliasing, mutation visibility, call-by-sharing, late binding in closures, and why `+=` differs between lists and tuples. A candidate who has internalized this derives behavior instead of memorizing cases — the single clearest marker of a strong Python mental model.

---

## Section 5 — Comprehensions

**64. List comp vs genexp** — a list comprehension builds and returns the whole list eagerly; a generator expression returns a lazy iterator producing items on demand. Syntax differs only in brackets. Memory: O(n) vs O(1); the genexp is single-use.

**65. Comprehension vs loop** — a comprehension is clearer when it's a direct *map/filter* producing one collection. It's worse when it has multiple `for` clauses plus conditions, when the body needs a statement (try/except, logging), when it spans more than a line or two, or when it exists purely for side effects — which is an abuse; use a `for` loop.

**66. Nested comprehensions** — the clause order reads like nested loops (outer first) but the *expression* comes first, which inverts normal reading order. Two levels with a condition is usually the limit. For a matrix, `[[f(x) for x in row] for row in rows]` is fine; three levels or a flatten-plus-filter is better as a loop or a generator function.

**67. Set and dict comprehensions** — `{expr}` and `{k: v}`. Same semantics, different container, and both deduplicate by hash — which can silently shrink the result, a real gotcha when the key expression isn't unique.

**68. Genexp memory** — the generator holds one item plus its frame state instead of the full result, so memory is O(1) regardless of input. **Not** more efficient when: the consumer materializes it anyway (`sorted()`, `len()`, `random.choice`), you need multiple passes, the input is tiny (the per-item overhead dominates), or you need indexing. `sum(x for x in it)` and `sum([x for x in it])` differ in memory but the list version can be marginally faster for small inputs.

**69. Consuming a genexp twice** — the second consumption yields **nothing** and raises no error, which is the dangerous part. `list(g)` then `len(list(g))` gives the right answer then `0`. If you need two passes, materialize once into a list or use `itertools.tee` (which buffers, so it isn't free).

**70. Comprehension scope `[S]`** — in Python 3, a comprehension has **its own scope**, so the loop variable does not leak into the enclosing namespace. In Python 2, list comprehensions leaked (genexps didn't), which silently clobbered existing names. Consequence in 3: a comprehension at class scope can't see other class-level names (except the leftmost iterable), which surprises people writing class-body comprehensions.

**71. Comprehension hurting maintainability** — the realistic example is a single comprehension doing filtering, transformation, and a nested lookup at once, which then needs error handling for one bad row — and can't get it without being rewritten as a loop. A 4 describes rewriting such a line *back* into a loop during review and the readability win.

**72. Walrus in comprehensions** — genuinely good when it avoids calling an expensive function twice: `[y for x in xs if (y := f(x)) is not None]`. Showing off when it's used to cram an accumulator or side effect into a comprehension, or to save a trivial line. See Q276.

---

## Section 6 — Iterables, Iterators, Generators

**73–76. The protocol** — see Appendix A #7. Summary: **iterable** = has `__iter__` returning a fresh iterator. **Iterator** = has `__next__` *and* `__iter__` returning `self`; stateful, single-use, raises `StopIteration` when exhausted. `iter(x)` obtains an iterator (and also supports the two-argument `iter(callable, sentinel)` form); `next(it[, default])` advances it.

**77. Custom iterator vs `__iter__` as a generator** — the class form needs `__iter__` and `__next__` with explicit state. **The generator form is almost always better**: `def __iter__(self): yield from self._items` gives you a *fresh* iterator per call automatically, so the object stays re-iterable, and the state machine is written for you. Write a raw `__next__` only when you need to expose iterator-specific methods or control memory precisely. *Probe:* "What breaks if `__iter__` returns `self` and holds position on the instance?" (It becomes single-use; nested loops over the same object interfere.)

**78–81. Generators: value and cost** — see Appendix A #8. For Q80 specifically, from the **caller's** perspective: a list can be indexed, `len()`-ed, iterated repeatedly, and printed; a generator can't do any of those, and the work happens during *their* loop, so exceptions and latency appear at consumption time, not at the call. For Q81, streaming: constant memory over arbitrarily large input, work starts before input ends, and pipelines compose — `sum(parse(line) for line in file)` never materializes the file.

**82. `yield from`** — see Appendix A #9.

**83. `send` / `throw` / `close` `[St]`** — `send(v)` resumes the generator making the paused `yield` expression evaluate to `v` (the generator-as-coroutine pattern); `throw(exc)` raises an exception *at* the suspended `yield`; `close()` raises `GeneratorExit` there to trigger cleanup. Honest senior answer: **rarely needed in application code** — `async`/`await` superseded the coroutine use case. Legitimate uses remaining: `contextlib.contextmanager` (which uses `throw` to deliver the exception into your `try`), and accumulator/pipeline generators.

**84. Generators with side effects** — the side effect fires *when consumed*, not when created, so ordering is surprising; if the consumer stops early or never iterates, the effect never happens; partial consumption leaves things half-done; and two consumers can't both get the effect. Rule: generators should be **lazy producers**, not drivers of I/O or state change.

**85. Lazy vs eager** — lazy computes on demand, enabling infinite sequences, constant memory, and early termination. Prefer **eager** when: the data is small, you need multiple passes, you need length or indexing, you want errors surfaced immediately at the call site, you need a stable snapshot of mutable source data, or the laziness makes debugging disproportionately hard.

**86. Generators and debugging** — the traceback points at the consumption site, not where the pipeline was built, so the stack doesn't show you the logical origin. You can't print a generator's contents without consuming (and destroying) it. Breakpoints fire in an order that doesn't match the source reading order. Exceptions surface late and far from the cause. A 4 mentions temporarily wrapping in `list()` as a debugging technique.

**87. Abandoned generator and `finally` `[St]`** — when a suspended generator is garbage-collected, Python calls `close()`, which raises `GeneratorExit` at the `yield`, so the `finally` **does** run — but only at **collection time**, which is non-deterministic and may be at interpreter shutdown, or never if the generator is in a reference cycle. Therefore: never rely on a generator's `finally` to release a lock, close a file, or end a transaction. Use `with` inside the generator, or `contextlib.closing` around it.

---

## Section 7 — Decorators

**88–91.** See Appendix A #10 and #11. Syntax mapping: `@d` → `f = d(f)`; `@d(arg)` → `f = d(arg)(f)`; stacked decorators apply **bottom-up** and execute **top-down**.

**92. Production uses** — cross-cutting concerns that are genuinely orthogonal to the function's logic: logging/tracing, timing and metrics, retry with backoff, caching (`lru_cache`), authentication and permission checks, input validation, feature flags, deprecation warnings, registration into a routing table, transaction or lock management. The test for "should this be a decorator": would you want it on *many* functions, and is it invisible to the function's own logic?

**93–94. Decorator debuggability** — each decorator adds a stack frame, so tracebacks get noisy and all point at `wrapper`; without `wraps` every function looks identical in profiler output; behavior depends on stack *order*, which isn't obvious from reading; and the decorator can swallow exceptions or alter return values invisibly. Mitigation: always `functools.wraps`; keep decorators tiny; expose the original as `__wrapped__`; make them no-op-able via config.
To debug a stack of three: **bisect** — remove decorators one at a time and re-test. Call `fn.__wrapped__` to reach the original. Check order dependence by swapping two. Inspect `inspect.unwrap(fn)` and `inspect.signature(fn)`.

**95. Decorator for both functions and methods** — a plain `*args, **kwargs` wrapper works for both, because by the time the wrapper runs, `self` is just the first positional argument. Problems arise when the decorator tries to *introspect* arguments by name, or when it's applied **above** `@classmethod`/`@staticmethod` — those must be the **outermost** decorator, because the objects they produce aren't callable in the normal way on older versions. For a decorator that needs the instance, use a descriptor-based (`__get__`) implementation.

**96. Class decorators vs metaclasses `[St]`** — a class decorator receives the already-built class and modifies or replaces it. A metaclass participates in *creating* the class and is **inherited** by subclasses. Use a decorator for one-off, non-inherited modification (`@dataclass`, `@total_ordering`, registration) — it's simpler and composes. Use a metaclass only when subclasses must also be affected and `__init_subclass__` isn't enough.

**97. Signature-preserving decorator `[St]`** — `functools.wraps` sets `__wrapped__`, and `inspect.signature` follows it, so runtime introspection already works. What *doesn't* work is **static** typing — checkers see `(*args, **kwargs)`. The fix is `ParamSpec`:
```python
P = ParamSpec("P"); R = TypeVar("R")
def deco(fn: Callable[P, R]) -> Callable[P, R]: ...
```
For stricter runtime needs, `decorator` library or manually building a signature via `inspect.Signature`.

---

## Section 8 — Context Managers

**98–102.** See Appendix A #12. Key facts: `__exit__(exc_type, exc_value, traceback)` receives `(None, None, None)` on the success path; **returning truthy suppresses the exception**; the `@contextmanager` generator must wrap `yield` in `try/finally` or cleanup is skipped on error.

**103. Real-world uses** — database transactions (commit/rollback), locks, temporary directories and files, changing the working directory, patching in tests, timing blocks, opening/closing network connections, `decimal.localcontext`, suppressing warnings, redirecting stdout, acquiring semaphores, and setting/restoring any global state.

**104. Common mistakes** — forgetting `try/finally` around `yield`; accidentally returning a truthy value from `__exit__` and silently swallowing exceptions; doing work in `__init__` instead of `__enter__` (which breaks reuse and means failure happens before the `with`); returning the wrong object from `__enter__` (not everything should `return self` — `open()` returns the file, a transaction might return a cursor); assuming re-entrancy or reusability when the implementation isn't; and raising a *new* exception inside `__exit__`, which masks the original.

**105. `ExitStack` `[St]`** — manages a **dynamic** number of context managers: entering a list of files, or conditionally acquiring resources. `enter_context()` registers each with correct LIFO unwinding; `callback()` registers arbitrary cleanup; `pop_all()` transfers ownership so cleanup can be deferred — the idiom for "build an object that owns several resources, and only commit on success."

**106. Async context managers `[St]`** — `__aenter__`/`__aexit__`, used with `async with`, so setup/teardown can **await**. Needed when acquiring the resource is itself I/O: connection pools, async DB transactions, `aiohttp` sessions, async locks. You cannot use a normal `with` for these — blocking in `__enter__` would stall the event loop *(Snippet S10)*. `contextlib.asynccontextmanager` is the generator form.

---

## Section 9 — Exceptions

**107. How it works** — `raise` unwinds the stack until a matching `except` is found, running `finally` blocks on the way; unmatched exceptions reach the top and print a traceback. All exceptions derive from `BaseException`; application errors derive from `Exception`. `except` matches by `isinstance`, so a base class catches subclasses.

**108. Broad catching** — `except Exception` catches typos (`NameError`), bugs (`TypeError`, `AttributeError`), and `KeyboardInterrupt`-adjacent control flow, converting crashes into silent wrong behavior *(Snippet S11)*. **Acceptable** at a genuine top-level boundary: a request handler, a worker loop, a plugin host, a CLI entry point — where the goal is "log it and keep serving." Even there it must **log with traceback** and usually re-raise or return a 500.

**109–110. `else` and `finally`** — `else` runs only if the `try` block raised nothing, and it keeps the "success path" out of the `try` so you don't accidentally catch exceptions from it. `finally` always runs — on success, exception, `return`, or `break`. **A `return` in `finally` discards the pending exception and any pending return value** *(Snippet S12)* — a genuine data-loss pattern that Python 3.14 now warns about (PEP 765).

**111. Designing custom exceptions** — define one base per package/domain (`class PaymentError(Exception)`), then specific subclasses. This lets callers catch at whatever granularity they need, and lets you add new error types without breaking `except PaymentError`. Carry structured data as attributes (`order_id`, `status`), not just a message string. Don't subclass builtins unless the semantics genuinely match. You need a hierarchy as soon as callers would want to distinguish causes.

**112. Chaining / `raise ... from`** — when you raise inside an `except`, Python sets `__context__` automatically → "During handling of the above exception, another exception occurred." `raise New() from original` sets `__cause__` → "The above exception was the direct cause." Use `from` when deliberately translating a low-level error into a domain error, preserving the original for debugging. `raise ... from None` suppresses the chain — appropriate when the inner exception is noise that would confuse users.

**113. Fail fast vs handle locally** — fail fast when the error means your assumptions are broken, when continuing would corrupt data, or when the local code has no information to make a good decision. Handle locally when you can genuinely recover (retry a transient network call, fall back to a cached value) and the fallback is a deliberate product decision. The anti-pattern is catching an exception just to log it and continue with invalid state.

**114. Poor exception design** — bare `except:`; `except Exception: pass`; using exceptions for ordinary control flow in hot paths; a single generic `AppError` for everything; messages with no context ("failed"); catching and re-raising without adding value; returning error codes alongside exceptions inconsistently; swallowing in `finally`; and catching broadly in a *library*, which denies the application the chance to decide.

**115. Useful but safe messages** — include what operation failed, the identifiers needed to find the record (order ID, user ID — not the user's data), and the expected vs actual where cheap. Exclude: passwords, tokens, API keys, PII, full request bodies, raw SQL with parameters. Attach structured fields for logs rather than concatenating into the string. Remember exception messages often reach users and error trackers.

**116. Validation vs usage vs programmer errors** — **Validation**: bad *external* input from a user or upstream system — expected, should produce a clear domain exception and typically a 4xx; often you want to collect *all* of them, not fail on the first. **Usage**: a caller used your API wrongly (bad argument combination) — raise `TypeError`/`ValueError` immediately. **Programmer**: an internal invariant is broken — ideally `AssertionError` or a dedicated internal error; these should alert, not be handled. All three are exceptions, but they differ in who sees them, whether they're logged as errors, and whether they're retried.

**117. Exception groups / `except*` `[St]`** — see Appendix B Q281.

**118. `except BaseException` `[St]`** — `BaseException` includes `KeyboardInterrupt`, `SystemExit`, and `GeneratorExit`, which are *control flow*, not errors. Catching them means Ctrl-C stops working, `sys.exit()` is ignored, and generator cleanup breaks. Catch `Exception` instead. The only legitimate uses are a top-level handler that re-raises after cleanup, and code that must run cleanup on shutdown — and `finally` is the better tool for that.

---

## Section 10 — OOP

**119. Python OOP vs rigid languages** — no true private (only `_name` convention and `__name` mangling); no interfaces or compile-time contracts (duck typing, ABCs, Protocols instead); no method overloading by signature (default args / `singledispatch` instead); everything is public and patchable; multiple inheritance is allowed and resolved by MRO; properties mean you can start with a plain attribute and add behavior later **without changing the call site** — which is why Python code doesn't need getters and setters everywhere.

**120–121. Class vs instance attributes** — class attributes live in the class dict and are shared; instance attributes live in the instance dict and shadow plain class attributes on read. **The failure mode:** `self.items.append(x)` on a class-level `items = []` *mutates the shared list* because it never rebinds, so every instance sees it *(Snippet S5)*. `self.items = [...]` would instead create an instance attribute. Fix: initialize mutable state in `__init__` or use `field(default_factory=list)`.

**122–123. Method types** — instance methods take `self`; `@classmethod` takes `cls` and is **inherited correctly by subclasses**; `@staticmethod` takes neither. Use `@classmethod` for alternative constructors (`from_json`, `from_row`) — critically, `cls(...)` means a subclass gets an instance of *itself*, which a hardcoded class name wouldn't — and for class-level registries or factory dispatch. `@staticmethod` is justified when the function is conceptually part of the class's API and you want it found via the class, but genuinely needs no state; if it's just namespacing, a **module-level function is better** and more testable.

**124. `__new__` vs `__init__`** — see Appendix A #13.

**125. Inheritance** — a subclass's instances are instances of the base; attribute lookup walks the MRO; `super()` delegates to the next class in that order. Python has no `final`, no `virtual`, and no access modifiers — everything is overridable.

**126. Composition over inheritance** — prefer composition when the relationship is "has-a" or "uses-a" rather than "is-a"; when you'd otherwise inherit to reuse code rather than to be substitutable; when you need to vary behavior at runtime; when the base class has state or methods the subclass doesn't want; or when you're reaching for multiple inheritance to mix unrelated capabilities. Inheritance couples you to the parent's *implementation*, not just its interface — the fragile base class problem.

**127. `super()` and zero-arg form** — see Appendix A #14. The zero-argument form is compiler magic: inside a class body, the compiler adds an implicit `__class__` closure cell and `super()` reads it plus the first positional argument, equivalent to `super(__class__, self)`. That's why it only works lexically inside a class body and fails in a nested function that doesn't close over it.

**128. MRO and C3** — see Appendix A #14. C3 linearization guarantees: a class precedes its parents, parent order is preserved left-to-right, and the result is monotonic (consistent across the whole hierarchy). If no such order exists, the `class` statement raises `TypeError` — a *compile-time* guarantee that the lookup order is coherent.

**129. Multiple inheritance risks** — ambiguity about which implementation wins; fragile cooperative chains where one class forgetting `super()` silently breaks the rest; `__init__` signatures that don't compose (the classic pain point — every class must accept and forward `**kwargs`); and diamond state being initialized twice or not at all. Reasonable for **mixins** that are stateless and orthogonal; risky for combining two stateful concrete classes.

**130. Mixins** — small, focused classes providing one capability, not meant to be instantiated alone, ideally stateless, and placed **to the left** of the base class so they precede it in the MRO. Good: `JSONSerializableMixin`, `TimestampMixin`, `ComparableMixin`. Bad: a mixin that requires the host class to define specific attributes without saying so, or mixins that depend on each other's ordering.

**131. Easy to extend and test** — dependencies injected rather than constructed internally; no I/O in `__init__`; small focused methods with clear seams; state exposed enough to assert on; a useful `__repr__`; prefers composition so you can swap a collaborator with a fake; avoids class-level mutable state and global singletons; and doesn't do work at import time.

**132. Diamond `super()` and cooperative inheritance `[St]`** — surprising because `super()` follows the *instance's* MRO, so a class's `super()` can land on a sibling it never heard of *(Snippet S7)*. To make it work: every class in the chain calls `super()`; signatures are compatible, typically by accepting and forwarding `**kwargs`; the root base absorbs remaining arguments and calls `super().__init__()` (eventually `object`); and no class assumes it's last. If you can't satisfy those, use composition instead.

---

## Section 11 — Dunder Methods

**133. Dunders and the data model** — double-underscore methods are the hooks Python's syntax and builtins dispatch to: `len(x)` → `__len__`, `x[k]` → `__getitem__`, `x + y` → `__add__`, `with x` → `__enter__`/`__exit__`, `for x in y` → `__iter__`. They're how you make your own types behave like built-in ones. Crucially, **implicit dunder lookups go through the type, not the instance** — setting `obj.__len__` on an instance does nothing.

**134. `__repr__` vs `__str__`** — `__repr__` is for **developers**: unambiguous, ideally `eval`-able or at least showing the class and key fields — `User(id=42, email='a@b.com')`. `__str__` is for **end users**: readable, possibly lossy. If you only write one, write `__repr__`, because `__str__` falls back to it (not vice versa) and because `repr` is what appears in lists, dicts, debuggers, and logs. A 4 mentions that a missing `__repr__` is a real debugging tax — `<obj at 0x7f...>` tells you nothing — and that `__repr__` must never raise.

**135. `__len__` vs `__bool__`** — see Q10–11. `__bool__` wins; `__len__` is the fallback. Defining `__len__` on something that isn't a collection silently makes it falsy when "empty".

**136. `__contains__`** — implement it for `in` when you can answer membership faster than a linear scan, or when membership means something domain-specific (a date in a range, an IP in a subnet). **If you don't define it**, Python falls back to iterating via `__iter__` (or `__getitem__` with integer indices), which is O(n) — and *silently works*, so the performance bug is invisible.

**137. Rich comparisons and `NotImplemented`** — `__lt__`, `__le__`, `__eq__`, `__ne__`, `__gt__`, `__ge__`. If a method returns the **`NotImplemented` singleton**, Python tries the *reflected* operation on the other operand (`a < b` → `b > a`); if that also returns `NotImplemented`, it raises `TypeError` (or, for `==`, falls back to identity comparison). This is why you return `NotImplemented` rather than `False` for unknown types — `False` claims a definitive answer and breaks symmetric comparison with types that could have handled it.

**138. Designing equality for a domain object** — decide first whether the object has **identity semantics** (a `User` row — equal iff same ID) or **value semantics** (a `Money`, a `Point` — equal iff all fields equal). Then: compare only the defining fields, guard the type (return `NotImplemented` for others), keep `__hash__` consistent with it, and make hashed fields immutable. Don't include derived or cached fields, timestamps, or mutable collections.

**139. Ordering and `total_ordering`** — define `__lt__` plus `__eq__`, then `@functools.total_ordering` generates the remaining four. It's a convenience with a small performance cost (extra calls) and it only works if your ordering is consistent. Only define ordering if there is a *natural* total order — for anything else, pass an explicit `key=` to `sorted` instead of pretending the type has one.

**140. When to make an object immutable** — when it's a value object, when it's used as a dict key or set member, when it's shared across threads, when it's part of a cache, or when accidental mutation would be a serious bug (money, config, coordinates). `frozen=True` dataclasses are the cheap route. Cost: every "change" allocates a new object, and `dataclasses.replace` becomes the update idiom.

**141. Risks of too many magic methods** — operator overloading with non-obvious meaning (`+` on a user object: merge? concatenate?); implementing `__getattr__` and losing `AttributeError` on typos; making something iterable or indexable that isn't conceptually a container; implicit conversions via `__int__`/`__index__` that hide bugs. Each magic method is an implicit contract readers must learn. Implement them to satisfy a protocol the language already defines, not to look clever.

**142. `__call__` / `__getitem__` DSLs `[St]`** — they enable fluent, terse APIs: `query['users'].filter(...)`, a configured object invoked like a function, path-building with `/`. Legitimate where the domain genuinely has notation (pathlib, numpy, ORM query builders, Decimal arithmetic). A **mistake** when it obscures what's happening, makes errors late and confusing, defeats IDE autocomplete and type checking, or exists to save a few characters. The honest senior test: can a new team member read it without reading your source?

---

## Section 12 — Copying, Equality, Hashing

**143–146.** See Appendix A #5. Specifics: `copy.copy` creates a new outer object sharing the same element references; `copy.deepcopy` recursively copies, using a **memo dict** keyed by `id()` — which is what makes it handle **circular references** correctly rather than recursing forever. The memo also means shared references inside the graph stay shared in the copy (two attributes pointing at the same list still point at one list afterwards), which people often don't expect.

**144 specifically — shallow-copy bugs** — copying a dict of lists and mutating one list "in the copy", which changes the original; copying a config object and mutating a nested dict; `list(rows)` then sorting each row in place; passing a shallow copy to a function "for safety" and still having it mutate your nested data. The symptom is always "I copied it, why did the original change?"

**147–149.** See Appendix A #15. For Q149, the demonstration is `Snippet S6`: put an object in a set, mutate a hashed field, and `obj in s` becomes `False` while `obj in list(s)` stays `True` — the object is in the container but in the wrong bucket. Python cannot detect this; it's a silent data-structure corruption.

**150. `__copy__` / `__deepcopy__` `[St]`** — define them when default copying is wrong: the object holds a resource that shouldn't be duplicated (connection, lock, file handle), it has a cache that should be reset rather than copied, or it participates in a registry that must be updated. `__deepcopy__(self, memo)` **must** respect and populate the memo dict — `memo[id(self)] = new_obj` before copying children — or you'll break cycle handling and duplicate shared objects. `__reduce__` affects both pickling and copying, and is sometimes the better single place to define the semantics.

---

## Section 13 — Dataclasses and Lightweight Modeling

**151. When to use a `dataclass`** — when the class is primarily a *bundle of typed fields*. It generates `__init__`, `__repr__`, `__eq__` (and optionally ordering and `__hash__`) from annotations, removing the boilerplate where bugs hide — a hand-written `__eq__` that forgets a field, or a `__repr__` that goes stale. A regular class is better when behavior dominates, when construction is non-trivial, or when you don't want the generated methods.

**152. `frozen=True`** — blocks `__setattr__`/`__delattr__` (raising `FrozenInstanceError`) and makes instances hashable. Benefits: safe as dict keys and set members, safe to share across threads, no spooky mutation, and clearer intent. Tradeoffs: "updates" require `dataclasses.replace(obj, field=new)`, which allocates; `__post_init__` must use `object.__setattr__` to set derived fields; and it's shallow — a frozen dataclass holding a list still has a mutable list.

**153. `default_factory`** — a bare `field: list = []` would share one list across all instances — the mutable-default bug at class level. Dataclasses **detect** this and raise `ValueError: mutable default ... use default_factory` at class creation, which is one of the nicest guardrails in the stdlib. `field(default_factory=list)` calls the factory per instance.

**154. `eq` and `frozen` → `__hash__`** — the rules: `eq=True` (default) with `frozen=False` sets `__hash__ = None` (unhashable), mirroring the normal `__eq__` rule. `eq=True, frozen=True` **generates** a `__hash__` from the fields. `eq=False` leaves the inherited identity hash. You can force one with `unsafe_hash=True`, but the name is a warning: you're promising the hashed fields won't change.

**155. `namedtuple` vs `NamedTuple` vs dataclass** — `collections.namedtuple` is still useful for lightweight records that should behave like tuples: unpackable, indexable, immutable, very memory-efficient, and backward-compatible with code expecting a tuple (great for return values and database rows). `typing.NamedTuple` is the same thing with annotations and class syntax. A **dataclass** is better when you want mutability, methods, defaults with factories, or don't want tuple behavior — and notably, a dataclass is **not** iterable, which prevents accidental unpacking bugs.

**156. When a dict should stay a dict** — when the keys are genuinely dynamic or data-driven (parsed JSON, a config blob, user-defined fields), when it's a short-lived intermediate, when you're just passing it through, or when the schema is externally owned. Promote to a class when the same keys are accessed in many places (typos become silent `KeyError`s), when you want validation or methods, or when the structure is part of your domain.

**157. Over-engineered data models** — a class per JSON node for data you read once; inheritance hierarchies three deep for data with no behavior; getters and setters that only pass through; converting to and from a dozen representations; abstract base classes with one implementation; dataclasses wrapping single values. The test: does the abstraction prevent a real class of bug or enable a real extension, or does it just add a layer?

**158. `__post_init__` `[St]`** — runs after the generated `__init__` for derived fields, validation, or type coercion. Pitfalls: with `frozen=True` you must use `object.__setattr__`; it does **not** run on `copy`/`deepcopy`/unpickle, so invariants established there can be silently skipped; combined with `InitVar` the parameter ordering gets subtle; and heavy validation in it makes construction expensive in hot loops. For serious validation, `pydantic` is the honest answer.

---

## Section 14 — Properties, Descriptors, Attribute Access

**159. What `@property` solves** — it lets an attribute *become* computed later without changing any call site. That's the reason Python code doesn't write getters and setters upfront: you start with `self.x` as a plain attribute and add validation, laziness, or derivation only when needed. Better than a public attribute when you need validation, a computed/derived value, a deprecation shim, or a read-only view.

**160. When properties hurt** — when they hide expensive work (an innocuous-looking `obj.total` triggering a database query is a classic production incident); when they have side effects; when they're used in a hot loop and the call overhead matters; when there are so many that the class's real state is unclear; or when a property raises, making simple attribute access unsafe. Rule: properties should be **cheap and side-effect-free**. If it's expensive, make it a method named `compute_total()` so the cost is visible — or `cached_property`.

**161–164. Descriptors `[St]`** — see Appendix A #16. Additions: a descriptor's `__set_name__(owner, name)` (3.6+) gives it the attribute name automatically, which removes the old boilerplate of passing the name in. Use a descriptor over a property when the same managed-attribute logic repeats across **several attributes or classes** — typed/validated fields, unit conversion, lazy loading. Below about three uses, a property is clearer.

**165. `__getattr__` vs `__getattribute__` `[St]`** — see Appendix A #17.

**166. Custom attribute access and debugging** — typos silently return something instead of raising `AttributeError`; `hasattr` returns `True` for everything, so feature detection breaks; autocomplete and type checking stop working; `repr`/`copy`/`pickle` can recurse infinitely because they probe dunder attributes; and stepping through in a debugger triggers your hook on every inspection. Mitigation: raise `AttributeError` for anything starting with `_`, keep the dynamic surface narrow and documented, and implement `__dir__`.

**167. `cached_property` `[St]`** — computes once per instance on first access and stores the result **in the instance `__dict__`** under the same name, so subsequent reads are plain attribute lookups with zero overhead. Caveats: the class needs a `__dict__` (so it's **incompatible with `__slots__`**); there is **no invalidation API** — you clear it with `del obj.attr` or `obj.__dict__.pop(name, None)`; and it is **not thread-safe for the computation** — concurrent first access can compute twice (the per-instance lock was removed in 3.12 because it serialized unrelated instances). Use it for expensive, deterministic, instance-scoped values.

---

## Section 15 — Imports and Execution Model

**168–170.** See Appendix A #21. Additional detail for Q170: `__name__` is `"__main__"` for the script you ran and the module's dotted path otherwise; the `if __name__ == "__main__":` guard keeps script behavior from firing on import, which matters for testability and is **required** for `multiprocessing` on Windows and macOS spawn, where child processes re-import the main module.

**171. Circular imports** — see Snippet S16. Causes: bidirectional layering (models ↔ services), a shared type pulled from the wrong place, type-annotation-only imports, and over-eager `__init__.py` re-exports. Fixes in order: fix the dependency direction; extract the shared piece; defer with a function-level import; `if TYPE_CHECKING:` for annotations only.

**172. Import-time side effects** — module bodies execute on import, so connecting to a database, reading config, spawning a thread, registering signal handlers, or calling an API at module level means: you cannot import the module in a test without triggering it; CI fails without production credentials; startup time grows with every import; import *order* becomes load-bearing and fragile; and `--help` on your CLI connects to the database. Rule: **module bodies define, they don't do.**

**173. Safe, testable modules** — no I/O or network at import; configuration read inside functions or injected, not at module scope; no mutable module-level state that tests have to reset; side-effect-free imports; a `main()` function behind the `__main__` guard; and minimal import-time work so the module is cheap to import.

**174. Absolute vs relative imports** — absolute (`from myapp.models import User`) is explicit, unambiguous, works when the module is run directly, and is PEP 8's recommendation. Relative (`from .models import User`) is shorter, survives package renaming, and makes intra-package coupling obvious — but only works inside a package and **breaks when the module is run as a script**, which is the error people actually hit (`ImportError: attempted relative import with no known parent package`).

**175. Wildcard imports** — `from x import *` pollutes the namespace with unknown names, silently shadows existing ones (including builtins), makes it impossible to tell where a name came from, breaks static analysis and IDE navigation, and makes the module's dependencies invisible. It also respects `__all__` if defined, which many modules don't set correctly. Acceptable only in a REPL or a deliberate package-level re-export.

**176. Module vs package vs namespace package `[St]`** — a **module** is a single `.py` file. A **package** is a directory with `__init__.py` that can contain modules and subpackages. A **namespace package** (PEP 420) has *no* `__init__.py` and lets a single logical package span multiple directories or distributions — used for plugin ecosystems (`company.plugin_a`, `company.plugin_b` installed separately). The gotcha: an accidentally missing `__init__.py` creates a namespace package silently, which can produce confusing shadowing and import behavior.

**177. Lazy imports `[St]`** — import inside the function that needs it, or use a module `__getattr__` (PEP 562) for lazy attribute-level imports. Worth it for: heavy optional dependencies (pandas, torch) that most code paths don't use; CLI startup time; and breaking an import cycle. Costs: the import cost moves to first call (bad for latency-sensitive paths), `ImportError` surfaces late rather than at startup, and it hides dependencies from static analysis. Do it deliberately, with a comment, not as a default style.

---

## Section 16 — Typing

**178. What hints solve / runtime limits** — they document intent in a machine-checkable way, catch a real class of bugs before runtime, make refactoring safer, and dramatically improve IDE navigation and autocomplete. At runtime, Python **ignores them entirely** — they're stored in `__annotations__` and never enforced. Nothing stops `f("str")` on `def f(x: int)`. Enforcement requires mypy/pyright in CI or a runtime validator.

**179. Introducing types pragmatically** — don't boil the ocean. Start with new code and public APIs; add a checker in non-blocking mode and ratchet; type the boundaries first (function signatures) and leave internals inferred; use per-module strictness settings to lock in progress; let `Any` be a temporary escape hatch; prioritize the modules with the most bugs or churn. Tools: `mypy --strict` per-module overrides, `--disallow-untyped-defs` module by module.

**180. `list[int]` vs `Sequence[int]`** — for **parameters**, accept the most general type that works: `Sequence[int]` (or `Iterable[int]`) lets callers pass a tuple, a range, or a custom sequence, and signals that you won't mutate it. `list[int]` demands a list specifically. For **return types**, be concrete: return `list[int]` so callers know exactly what they can do. The principle: *be liberal in what you accept, conservative in what you return.* Also: `list` is invariant, so `list[Derived]` is **not** a `list[Base]`, while the read-only `Sequence` is covariant and is.

**181. `Optional` and `None`** — `Optional[X]` is exactly `X | None`. `def f(x: int = None)` is **wrong** — the annotation says `int` but the default is `None`; it should be `x: int | None = None`. Older type checkers silently inferred the Optional; modern ones flag it. The modern syntax `int | None` (PEP 604) is preferred.

**182. Overly strict typing / noise** — noise: annotating obvious locals; `Any` everywhere (which disables checking while looking typed); enormous `Union`s that should be a `Protocol` or separate functions; deeply nested generics that are unreadable; `cast()` sprinkled to silence errors; typing private helpers to the same standard as public APIs. Real value: public function signatures, dataclass/model fields, return types, `Optional` correctness, and narrowing with `TypeGuard`. A 4 says the goal is *catching bugs and documenting intent*, not type-checker score.

**183. `Protocol` `[St]`** — structural typing: a class satisfies it by having the right methods, with no inheritance and no registration. Better than an ABC at **library boundaries** — you can accept types that existed before yours did, and callers don't take a dependency on your base class. Better than a concrete type because it states only what you actually use (`class HasRead(Protocol): def read(self) -> bytes: ...`). Use `@runtime_checkable` for `isinstance`, with the caveat that it only checks method *presence*, not signatures.

**184. `TypeVar` and `Generic` `[St]`** —
```python
T = TypeVar("T")
class Stack(Generic[T]):
    def push(self, item: T) -> None: ...
    def pop(self) -> T: ...
def first(xs: Sequence[T]) -> T: ...
```
A `TypeVar` links types across a signature so the checker knows the output type depends on the input — which a bare `Any` would lose. `bound=` constrains it (`TypeVar("T", bound=Comparable)`).

**185. Variance `[St]`** — **covariant**: if `Derived` is a `Base`, then `Container[Derived]` is a `Container[Base]` — safe for read-only containers (`Sequence`, `Iterable`, return positions). **Contravariant**: reversed — applies to argument positions (`Callable[[Base], None]` is usable where `Callable[[Derived], None]` is expected). **Invariant**: neither — mutable containers like `list` and `dict`, because allowing either direction lets you insert the wrong type. Where it bites in practice: passing a `list[Derived]` to a function annotated `list[Base]` is rejected, and the fix is to annotate the parameter `Sequence[Base]`.

**186. `ParamSpec` `[St]`** — captures an arbitrary parameter list so a decorator can preserve the wrapped function's exact signature for the type checker, instead of degrading it to `(*args: Any, **kwargs: Any)`. See Q97.

**187. `Any` vs `object` vs `Never` `[St]`** — **`Any`** is the escape hatch: compatible with everything in both directions, and it *disables* checking (any attribute access is allowed). **`object`** is the top type: everything is an `object`, but you can do almost nothing with it without narrowing — it's the *safe* "I accept anything". **`Never`** (`NoReturn`) is the bottom type: no value has it; it annotates functions that never return (always raise or loop forever) and marks unreachable branches for exhaustiveness checking.

**188. `from __future__ import annotations` `[St]`** — makes all annotations **strings** (lazy evaluation, PEP 563), so you can use forward references and newer syntax (`int | None`, `list[int]`) on older runtimes without quoting, and you avoid the runtime cost of evaluating them. The catch: anything that *reads* annotations at runtime — `pydantic`, `dataclasses` with some patterns, `typing.get_type_hints`, DI frameworks — now gets strings and must resolve them, which fails if the names aren't importable at runtime. That tension is why PEP 563 never became the default.

**189. `typing.TYPE_CHECKING`** — a constant that is `False` at runtime and `True` for type checkers. Put imports needed *only* for annotations inside `if TYPE_CHECKING:` to avoid runtime import cost and to break circular imports, pairing it with `from __future__ import annotations` or quoted annotations so the names aren't evaluated at runtime.

---

## Section 17 — Memory and Performance

**190. Refcounting + cyclic GC** — CPython frees an object **immediately** when its reference count hits zero — which is why resources are often released promptly without explicit cleanup, and why CPython's memory behavior feels deterministic compared to the JVM. The cyclic GC exists solely to collect **reference cycles**, which refcounting can never free because the objects reference each other. It runs periodically over three generations, not continuously.

**191. What can form cycles** — any *container* that can hold references: lists, dicts, sets, instances with attributes, closures, frames. Objects that cannot reference others (ints, strings, floats) can't participate. Common real cycles: parent↔child back-references in trees and ORMs, an object holding a callback bound to itself, exception tracebacks referencing frames that reference the exception, and cached objects referencing their cache.

**192–193. Unexpected growth / lingering references** — see Appendix A #22. Three concrete sources worth naming: (1) **module-level caches and registries** that never evict — including `lru_cache(maxsize=None)` and a `_instances = []` class registry; (2) **closures and default arguments** capturing large objects — a decorator holding the last response for "debugging"; (3) **stored exceptions or tracebacks**, which pin every frame and every local in the entire call stack. Others: unclosed generators, accumulating `asyncio` tasks, threads that never exit, and listener/observer lists with no unregister.

**194. `weakref` `[St]`** — a reference that does **not** keep the object alive; when the last strong reference goes, the weakref returns `None`. It solves caches and back-references that shouldn't own their target. `WeakValueDictionary` holds values weakly, so entries vanish automatically when nothing else uses them — a canonical object cache that can't leak. `WeakKeyDictionary` attaches metadata to objects without preventing their collection. `weakref.finalize` registers cleanup that runs on collection, and is the safe alternative to `__del__`. Caveat: not everything is weak-referenceable — `list`, `dict`, and `__slots__` classes without `__weakref__` are not.

**195. Interning** — CPython reuses objects for small ints (−5..256) and automatically interns string literals that look like identifiers, so `a is b` can be `True` for equal values. It surprises people because the behavior differs between the REPL and a module, between literals and computed values, and across implementations — producing "it worked in my test" bugs with `is`. See Q9. `sys.intern` makes it explicit.

**196. `__slots__`** — see Appendix A #24.

**197. Localizing global lookups** — a global/builtin lookup is a dict lookup in the module namespace then builtins, whereas a local is an array index (`LOAD_FAST`). Binding `_len = len` before a tight loop therefore measurably helps — in micro-benchmarks. It's the wrong place to start because it buys a constant factor of a few percent while obscuring the code, and almost every real slowdown is algorithmic or I/O-bound. If this optimization is your bottleneck, you're already in rare territory — and you should have a profile proving it.

**198. Algorithms over micro-optimization** — micro-optimization buys a constant factor; a better algorithm changes the growth curve. Going from O(n²) to O(n) turns a 100× input increase from 10,000× slower into 100× slower — no amount of local caching competes. Python's interpreter overhead also means the win from "clever" Python is small, while the win from using the right data structure (set vs list membership) is enormous.

**199. Reasoning before changing code** — define the metric (p99 latency? throughput? memory?) and the target; measure a baseline reproducibly; profile to find where time actually goes (`cProfile`, `py-spy`); form a hypothesis about *why*; check whether it's CPU, I/O, or memory-bound before choosing a tool; then change one thing and re-measure. The senior habit is refusing to optimize without a measurement — and being willing to conclude the code is fine and the problem is elsewhere.

**200. Premature optimization anti-patterns** — rewriting readable loops as unreadable comprehensions "for speed"; caching things that are cheap, introducing invalidation bugs; adding threads to CPU-bound Python; micro-optimizing a function that runs once at startup; choosing a complex data structure for 50 items; `__slots__` everywhere by default; replacing stdlib calls with hand-rolled versions; and optimizing before the feature is correct or the requirements are stable.

**201. GC generations `[St]`** — new objects start in generation 0; objects surviving a collection are promoted to 1, then 2. Younger generations are collected far more often, on the generational hypothesis that most objects die young. **Tuning**: `gc.set_threshold()` to collect less often, or `gc.freeze()` after startup (moves existing objects out of consideration — valuable before forking, since it prevents GC from touching and copying shared pages). **Disabling** `gc.disable()` is legitimate for short-lived batch processes, or latency-critical request paths where you collect manually between requests — but only if you're confident about cycles, or you'll leak.

**202. `memoryview` `[St]`** — a zero-copy view into an object supporting the buffer protocol (`bytes`, `bytearray`, `array`, numpy arrays). Slicing a `bytes` object copies; slicing a `memoryview` doesn't. It avoids the O(n) copies that make parsing or chunked network I/O accidentally quadratic, and it allows in-place modification of a `bytearray` through the view.

**203. String concatenation in a loop `[St]`** — strings are immutable, so `s += x` allocates a new string and copies both operands each iteration: O(n²) overall. The idiomatic fix is to accumulate in a list and `"".join(parts)` once, which is O(n) and allocates the exact final size. (CPython has an in-place optimization when the refcount is 1, which sometimes hides the problem in micro-benchmarks — relying on it is a mistake, as it's an implementation detail that disappears the moment a second reference exists.) `io.StringIO` is the alternative for incremental building.

---

## Section 18 — Concurrency

**204. The GIL** — see Appendix A #18.

**205. Threads despite the GIL** — the GIL is **released** during blocking I/O (socket, disk, `time.sleep`) and by many C extensions during heavy computation (numpy, zlib, hashlib, image codecs). So threads give real overlap for I/O-bound work, let you use ordinary blocking libraries with no rewrite, and are the right answer for "call 50 HTTP endpoints" in a sync codebase. They also keep a UI or heartbeat responsive alongside blocking work.

**206. Multiprocessing for CPU-bound** — each process has its own interpreter and its own GIL, so work runs truly in parallel across cores. Costs: process startup (expensive on Windows/macOS spawn, which re-imports your `__main__`); **all arguments and results must be picklable** and are copied, so large payloads dominate; memory multiplies by process count; shared state requires `multiprocessing.Manager`, shared memory, or a queue; debugging and tracebacks are harder. Rule of thumb: worth it only when the per-task compute clearly exceeds the serialization cost.

**207. Concurrency vs parallelism** — **concurrency** is structuring a program so multiple tasks are *in progress* and can interleave; **parallelism** is multiple tasks *executing simultaneously* on different cores. In Python: threads and asyncio give concurrency (and parallelism only for I/O waits or GIL-releasing C code); multiprocessing gives true parallelism. Concurrency is a program-structure property; parallelism is an execution property.

**208. Choosing** — see Appendix A #19.

**209. `concurrent.futures`** — a uniform high-level API (`submit`, `map`, `as_completed`) over both pools, so you can switch between thread and process execution by changing one class. `ThreadPoolExecutor` for I/O-bound and GIL-releasing work; `ProcessPoolExecutor` for CPU-bound pure Python. Both as context managers for clean shutdown. Prefer it over raw `Thread`/`Process` objects — it handles lifecycle, result collection, and exception propagation.

**210. Exceptions in a `Future`** — the exception is **captured and stored**, not raised in the worker. You only see it when you call `future.result()` (which re-raises it) or `future.exception()`. **The danger:** if you submit tasks and never check results — fire-and-forget, or only iterating successful ones — failures vanish **silently**. With `Executor.map`, the exception surfaces when you iterate to that element. Always consume results, or use `as_completed` and handle each.

**211. Race conditions the GIL doesn't prevent** — the GIL guarantees one thread runs bytecode at a time, not that your *operation* is atomic. `counter += 1` is `LOAD`, `ADD`, `STORE` — a thread switch between load and store loses an update:
```python
# two threads x 100_000 increments -> final value is usually < 200_000
counter += 1
```
Also non-atomic: check-then-act (`if k not in d: d[k] = v`), read-modify-write on a list element, and multi-step invariants across two containers. Fix with a `Lock`, or use atomic single operations (`list.append`, `deque.append`, `Queue.put`) which *are* effectively atomic.

**212. `Lock` vs `RLock`** — a `Lock` is not reentrant: the same thread acquiring it twice **deadlocks itself**. An `RLock` tracks the owning thread and a recursion count, so the owner can re-acquire and must release the same number of times. You need `RLock` when a locked public method calls another locked method of the same object, or with recursive code. Prefer `Lock` by default — needing `RLock` often signals that the locking boundaries are unclear.

**213. `queue.Queue` over shared state + locks** — the queue encapsulates the locking, so you can't forget it or get the order wrong; it provides blocking semantics and `maxsize` for natural **backpressure**; it gives a clean producer/consumer shutdown story via sentinels or `task_done`/`join`; and it turns a shared-mutable-state problem into a message-passing one, which is far easier to reason about and test. Shared state plus manual locks is where deadlocks and missed-lock bugs live.

**214. What asyncio is good at / worse at** — excellent for very high concurrency over I/O: thousands of sockets, websockets, long-polling, proxies, chat — where a thread-per-connection would exhaust memory on stack space. Also good when you want explicit, inspectable scheduling points. **Worse than threads** when: the ecosystem you need is synchronous (one blocking driver poisons the loop); the work is CPU-bound; the concurrency is modest (a few dozen); or the team isn't fluent — async bugs are subtler and the migration is viral across the whole call stack.

**215. Cooperative vs preemptive** — **cooperative** (asyncio): a task runs until it voluntarily yields at an `await`; the scheduler cannot interrupt it. Predictable — state can't change between your awaits, so you often need no locks — but one misbehaving task blocks everything. **Preemptive** (OS threads): the scheduler can interrupt a thread at almost any point, so no task can starve the others, but state can change between *any* two bytecodes, so shared data needs locks.

**216. Blocking in async** — see Appendix A #20 and Snippet S10.

**217. Coroutine vs task vs future** — a **coroutine** is what `async def` returns when called: an object describing work that has **not started**. Nothing runs until it's awaited or scheduled — forgetting that is the classic "my coroutine never ran" plus a `RuntimeWarning: coroutine was never awaited`. A **Task** wraps a coroutine and schedules it on the loop, so it starts running concurrently right away (`asyncio.create_task`). A **Future** is the lower-level promise of a result that something else will set; `Task` is a subclass of it. Practically: `await coro` runs it sequentially; `create_task(coro)` runs it concurrently.

**218. `asyncio.gather`** — schedules all awaitables concurrently and returns results **in input order** (not completion order). By default, the **first** exception propagates immediately to the caller — but the other tasks are **not cancelled** and keep running, which is a common source of orphaned work and "task exception was never retrieved" warnings. With `return_exceptions=True`, exceptions are returned *as values* in the results list instead of raised, so you always get one entry per input and must inspect each with `isinstance(r, Exception)`.

**219. `TaskGroup` `[St]`** — structured concurrency (3.11+): an `async with asyncio.TaskGroup() as tg:` block that doesn't exit until all child tasks finish. Improvements over `gather`: if any child fails, the others are **automatically cancelled**; all errors are collected into an `ExceptionGroup` rather than only the first surfacing; and tasks cannot outlive the block, so there are no orphans. It's the recommended default for concurrent tasks in modern asyncio.

**220. Cancellation `[St]`** — `task.cancel()` raises `CancelledError` *at* the task's current suspension point. Correct handling: let it propagate. Use `try/finally` or `async with` for cleanup; if you catch it to clean up, **re-raise it** — swallowing cancellation makes a task uncancellable and can hang shutdown. Since 3.8 `CancelledError` inherits from `BaseException`, not `Exception`, specifically so `except Exception:` doesn't accidentally swallow it. Use `asyncio.shield` to protect a critical section, and `uncancel()`/cancel-scopes for advanced cases.

**221. Fire-and-forget GC trap `[St]`** — the event loop holds only a **weak** reference to tasks, so a task created with `asyncio.create_task(...)` whose result you don't keep can be garbage-collected mid-flight and silently stop. The fix is to keep a strong reference: store tasks in a module-level `set` and add a `task.add_done_callback(set.discard)`. `TaskGroup` solves this structurally.

**222. `contextvars` `[St]`** — context-local state that works correctly with both threads *and* asyncio tasks. Thread-locals break under asyncio because many tasks share one thread, so a request ID stored thread-locally leaks across concurrent requests. A `ContextVar` is copied into each task at creation, so each task sees its own value. Used for request IDs, trace/span context, current user, and DB sessions.

**223. Why async is harder to debug** — tracebacks are shorter and lose the logical caller chain across `await` boundaries; a missing `await` silently does nothing; exceptions in un-awaited tasks are swallowed until GC prints a warning; blocking calls cause latency rather than an error *(Snippet S10)*; breakpoints change timing and can mask races; concurrency means log lines interleave; and `pdb` interacts awkwardly with the loop. Mitigation: `loop.set_debug(True)`, structured concurrency, and correlation IDs via `contextvars`.

**224. GIL misconceptions** — "Python can't do concurrency" (it can, for I/O); "the GIL makes Python slow" (it affects multithreaded CPU parallelism, not single-thread speed); "the GIL makes my code thread-safe" (**the most dangerous one** — see Q211); "asyncio bypasses the GIL" (it doesn't — it's single-threaded by design); "multiprocessing is always faster for parallel work" (serialization cost often dominates); "the GIL is part of the Python language" (it's a CPython implementation detail; Jython and IronPython have none).

**225. Free-threaded CPython (PEP 703) `[St]`** — an officially supported build (experimental in 3.13, improving in 3.14) that removes the GIL, enabling true multithreaded parallelism for pure Python. Costs and caveats: some single-threaded performance overhead; C extensions must be rebuilt and declared compatible or the GIL is re-enabled; and **existing race conditions that the GIL's coarse granularity used to hide become far more likely to manifest** — code that was accidentally working may start failing. A 4 notes that it doesn't make unsynchronized code correct; it makes incorrect code fail more often.

---

