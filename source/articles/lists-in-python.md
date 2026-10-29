# Lists and how they change

**Covers:** List, append(), extend(), pop(), insert(), push(), Mutable, Immutable

## An ordered collection {#overview}
Programs often hold many values of a kind: scores, names, prices. A list holds them in order and lets you add, remove and change items. This article shows the main list operations, and explains the property that decides how they behave: whether a value can be changed in place.

## List {#list}
A list is an ordered, mutable collection of items written in square brackets, like `[1, 2, 3]`. The items may be of different types, and you can add, remove or change them after the list has been created. Positions start at 0, and negative positions count from the end.

## Mutable {#mutable}
Mutable describes an object whose content can be altered after it is created, with no new object made. Lists and dictionaries are mutable. If two names point at the same list, a change made through one name is visible through the other, because there is only one list.

## Immutable {#immutable}
Immutable means locked once created: its content never changes, and it can only be replaced entirely with a new object. Strings, numbers and tuples are immutable. Immutable values are safe to share, since nobody can change them under you.

## append() {#append}
`append()` adds one item to the end of a list. It changes the list in place and returns nothing.

## extend() {#extend}
`extend()` adds every item of another collection to the end. Where `append([4, 5])` adds a list as one item, `extend([4, 5])` adds two items.

## insert() {#insert}
`insert()` puts an item at a chosen position instead of at the end: `insert(0, "x")` puts it first. The items after it move one place along.

## pop() {#pop}
`pop()` removes and returns an item, the last one by default, or the one at a given position.

## push() {#push}
`push()` is the JavaScript name for adding an item to the end of an array. Python has no `push()`, and a list uses `append()` instead.

## A worked example {#example}
Start with a shopping list and change it.

```python
items = ["milk", "eggs"]
items.append("bread")
print(items)  # ['milk', 'eggs', 'bread']
items.extend(["jam", "tea"])
print(items)  # ['milk', 'eggs', 'bread', 'jam', 'tea']
items.insert(0, "coffee")
print(items)  # ['coffee', 'milk', 'eggs', 'bread', 'jam', 'tea']
last = items.pop()
print(last)   # tea
print(items)  # ['coffee', 'milk', 'eggs', 'bread', 'jam']
```

Count: 2 items at the start, 3 after `append`, 5 after `extend`, 6 after `insert`, 5 after `pop`. Compare `append` and `extend`:

```python
a = [1, 2]
a.append([3, 4])
print(a)  # [1, 2, [3, 4]]
b = [1, 2]
b.extend([3, 4])
print(b)  # [1, 2, 3, 4]
```

`a` has 3 items, one of which is a list. `b` has 4 items.

Now mutability, the cause of many surprises:

```python
first = [1, 2, 3]
second = first
second.append(4)
print(first)  # [1, 2, 3, 4]
```

There was only one list, and two names for it. Appending through `second` changed what `first` shows. To get an independent list, copy it:

```python
first = [1, 2, 3]
second = first.copy()
second.append(4)
print(first)   # [1, 2, 3]
print(second)  # [1, 2, 3, 4]
```

Compare with an immutable value:

```python
word = "cat"
other = word
other = other + "s"
print(word)   # cat
print(other)  # cats
```

Here `other + "s"` made a new string, so `word` could not change.

Speed also differs. `append()` and `pop()` at the end are quick. `insert(0, x)` has to shift all the other items, so for a list of 1,000,000 items it moves a million places, much slower. If you often add at the front, a different structure is better.

| Operation | Effect | Returns |
|---|---|---|
| `append(x)` | Adds one item at the end | Nothing |
| `extend(xs)` | Adds each item of `xs` | Nothing |
| `insert(i, x)` | Adds at position `i` | Nothing |
| `pop()` | Removes the last item | That item |

## A common mistake {#mistake}
Writing `items = items.append("x")`. `append()` returns nothing, so `items` becomes `None`, and the list is lost. Call it on its own line. Another is thinking that `second = first` makes a copy. It does not.

## Key points {#key-points}
- A list is ordered and mutable. Strings, numbers and tuples are immutable.
- `append` adds one item. `extend` adds many. `insert` adds at a position. `pop` removes and returns.
- Assigning a list to another name shares it. Use `copy()` for an independent list.
- Python has `append()`, not `push()`.
