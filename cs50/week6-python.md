# CS50 Week 6 — Python

## Summary
Python is an interpreted, high-level language that eliminates much of C's manual work — no pointers, no memory management, no semicolons. Its syntax is concise and readable, making it ideal for rapid development. Python's standard library and ecosystem (pip packages) cover almost every use case out of the box. Lists replace arrays with dynamic sizing, and dictionaries replace hash tables with clean key-value syntax. Functions, loops, and conditionals work similarly to C but with less boilerplate. Python's CS50 library provides familiar helpers like `get_int()` and `get_string()` for beginners. The language's flexibility makes it the dominant language in data science, machine learning, and backend web development.

---

## Notes

### Python Basics vs. C

| Concept | C | Python |
|---|---|---|
| Print | `printf("hello\n")` | `print("hello")` |
| Variable | `int x = 5;` | `x = 5` |
| Input | `get_string("Name: ")` | `input("Name: ")` |
| Loop | `for (int i=0; i<3; i++)` | `for i in range(3):` |
| Condition | `if (x == 5) {}` | `if x == 5:` |

- No curly braces — **indentation** defines blocks
- No semicolons at end of lines
- No need to declare variable types — Python is **dynamically typed**
- No `main()` function required — code runs top to bottom

---

### Data Types & Variables

- **`int`** — whole numbers: `x = 5`
- **`float`** — decimals: `x = 3.14`
- **`str`** — strings: `name = "Alice"`
- **`bool`** — `True` / `False` (capitalized)
- **`None`** — equivalent to `null` in other languages

**Type conversion:**
```python
x = int("5")      # string → int
y = str(42)       # int → string
z = float("3.14") # string → float
```

---

### Strings

- Concatenation: `"hello" + " " + "world"`
- f-strings (preferred): `f"Hello, {name}!"`
- Methods: `.lower()`, `.upper()`, `.strip()`, `.split()`, `.replace()`

```python
name = "Alice"
print(f"Hello, {name}!")   # Hello, Alice!
print(name.upper())        # ALICE
print("  hi  ".strip())    # hi
```

---

### Lists

- Python's dynamic array — no fixed size
- Zero-indexed like C arrays
- Can hold mixed types

```python
nums = [1, 2, 3, 4, 5]
nums.append(6)        # add to end
nums.remove(3)        # remove by value
len(nums)             # length
nums[0]               # first element
nums[-1]              # last element
```

**Iterating:**
```python
for n in nums:
    print(n)

for i, n in enumerate(nums):
    print(i, n)   # index + value
```

---

### Dictionaries

- Key-value pairs — Python's built-in hash table
- Keys must be unique

```python
person = {
    "name": "Alice",
    "age": 21
}

person["name"]           # Alice
person["email"] = "..."  # add new key
"age" in person          # True — check if key exists
person.keys()            # all keys
person.values()          # all values
person.items()           # key-value pairs
```

---

### Functions

```python
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))   # Hello, Alice!
```

- Default arguments: `def greet(name="World"):`
- Multiple return values: `return x, y` (returns a tuple)

---

### Conditionals & Loops

```python
# if / elif / else
if x > 0:
    print("positive")
elif x < 0:
    print("negative")
else:
    print("zero")

# while loop
i = 0
while i < 5:
    print(i)
    i += 1

# for loop with range
for i in range(10):
    print(i)

# range(start, stop, step)
for i in range(0, 10, 2):
    print(i)   # 0, 2, 4, 6, 8
```

---

### Files

```python
# Reading
with open("file.txt", "r") as f:
    content = f.read()       # whole file as string
    lines = f.readlines()    # list of lines

# Writing
with open("file.txt", "w") as f:
    f.write("hello\n")

# Appending
with open("file.txt", "a") as f:
    f.write("more\n")
```

- `with` statement automatically closes the file

---

### Libraries & pip

```python
import math
import random
import sys
import csv

math.sqrt(16)        # 4.0
random.randint(1,10) # random int 1–10
sys.argv             # command-line arguments
sys.exit(1)          # exit with error code
```

**Install packages:**
```bash
pip install requests
```

---

### CS50 Python Library

```python
from cs50 import get_int, get_float, get_string, SQL

name = get_string("Name: ")
age  = get_int("Age: ")
```

---

## Cue Column

| Term | Definition |
|---|---|
| **Interpreted** | Python runs line by line — no compilation step |
| **Dynamically typed** | Variable types determined at runtime, not declared |
| **f-string** | `f"..."` syntax for embedding variables in strings |
| **List** | Ordered, mutable, dynamic-size collection |
| **Dictionary** | Key-value store — Python's built-in hash table |
| **`None`** | Python's null value |
| **`with` statement** | Context manager — auto-closes files and resources |
| **pip** | Python's package manager for installing libraries |
