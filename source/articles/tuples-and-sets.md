# Tuples, unpacking and sets

**Covers:** Tuple, Unpacking, Set

## Two collections with a rule {#overview}
A list can hold anything and change at any time. Sometimes you want less freedom: a group of values that must not change, or a collection in which an item cannot appear twice. Python has a built-in type for each.

## Tuple {#tuple}
A tuple is an ordered collection of items written in parentheses, like `(1, 2)`. Once created, its content can never change: you cannot add, remove or replace an item. It supports indexing and slicing like a list. A tuple with one item needs a trailing comma, `(5,)`, otherwise Python reads the parentheses as plain grouping.

Use a tuple for a record whose parts belong together and do not change, such as a point `(x, y)` or a date `(2026, 10, 28)`. Because it cannot change, it is safe to share and can be used where a mutable list cannot.

## Unpacking {#unpacking}
Unpacking means assigning each item of a tuple, or a list, to its own variable in one line, in order. `x, y = (1, 2)` gives `x` the value 1 and `y` the value 2. The number of variables must match the number of items, unless you collect the rest with a star.

## Set {#set}
A set is an unordered collection that automatically drops duplicates. Adding the same item twice keeps one copy, so `{1, 1, 2}` becomes `{1, 2}`. Because there is no order, a set has no indexing. Checking whether an item is in a set is very fast, much faster than searching a long list.

Sets support the operations of mathematics: joining with `|`, overlap with `&`, and difference with `-`. An empty set is written `set()`, since `{}` makes an empty `dict`.

## A worked example {#example}
A tuple as a fixed record:

```python
point = (3, 4)
print(point[0])    # 3
print(point[1])    # 4
print(len(point))  # 2
```

Try to change it, and Python refuses:

```python
point = (3, 4)
try:
    point[0] = 10
except TypeError:
    print("tuples cannot be changed")  # tuples cannot be changed
```

Unpacking makes the code readable. Instead of indexing:

```python
point = (3, 4)
x, y = point
print(x + y)  # 7
```

A common idiom is swapping two values without a temporary variable:

```python
a, b = 1, 2
a, b = b, a
print(a, b)  # 2 1
```

Unpacking with a star collects the rest:

```python
first, *rest = (10, 20, 30, 40)
print(first)  # 10
print(rest)   # [20, 30, 40]
```

Now sets. Remove duplicates from a list of visitors:

```python
visitors = ["ann", "bob", "ann", "cy", "bob", "ann"]
unique = set(visitors)
print(len(visitors))  # 6
print(len(unique))    # 3
print(sorted(unique))  # ['ann', 'bob', 'cy']
```

Six entries, three different people. `sorted()` is used for display, because a set has no fixed order.

Compare two groups:

```python
monday = {"ann", "bob", "cy"}
tuesday = {"bob", "cy", "dee"}
print(sorted(monday & tuesday))  # ['bob', 'cy']
print(sorted(monday | tuesday))  # ['ann', 'bob', 'cy', 'dee']
print(sorted(monday - tuesday))  # ['ann']
```

Both days: bob and cy (overlap). Either day: 4 people (joined). Monday only: ann (difference).

Speed: checking whether a name is among 1,000,000 stored names takes about a million comparisons in a list in the worst case, but a set finds it in roughly one step. For a program that makes many such checks, the choice of a set can turn minutes into milliseconds.

| | List | Tuple | Set |
|---|---|---|---|
| Ordered | Yes | Yes | No |
| Changeable | Yes | No | Yes (add, remove) |
| Duplicates | Allowed | Allowed | Dropped |
| Brackets | `[ ]` | `( )` | `{ }` |

## A common mistake {#mistake}
Writing `{}` and expecting an empty set. It is an empty `dict`. Use `set()`. Another is writing `(5)` for a one-item tuple, which is just the number 5. Write `(5,)`.

## Key points {#key-points}
- A tuple is ordered and cannot change. A set has no order and no duplicates.
- Unpacking assigns each item to its own variable.
- A set answers "is it in here?" very quickly and supports joining, overlap and difference.
- `set()` makes an empty set, and `{}` makes an empty `dict`.
