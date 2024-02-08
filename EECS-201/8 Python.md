[[2023-03-25]]

### Overview
- Shell scripting is hard
- Traditional compiled high-level languages (C, C++, Java, etc.) tend to have a lot of boilerplate to deal with
	- Very fast, however
- If we want something easy and powerful but don't necessarily need blazing performance
	- Higher level scripting languages
	- Python, Perl, Ruby
	- Tend to be **interpreted**, not needing compilation
	- More expressive

```ad-important
**Definition 8.1**: Python

Python is an interpreted, interactive, object-oriented programming language. It incorporates modules, exceptions, dynamic typing, very high level dynamic data types, and classes. Python combines remarkable power with very clear syntax.

Currently in version 3.
```

There are multiple ways to run and use Python:
- Script
	- Script files can be run via the `python3` command or directly with a shebang (`#!/usr/bin/env python3`)
	- Then `./script.py` after `chmod`
- Interpreter's shell
	- `python3`
- IDE

---

### Python Basics
- Conceptually works much like a **shell script** interpreter
- **Semicolons** not required
- Meaningful **whitespace**
	- Indentations are used to mark the scope of code blocks

Every datum is an **object** (this includes **functions**!).
- Consists of an `ID`, `type`, `value`
- Type determines **mutability**

A variable is a **reference** to a particular object.
- Variables can be assigned via `=`

#### Built-In Types
- `None` - Indicates "lack" of value; analogous to `null`
- **Numbers**
	- `int`, `bool`, `float`
	- `complex`
		- `real` and `imag` components
- **Sequences**
	- Mutable
		- `list`: sequence of arbitrary objects
		- `bytearray`: sequence of 8-bit bytes
			- Created by `bytearray()`
	- Immutable
		- `str` : Sequence of Unicode code points from `U+0000` - `U+10FFF` 
		- `bytes`: Sequences of 8-bit bytes (like a `bytearray` but immutable)
		- `tuple`: like a `list` but immutable
- **Sets**
	- `set`: **Mutable** unordered sets of **unique**, **immutable** objects
	- `frozenset`: **Immutable** sets
- **Mappings**
	- `dict`
- **Callables**
	- Functions. Note that we can also assign variables to them
	- `p = print`, `p('hello')`
		- `__default__`
		- User-defined functions
		- `__self__`
		- Classes: provide new object instances

#### Operators (SOME)
- **Membership**
	- `in`, `not in`
- **Boolean**
	- `not`, `and`, `or`
- **Ternary**
	- `x if C else y`
		- Equivalent to `C ? x : y` in C

#### Comprehensions
"Pythonic" way to create lists, sets, and dictionaries.
- Iterates over an iterable object allowing you to perform operations
	- `{s.name:s.grade for s in students if s.name[0] == 'A'}`

#### Expressions
- `del`: delete
	- Can unbinds variable (s); various classes can overload this for different behaviors
		- `del a`
- `pass`: no-op, used where a statement is needed but you don't want to do anything
- `raise`: raises an **exception**

`try`, `except`, `finally`
Allows you to handle exceptions and perform cleanup.
```python
a=0 
try:
	b = 5 // a  
except ZeroDivisionError:
	print("oopsie") 
finally:
	print("cleanup...")
```

`with`
It adds some convenience factor to `try-except- finally`.
- Can close files for you without your explicitly calling `close()`
```python
with open('somefile.txt', 'r') as f:
	data = f.read()
```

#### Classes
Classes have special functions that you can implement things like "**constructors**" and do the equivalent of operator overloading from C++.
- When classes are called, they run their `__new()__` function to make a new instance, then pass the arguments to the instance's `__init()__`

```ad-note
These `__xxx()__` functions are called "dunder" methods and serve as a way to implement underlying behavior for various things, such as operator overloading.
```

---

### Python Module
To make use of the standard library, you'll have to `import` the module.
- `import sys` will import the `sys` module. Now the module is accessible through the identifier `sys`

You can also have use another identifier for that module.
- `import tensorflow as tf`

```ad-important
**Definition 8.2**: Module and Package

A module is a unit of Python code.
- A module can comprise of a single or multiple files

A Python package is a special kind of module that has a sort of **hierarchy** of subpackages.
- For instance, in `a.b.c`, `a` is a package that has subpackage `b`
```

The two most common package managers are `pip` and `conda`.