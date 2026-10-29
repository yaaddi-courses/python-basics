# Type conversion and common errors

**Covers:** Type conversion, Exception, TypeError, ValueError, IndexError, KeyError, ZeroDivisionError

## When values do not fit {#overview}
Programs receive data they cannot control: what a user types, what a file contains, what a server returns. Often it arrives as text and must be turned into a number. Sometimes it does not fit what the code expects. Python reports such problems with named errors, and knowing the common ones tells you where to look.

## Type conversion {#type-conversion}
Type conversion means converting a value from one data type to another, like turning the text `'42'` into the number 42 with `int('42')`. It is necessary because text typed by a user is always text, even when it looks like a number. The usual converters are `int()`, `float()` and `str()`.

## Exception {#exception}
An exception is an error that occurs while a program is running, like dividing by zero. Unlike a typing mistake that stops the program from starting at all, an exception can be caught and handled, so the program can go on. If nothing handles it, the program stops and prints a traceback that shows where it happened. Every exception has a type, and the type is the first clue.

## TypeError {#typeerror}
`TypeError` is raised when an operation is given a value of the wrong type entirely. Adding text to a number, `'a' + 5`, fails with it, since Python will not guess whether you meant text or arithmetic.

## ValueError {#valueerror}
`ValueError` is raised when a value has the right type but is not valid for what you ask. `int('abc')` fails with it: the argument is text, which `int()` accepts, but this text is not a number.

## IndexError {#indexerror}
`IndexError` is raised when you access a position that does not exist in a list, tuple or string. `[1, 2, 3][5]` fails, because a list of 3 items has positions 0 to 2 only.

## KeyError {#keyerror}
`KeyError` is raised when you look up a dictionary key that is not there. `d['missing']` on a dictionary without that key fails. `d.get('missing')` avoids it by returning `None`.

## ZeroDivisionError {#zerodivisionerror}
`ZeroDivisionError` is raised when a number is divided by zero, as in `1 / 0` or `5 // 0`. Division by zero is mathematically undefined, so Python refuses.

## A worked example {#example}
One line of code for each of the five errors. Each is caught so that the program goes on and shows the error's name.

```python
def attempt(label, action):
    try:
        action()
    except Exception as error:
        print(label, "->", type(error).__name__)

attempt("text + number", lambda: "a" + 5)
attempt("bad number", lambda: int("abc"))
attempt("bad position", lambda: [1, 2, 3][5])
attempt("bad key", lambda: {"a": 1}["b"])
attempt("zero", lambda: 1 / 0)
# text + number -> TypeError
# bad number -> ValueError
# bad position -> IndexError
# bad key -> KeyError
# zero -> ZeroDivisionError
```

Do not worry about `lambda` here: each one is a tiny function that runs the risky line later, inside the `try`.

Now conversion done safely. A user types a quantity, and the program must turn it into a number:

```python
def to_quantity(text):
    try:
        return int(text)
    except ValueError:
        return None

print(to_quantity("12"))   # 12
print(to_quantity("12x"))  # None
print(to_quantity("3.5"))  # None
```

Notice that `"3.5"` also fails with `int()`: the text is a decimal. Convert with `float()` first if decimals are allowed, then `int()`.

```python
print(int(float("3.5")))  # 3
```

`int()` cuts off the fraction, it does not round. 3.5 becomes 3, and `-3.5` becomes -3.

Conversion between types in arithmetic:

```python
print("3" * 3)    # 333
print(int("3") * 3)  # 9
```

A text repeated three times is not the same as a number multiplied by three. Check the type before blaming the arithmetic.

How to use the error name. Read the traceback from the bottom. The last line names the type and gives a message, for example `ValueError: invalid literal for int() with base 10: 'abc'`. Above it are the lines of code, with the last one being where it happened. With the type, you know the category of the problem: wrong kind of value (`TypeError`), right kind but bad content (`ValueError`), missing position or key (`IndexError`, `KeyError`), or arithmetic (`ZeroDivisionError`).

| Error | Typical cause | Fix |
|---|---|---|
| `TypeError` | Mixing incompatible types | Convert one side |
| `ValueError` | Text that is not a number | Validate or catch |
| `IndexError` | Position beyond the end | Check `len()` |
| `KeyError` | Missing dictionary key | Use `get()` |
| `ZeroDivisionError` | Divisor is zero | Test the divisor |

## A common mistake {#mistake}
Catching every exception with a bare `except:` and hiding the real problem. Catch the specific type you expect. Another is forgetting that `input()` returns text, and adding two typed numbers to get `'35'` instead of `8`.

## Key points {#key-points}
- Convert types explicitly: `int()`, `float()`, `str()`.
- An exception has a type, and the type tells you the kind of problem.
- `TypeError` is a wrong type, `ValueError` a bad value, `IndexError` and `KeyError` missing items.
- Read the traceback from the bottom.
