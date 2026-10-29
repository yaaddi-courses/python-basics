# Conditions, truthiness and None

**Covers:** Truthy, Falsy, Ternary expression, None

## Letting a program choose {#overview}
A program becomes useful when it can choose between actions. Python does this with `if`, `elif` and `else`. The condition after `if` does not have to be a comparison: any value can be used, and Python decides whether it counts as yes or no.

## Truthy {#truthy}
A truthy value is one that counts as passing when used in a condition, even though it is not literally the boolean yes value. Any non-empty string, any non-zero number and any non-empty collection is truthy. This lets you write `if name:` to mean "if name has something in it".

## Falsy {#falsy}
A falsy value counts as false in a condition. The falsy values are `0`, `0.0`, the empty string `''`, an empty list `[]`, an empty collection of any other kind, `False` and `None`. Everything else is truthy.

## None {#none}
`None` is Python's special value that means "no value here". A function that has no `return` gives back `None`. It is used to show that something is missing or not yet filled in. Its type is its own: `NoneType`. Compare with `is`, as in `x is None`.

## Ternary expression {#ternary-expression}
A ternary expression is a one-line shortcut for an `if` and `else` that produces a value. The form is `value_if_true if condition else value_if_false`. It is for simple choices. For anything longer, a full block is easier to read.

## A worked example {#example}
A full `if` chain:

```python
score = 72
if score >= 90:
    grade = "A"
elif score >= 70:
    grade = "B"
else:
    grade = "C"
print(grade)  # B
```

Python tests from the top and takes the first condition that passes. 72 is not at least 90, but it is at least 70, so the result is `B`. The `else` is the fallback.

Now truthiness. Which of these values pass an `if`?

```python
for value in (0, 1, "", "hi", [], [0], None):
    if value:
        print(repr(value), "truthy")
    else:
        print(repr(value), "falsy")
# 0 falsy
# 1 truthy
# '' falsy
# 'hi' truthy
# [] falsy
# [0] truthy
# None falsy
```

Notice `[0]`: a list with one item, even though that item is zero, is truthy. Only an empty list is falsy.

A practical use:

```python
names = []
if names:
    print("have names")
else:
    print("no names yet")  # no names yet
```

Do not mix truthiness with exact comparison when zero is a valid value. Suppose a function should say how many items were found, or `None` if the search did not run:

```python
def describe(count):
    if count is None:
        return "search not run"
    if count == 0:
        return "nothing found"
    return f"{count} found"

print(describe(None))  # search not run
print(describe(0))     # nothing found
print(describe(3))     # 3 found
```

If the first test were `if not count:`, then `0` and `None` would be treated alike, and the message "search not run" would appear for a search that ran and found nothing.

The ternary version of a simple choice:

```python
age = 20
status = "adult" if age >= 18 else "minor"
print(status)  # adult
```

One line instead of four. Count the saving: the full block takes 4 lines, the ternary 1. Use it when the choice is this plain. Avoid nesting two of them in a single line, which is hard to read.

| Value | Counts as |
|---|---|
| `0`, `0.0` | Falsy |
| `''`, `[]`, `{}` | Falsy |
| `None`, `False` | Falsy |
| Any other number or text | Truthy |
| A non-empty collection | Truthy |

## A common mistake {#mistake}
Using `if not x:` to test for `None` when `0` or an empty string is also a valid value. Be exact: `if x is None:`. Another is comparing with `== None`. It usually works, but `is None` is the correct and faster form.

## Key points {#key-points}
- Any value can be used in a condition. Empty and zero values are falsy.
- `None` means "no value", and is tested with `is None`.
- A ternary expression is a one-line `if` and `else` that gives a value.
- Be exact when zero or an empty value is meaningful.
