# Sorting and sort keys

**Covers:** sort(), sorted(), Sort key

## Putting things in order {#overview}
Sorting is one of the most common tasks you will meet, and Python offers two tools for it. They differ in one important way: one changes the list you give it, and the other leaves it alone. A third idea, the key, lets you decide what "order" means. Let's take them in turn.

## sort() {#sort}
`sort()` sorts a list in place and returns `None`. The original list is changed and nothing new is made. It exists only for lists.

## sorted() {#sorted}
`sorted()` returns a new sorted list and leaves the original alone. It works on any iterable: a list, a tuple, a string, the keys of a dictionary. The result is always a list.

## Sort key {#sort-key}
What if you want to sort words by length, or people by age? You give Python a sort key. This is a function that you pass with `key=`, and Python calls it once for each item to decide what to sort by, instead of comparing the raw items directly. The items are then ordered by the values the function returns. Both `sort()` and `sorted()` accept it. The `reverse=True` option flips the order.

## A worked example {#example}
Let's see the difference between the two tools:

```python
numbers = [3, 1, 2]
result = sorted(numbers)
print(result)   # [1, 2, 3]
print(numbers)  # [3, 1, 2]

numbers.sort()
print(numbers)  # [1, 2, 3]
```

`sorted()` produced a new list, and `numbers` stayed as it was. Then `sort()` changed `numbers` itself.

Here is the classic mistake:

```python
numbers = [3, 1, 2]
result = numbers.sort()
print(result)   # None
print(numbers)  # [1, 2, 3]
```

`sort()` returns `None`, so `result` holds nothing useful. If you write `numbers = numbers.sort()`, you lose your list.

Now reverse order:

```python
print(sorted([3, 1, 2], reverse=True))  # [3, 2, 1]
```

Sorting text comes next. By default, capital letters come before lowercase ones, because the comparison uses character codes:

```python
words = ["banana", "Apple", "cherry"]
print(sorted(words))                 # ['Apple', 'banana', 'cherry']
print(sorted(["b", "A", "a", "B"]))  # ['A', 'B', 'a', 'b']
```

To ignore capitals, use a key:

```python
words = ["banana", "Apple", "cherry", "apple"]
print(sorted(words, key=str.lower))  # ['Apple', 'apple', 'banana', 'cherry']
```

The key `str.lower` is applied to each word to get the value to compare: "banana", "apple", "cherry" and "apple". "Apple" and "apple" tie, and they keep their original relative order, because the sort is stable.

Sorting by length:

```python
words = ["pear", "fig", "banana", "kiwi"]
print(sorted(words, key=len))  # ['fig', 'pear', 'kiwi', 'banana']
```

The keys are 4, 3, 6 and 4. In order, that gives 3 (fig), 4 (pear), 4 (kiwi) and 6 (banana). Pear came before kiwi in the input, so it stays before.

Sorting records by one field:

```python
people = [("Ada", 36), ("Bo", 28), ("Cy", 41)]
print(sorted(people, key=lambda p: p[1]))
# [('Bo', 28), ('Ada', 36), ('Cy', 41)]
```

The key picks the age, and the names are carried along. For several fields, return a tuple: `key=lambda p: (p[1], p[0])` sorts by age, then by name.

Why does Python use a key function and not a comparison? The key is computed once per item, so sorting 1,000 items calls it 1,000 times. A comparison function would be called on the order of 10,000 times for the same list. That is the reason Python chose keys.

| Need | Code |
|---|---|
| New sorted list | `sorted(xs)` |
| Sort a list in place | `xs.sort()` |
| Descending | `reverse=True` |
| Ignore case | `key=str.lower` |
| By a field | `key=lambda r: r[1]` |

## A common mistake {#mistake}
A common slip is assigning the result of `sort()`, which is `None`. Use `sorted()` when you want a value.

A second slip is sorting mixed types, such as numbers and text together, which raises an error. A third is forgetting that capitals sort before lowercase letters.

## Key points {#key-points}
- `sort()` changes the list and returns `None`. `sorted()` returns a new list.
- `key=` names the function that gives each item its sorting value.
- `reverse=True` flips the order.
- Sorting is stable: equal items keep their original order.
