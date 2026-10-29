# Lists and how they change

**Covers:** List, append(), extend(), pop(), insert(), push(), Mutable, Immutable

## An ordered collection {#overview}
Programs often hold many values of one kind: scores, names, prices. A list keeps them in order and lets you add, remove and change items. In this article you will learn the main list operations, and the property that explains how they behave: whether a value can be changed in place.

## List {#list}
A list is an ordered collection of items, written in square brackets, like `[1, 2, 3]`. The items may be of different types, and you can add, remove or change them after the list has been created. Positions start at 0, and negative positions count from the end.

## Mutable {#mutable}
Mutable describes an object whose content can be altered after it is created, without making a new object. Lists and dictionaries are mutable. This has a consequence you should remember: if two names point at the same list, a change through one name shows up through the other, because there is only one list.

## Immutable {#immutable}
Immutable is the opposite. The content never changes, and you can only replace the whole object with a new one. Strings, numbers and tuples are immutable. Immutable values are safe to share, since nobody can change them under you.

## append() {#append}
`append()` adds one item to the end of a list. It changes the list in place and returns nothing.

## extend() {#extend}
`extend()` adds every item of another collection to the end. Where `append([4, 5])` adds a list as one item, `extend([4, 5])` adds two items.

## insert() {#insert}
`insert()` puts an item at a position you choose instead of at the end: `insert(0, "x")` puts it first. The items after it move one place along.

## pop() {#pop}
`pop()` removes an item and gives it back to you. By default it takes the last one, or you can name a position.

## push() {#push}
If you have used JavaScript, you may look for `push()`, the name for adding an item to the end of an array. Python has no `push()`. A list uses `append()` instead.

## A worked example {#example}
Let's start with a shopping list and change it.

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

Let's count: 2 items at the start, 3 after `append`, 5 after `extend`, 6 after `insert` and 5 after `pop`. Now compare `append` and `extend`:

```python
a = [1, 2]
a.append([3, 4])
print(a)  # [1, 2, [3, 4]]
b = [1, 2]
b.extend([3, 4])
print(b)  # [1, 2, 3, 4]
```

`a` has 3 items, and one of them is a list. `b` has 4 items.

Now mutability, which causes many surprises:

```python
first = [1, 2, 3]
second = first
second.append(4)
print(first)  # [1, 2, 3, 4]
```

There was only one list, with two names for it. Appending through `second` changed what `first` shows. To get an independent list, copy it:

```python
first = [1, 2, 3]
second = first.copy()
second.append(4)
print(first)   # [1, 2, 3]
print(second)  # [1, 2, 3, 4]
```

Compare that with an immutable value:

```python
word = "cat"
other = word
other = other + "s"
print(word)   # cat
print(other)  # cats
```

Here `other + "s"` made a new string, so `word` could not change.

Speed differs too. `append()` and `pop()` at the end are quick. `insert(0, x)` has to shift all the other items, so for a list of 1,000,000 items it moves a million places, which is much slower. If you often add at the front, a different structure is better.

| Operation | Effect | Returns |
|---|---|---|
| `append(x)` | Adds one item at the end | Nothing |
| `extend(xs)` | Adds each item of `xs` | Nothing |
| `insert(i, x)` | Adds at position `i` | Nothing |
| `pop()` | Removes the last item | That item |

## A common mistake {#mistake}
A common slip is writing `items = items.append("x")`. It returns nothing, so `items` becomes `None` and the list is lost. Call it on a line of its own.

A second slip is thinking that `second = first` makes a copy. It does not.

## Key points {#key-points}
- A list is ordered and mutable. Strings, numbers and tuples are immutable.
- `append` adds one item. `extend` adds many. `insert` adds at a position. `pop` removes and returns.
- Assigning a list to another name shares it. Use `copy()` for an independent list.
- Python has `append()`, not `push()`.
