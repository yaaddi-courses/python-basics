# Names that look right but do not exist

**Covers:** downcase(), casefold_all(), add(), find(), lookup(), scan(), get_input(), ...

## Why wrong answers look so reasonable {#overview}
When you learn a new language, you guess names from other languages and from plain English. Some guesses are right and some are not. Python has a small, regular set of names, and the quickest way to learn them is to see the near-misses side by side with the real ones. Each name below looks plausible, and each is wrong in Python.

## downcase() {#downcase}
`downcase()` is not a Python function. It is a name from other languages, where text is "down-cased". In Python, the operation that turns text into lowercase is `lower()`.

## casefold_all() {#casefold-all}
`casefold_all()` does not exist either. Python does have `casefold()`, a stronger form of lowercase for comparing text in many languages, but no function with this long name. For everyday work, use `lower()`.

## add() {#add}
`add()` belongs to a set, not to a list. A set uses `add(item)` to insert an item. A list uses `append(item)` to add to the end, or `insert(position, item)` to add elsewhere. Calling `add()` on a list stops with an error.

## find() {#find}
`find()` is a string operation that returns the position of some text inside a string, or -1 if it is missing. It is not how you look up a key in a dictionary. A dictionary is searched with square brackets, or safely with `get()`.

## lookup() {#lookup}
`lookup()` is not a dictionary function. A dictionary has `get()` for a safe lookup that returns a default, and square brackets for a strict lookup that raises an error when the key is missing.

## scan() {#scan}
`scan()` is not a Python function. Other languages use it to read input. Python reads a typed line with `input()`.

## get_input() {#get-input}
`get_input()` is not a built-in either. Programmers often write a function with this name of their own, but Python itself offers `input()`.

## The ellipsis, ... {#ellipsis}
The three dots `...` are a real Python value, called the ellipsis. It stands for "something goes here" and is used as a placeholder, and in some libraries for slicing. It is not how a function collects extra arguments. That job belongs to `*args` for positional arguments and `**kwargs` for keyword arguments.

## A worked example {#example}
Try each real and unreal name and see what Python says.

```python
text = "Hello"
print(text.lower())  # hello
print(hasattr(text, "downcase"))  # False
print(hasattr(text, "casefold_all"))  # False
print(hasattr(text, "casefold"))  # True
```

The real `lower()` works. The two invented names are not found. `casefold` does exist, but under its shorter name.

```python
numbers = [1, 2]
numbers.append(3)
print(numbers)  # [1, 2, 3]
print(hasattr(numbers, "add"))  # False
unique = {1, 2}
unique.add(3)
print(sorted(unique))  # [1, 2, 3]
```

`add()` belongs to the set. The list has no such operation.

```python
word = "banana"
print(word.find("nan"))  # 2
print(word.find("x"))  # -1
data = {"a": 1}
print(data.get("b"))  # None
print(hasattr(data, "lookup"))  # False
```

`find()` gave the position of "nan", and -1 for missing text. The dictionary has `get()` and no `lookup()`.

Placeholders and extra arguments:

```python
def todo():
    ...

def total(*args):
    return sum(args)

print(todo())  # None
print(total(1, 2, 3))  # 6
```

The `...` made an empty body that is valid code. The `*args` collected the three numbers. Swapping them, for example `def total(...args)`, is a syntax error.

How to avoid being fooled by a plausible name: check it. In the console, `dir(object)` lists the real names, and `help(function)` shows what a function does. When an answer looks right but you do not remember seeing it, `hasattr` or `dir` settles the question in seconds.

| Plausible name | Real Python |
|---|---|
| `downcase()`, `casefold_all()` | `lower()` |
| `add()` on a list | `append()` |
| `lookup()` on a dictionary | `get()` |
| `scan()`, `get_input()` | `input()` |
| `...` to collect arguments | `*args` and `**kwargs` |

## A common mistake {#mistake}
Using the name from the language you know best. Someone from JavaScript writes `push()`, from Ruby `downcase`, from C `scanf`. Python's names are different, so look them up once, and then they stay in your memory.

## Key points {#key-points}
- Python has `lower()`, `append()`, `get()` and `input()`. The lookalike names do not exist.
- `add()` is for sets and `find()` is for strings.
- `...` is a placeholder, not an argument collector.
- Use `dir()` and `help()` to check a name.
