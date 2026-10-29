# Type hints, docstrings and code style

**Covers:** Type hint, Docstring, PEP, PEP 8, Union, Optional, ALLCAPS, UpperCamelCase

## Code that others can read {#overview}
Code is read far more often than it is written, by colleagues and by you in six months. Python helps with three habits: describing the types a function expects, documenting what it does, and following the shared style that the community agreed on.

## Type hint {#type-hint}
A type hint is an optional annotation on a function or variable that documents the type of value it expects or returns, like `age: int` or `-> str`. Python does not enforce hints when the program runs. Separate tools, such as editors and checkers, read them and warn about mismatches before the code is executed.

## Union {#union}
Union is a type hint meaning that a value may be one of several listed types. `int | str` means an integer or text. The older spelling is `Union[int, str]`, imported from the `typing` module.

## Optional {#optional}
Optional is a type hint meaning that a value may be of the given type or `None`. `Optional[str]` is the same as `str | None`. It is the usual hint for a parameter with a default of `None`, or a function that sometimes finds nothing.

## Docstring {#docstring}
A docstring is a string written as the very first line in the body of a function, class or module, which documents what it does. Tools and the built-in `help()` function can show it later. Triple quotes are used, so that it can span several lines.

## PEP {#pep}
A PEP, Python Enhancement Proposal, is a numbered design document for Python. Some propose language changes, and others, like PEP 8, give guidelines.

## PEP 8 {#pep-8}
PEP 8 is Python's official, community-standard style guide, covering naming, spacing and formatting. Examples: four spaces per indent, lines up to 79 characters, and two blank lines between top-level functions. Most Python code you meet follows it, and tools can check or fix it automatically.

## ALLCAPS {#allcaps}
ALLCAPS is a naming style with every letter capitalized, words separated by underscores, such as `MAX_RETRIES`. Python uses it by convention for constants, values that should not change.

## UpperCamelCase {#uppercamelcase}
UpperCamelCase is a naming style that capitalizes each word, without underscores, such as `BankAccount`. Python uses it for class names. Functions and variables use lowercase with underscores instead.

## A worked example {#example}
A function before and after.

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

What changed:

1. The name `scale` says what the function does, and `value` and `factor` replace `a` and `b`.
2. The hints say that both are numbers, and that `factor` may also be `None`. An editor can now warn about `scale("x")`.
3. The docstring explains the behavior, including the default.
4. Spacing follows PEP 8: spaces around operators, two blank lines before the function.
5. `MAX_ITEMS` is a constant, so it is written in ALLCAPS.

Hints are not enforced:

```python
def add(a: int, b: int) -> int:
    return a + b

print(add("x", "y"))  # xy
```

The program runs, since Python ignores the hints at run time. A checker would flag the call before it was run. The hint is a promise to readers and tools.

Class naming:

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

Optional versus Union:

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

Cost and benefit: adding hints to 50 functions might take an hour. A checker then finds a whole group of mistakes, such as passing text where a number is needed, before any user does. In a team of 5, a docstring saves each reader the time of reading the body. The habit pays back quickly.

## A common mistake {#mistake}
Believing that hints are checked by Python itself. They are not. Another is a docstring that repeats the name, such as `"""Gets the name."""` for `get_name`. Say what is not obvious: units, limits and what happens on bad input.

## Key points {#key-points}
- Type hints document types. They are checked by tools, not by Python at run time.
- `Optional[X]` means X or `None`. `Union` lists several types.
- A docstring documents a function, class or module. PEP 8 is the shared style guide.
- Constants are ALLCAPS, classes are UpperCamelCase, everything else is lowercase with underscores.
