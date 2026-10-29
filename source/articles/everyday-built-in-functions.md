# Everyday built-in functions

**Covers:** print(), input(), int(), float(), len(), sum(), help(), math.floor()

## Functions that are always there {#overview}
Python comes with a collection of functions that need no import. You will use a handful of them in nearly every program you write. In this article, you will see what each one does, what it gives back, and a few details that surprise beginners.

## print() {#print}
`print()` writes text to the screen. Give it several values, and it puts a space between them and ends with a new line. You can change both: `print("a", "b", sep="-", end="!")` writes `a-b!` without a new line.

## input() {#input}
`input()` reads a line that the user types and hands it back as text. It can show a prompt: `input("Name: ")`. The result is always text, even when the user types digits, so convert it if you need a number.

## int() {#int}
`int()` converts a value, such as a string, to a whole number. `int("42")` is 42. Given a decimal number, it cuts off the fraction: `int(3.9)` is 3. Given text that is not a whole number, such as `"3.9"` or `"abc"`, it raises an error.

## float() {#float}
`float()` converts a value to a decimal number. `float("3.5")` is 3.5, and `float(2)` is 2.0.

## len() {#len}
`len()` tells you how many items a sequence or collection holds: the characters of text, the items of a list, the keys of a dictionary.

## sum() {#sum}
`sum()` adds up the numbers in an iterable. `sum([1, 2, 3])` is 6. A second argument sets the starting value, which is 0 by default.

## help() {#help}
`help()` shows the documentation of an object. `help(len)` prints a short description of `len`. It is the quickest way to check what a function is for without leaving the program.

## math.floor() {#math-floor}
`math.floor()` rounds a number down to the nearest whole number. It comes from the `math` module, so it needs `import math`. It differs from `int()` for negative numbers: `math.floor(-2.5)` is -3, but `int(-2.5)` is -2.

## A worked example {#example}
Let's start with output and sizes:

```python
print("a", "b")                # a b
print("a", "b", sep="-")       # a-b
print(len("hello"))            # 5
print(len([4, 5, 6]))          # 3
print(len({"x": 1, "y": 2}))   # 2
```

Next, conversion:

```python
print(int("42") + 1)    # 43
print(int(3.9))         # 3
print(float("3.5") * 2) # 7.0
print(float(2))         # 2.0
```

Totals and averages use `sum()` and `len()` together:

```python
scores = [80, 90, 100, 70]
average = sum(scores) / len(scores)
print(sum(scores))  # 340
print(average)      # 85.0
```

The sum is 340. Divided by the 4 scores, the average is 85.0.

Now rounding down against cutting:

```python
import math

print(math.floor(2.7))   # 2
print(math.floor(-2.5))  # -3
print(int(-2.5))         # -2
```

For positive numbers the two agree. For -2.5, `floor` goes down to -3, and `int` cuts toward zero to -2. When money or time is involved, choose carefully.

A typical use of `input()` and conversion follows. It is shown as text because it waits for the keyboard:

```text
age_text = input("Age: ")
age = int(age_text)
print(f"Next year you will be {age + 1}")
```

If the user types `30`, the output is `Next year you will be 31`. If the user types `thirty`, `int()` raises an error, which a later article shows you how to handle. Without the conversion, `age_text + 1` would fail, because text cannot be added to a number.

And here is `help()`:

```text
>>> help(len)
Help on built-in function len in module builtins:
len(obj, /)
    Return the number of items in a container.
```

That is a quick way to confirm that `len` takes one argument.

| Function | Input | Output |
|---|---|---|
| `print(x)` | Anything | Writes text, returns nothing |
| `input(p)` | A prompt | Text typed by the user |
| `int(x)` | Text or number | Whole number |
| `float(x)` | Text or number | Decimal number |
| `len(x)` | A collection | Count of items |
| `sum(xs)` | Numbers | Their total |

## A common mistake {#mistake}
A common slip is forgetting that `input()` gives text. Then `'3' + '4'` becomes `'34'` instead of 7. Convert before you calculate.

A second slip is dividing by `len()` of an empty list, which raises an error. Check that the list has items first.

## Key points {#key-points}
- `print()` shows values. `input()` reads text.
- `int()` and `float()` convert. `int()` cuts off the fraction.
- `len()` counts items. `sum()` adds numbers.
- `math.floor()` goes down, which differs from `int()` for negative numbers.
