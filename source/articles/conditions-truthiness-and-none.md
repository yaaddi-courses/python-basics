# Conditions, truthiness and None

**Covers:** Truthy, Falsy, Ternary expression, None

## Letting a program choose {#overview}
A program becomes useful when it can choose between actions. Python does this with `if`, `elif` and `else`. Here is something that surprises many beginners: the condition after `if` does not have to be a comparison. You can use any value there, and Python decides whether it counts as yes or no. Let's see how it decides.

## Truthy {#truthy}
A truthy value passes when you use it in a condition, even though it is not literally the boolean yes value. Any non-empty string, any non-zero number and any non-empty collection is truthy. That lets you write `if name:` to mean "if name has something in it".

## Falsy {#falsy}
The opposite group counts as no. The falsy values are `0`, `0.0`, the empty string `''`, an empty list `[]`, an empty collection of any other kind, `False` and `None`. Everything else is truthy, so this short list is the only one you need to remember.

## None {#none}
`None` is Python's special value for "no value here". A function with no `return` gives back `None`, and programmers use it to show that something is missing or not yet filled in. It has a type of its own, `NoneType`. To test for it, compare with `is`, as in `x is None`.

## Ternary expression {#ternary-expression}
When a choice is small, a full block feels heavy. A ternary expression is a one-line shortcut for an `if` and `else` that produces a value. The form is `value_if_true if condition else value_if_false`. Keep it for simple choices, and write a full block for anything longer, because the block is easier to read.

## A worked example {#example}
Let's start with a full `if` chain:

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

Python tests from the top and takes the first condition that passes. 72 is not at least 90, but it is at least 70, so the result is `B`. The `else` is the fallback when nothing above it passed.

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

Look at `[0]`. It is a list with one item, and that item is zero, yet the list is truthy. Only an empty list is falsy.

Here is a practical use:

```python
names = []
if names:
    print("have names")
else:
    print("no names yet")  # no names yet
```

Be careful not to mix truthiness with exact comparison when zero is a valid value. Suppose a function should say how many items were found, or report that the search did not happen:

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

If the first test were `if not count:`, then `0` and `None` would be treated alike. The message "search not run" would then appear for a search that did happen and found nothing.

The ternary version of a simple choice looks like this:

```python
age = 20
status = "adult" if age >= 18 else "minor"
print(status)  # adult
```

That is one line instead of four. The full block takes 4 lines and the ternary takes 1, so use it when the choice is this plain. Avoid putting two of them inside each other on one line, because that is hard to read.

| Value | Counts as |
|---|---|
| `0`, `0.0` | Falsy |
| `''`, `[]`, `{}` | Falsy |
| `None`, `False` | Falsy |
| Any other number or text | Truthy |
| A non-empty collection | Truthy |

## A common mistake {#mistake}
A common slip is writing `if not x:` to test for `None` when `0` or an empty string is also a valid value. Be exact and write `if x is None:`.

A second slip is comparing with `== None`. It usually works, but `is None` is the correct and faster form.

## Key points {#key-points}
- Any value can be used in a condition. Empty and zero values are falsy.
- `None` means "no value", and you test it with `is None`.
- A ternary expression is a one-line `if` and `else` that gives a value.
- Be exact when zero or an empty value is meaningful.
