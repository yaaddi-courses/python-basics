# Type hints, docstrings and code style

**Covers:** Type hint, Docstring, PEP, PEP 8, Union, Optional, ALLCAPS, UpperCamelCase

## Code that others can read {#overview}
Code is read far more often than it is written, by colleagues and by you in six months. Python helps with three habits: describing the types a function expects, documenting what it does, and following the shared style the community agreed on. Let's go through them, starting with the types.

## Type hint {#type-hint}
A type hint is an optional annotation on a function or variable that documents the type of value it expects or returns, like `age: int` or `-> str`. Python does not enforce hints when the program is working. Separate tools, such as editors and checkers, read them and warn about mismatches before the code is executed.

## Union {#union}
Sometimes a value may be one of several types. Union is the hint for that. `int | str` means an integer or text. The older spelling is `Union[int, str]`, imported from the `typing` module.

## Optional {#optional}
Optional is a hint meaning that a value may be of the given type or `None`. `Optional[str]` is the same as `str | None`. It is the usual hint for a parameter whose default is `None`, or for a function that sometimes finds nothing.

## Docstring {#docstring}
A docstring is a string written as the very first line in the body of a function, class or module, and it documents what that thing does. Tools and the built-in `help()` function can show it later. You write it in triple quotes, so that it can span several lines.

## PEP {#pep}
A PEP, a Python Enhancement Proposal, is a numbered design document for Python. Some propose changes to the language, and others, like PEP 8, give guidelines.

## PEP 8 {#pep-8}
PEP 8 is Python's official, community-standard style guide, covering naming, spacing and formatting. For example, it asks for four spaces per indent, lines up to 79 characters, and two blank lines between top-level functions. Most Python code you meet follows it, and tools can check or fix it automatically.

## ALLCAPS {#allcaps}
ALLCAPS is a naming style with every letter capitalized and words separated by underscores, such as `MAX_RETRIES`. Python programmers use it by convention for constants, which are values that should not change.

## UpperCamelCase {#uppercamelcase}
UpperCamelCase capitalizes each word and uses no underscores, such as `BankAccount`. Python uses it for class names. Functions and variables use lowercase with underscores instead.

## A worked example {#example}
Let's look at a function before and after.

Before:

```python
def f(a, b=None):
    if b is None:
        b = 1
    return a*b
```

After:

```python
MAX_ITEMS = 100


def scale(value: float, factor: float | None = None) -> float:
    """Return value multiplied by factor (1.0 when no factor is given)."""
    if factor is None:
        factor = 1.0
    return value * factor


print(scale(4.0, 2.5))  # 10.0
print(scale(4.0))       # 4.0
print(scale.__doc__)    # Return value multiplied by factor (1.0 when no factor is given).
```

Here is what changed:

1. The name `scale` says what the function does, and `value` and `factor` replace `a` and `b`.
2. The hints say that both are numbers, and that `factor` may also be `None`. An editor can now warn about `scale("x")`.
3. The docstring explains the behavior, including the default.
4. Spacing follows PEP 8: spaces around operators, two blank lines before the function.
5. `MAX_ITEMS` is a constant, so it is written in ALLCAPS.

Remember that hints are not enforced:

```python
def add(a: int, b: int) -> int:
    return a + b

print(add("x", "y"))  # xy
```

The program works anyway, because Python ignores the hints while it is working. A checker would flag the call before it ever ran. The hint is a promise to readers and tools.

Now class naming:

```python
class BankAccount:
    pass

class HttpClient:
    pass

print(BankAccount.__name__)  # BankAccount
```

`BankAccount` and `HttpClient` use UpperCamelCase. A function named `calculate_interest` and a constant named `INTEREST_RATE` complete the picture:

| Thing | Style | Example |
|---|---|---|
| Variable, function | lowercase_with_underscores | `total_price` |
| Class | UpperCamelCase | `OrderLine` |
| Constant | ALLCAPS | `MAX_RETRIES` |

Finally, Optional against Union:

```python
from typing import Optional, Union

def label(x: Union[int, str]) -> str:
    return str(x)

def find(name: str) -> Optional[int]:
    return {"a": 1}.get(name)

print(label(5))    # 5
print(find("a"))   # 1
print(find("z"))   # None
```

`find` is honest in its hint: it returns an integer or `None`.

What does all this cost, and what does it give back? Adding hints to 50 functions might take an hour. A checker then finds a whole group of mistakes, such as passing text where a number is needed, before any user does. In a team of 5, a docstring saves each reader the time of reading the function body. The habit pays back quickly.

## A common mistake {#mistake}
A common slip is believing that Python itself checks hints. It does not, so a wrong call still works until it fails somewhere else.

A second slip is a docstring that repeats the name, such as `"""Gets the name."""` for `get_name`. Say what is not clear from the name: units, limits and what happens on bad input.

## Key points {#key-points}
- Type hints document types. Tools check them, and Python does not.
- `Optional[X]` means X or `None`. `Union` lists several types.
- A docstring documents a function, class or module. PEP 8 is the shared style guide.
- Constants are ALLCAPS, classes are UpperCamelCase, everything else is lowercase with underscores.
