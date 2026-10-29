# Dictionaries and hashable keys

**Covers:** Dictionary, Hashable, Ada, keys(), items(), get()

## Looking things up by name {#overview}
A list finds things by position. Often the natural question is different: "what is the phone number of Ada?" A dictionary answers it directly, by looking up a key and returning the value stored with it.

## Dictionary {#dictionary}
A dictionary is a collection that maps keys to values, written as `key: value` pairs inside curly braces. You look up a value by its key, not by position. Keys are unique: assigning to an existing key replaces its value. Dictionaries keep the order in which keys were first added. They are mutable, so you can add, change and remove entries.

## Hashable {#hashable}
Hashable means able to be used as a dictionary key, or as an item of a set, because its value can never change. Numbers, strings and tuples of such values are hashable. Lists are not, since a list can change after being used as a key, and then the dictionary would not find it. Python raises an error if you try.

## Ada {#ada}
Ada is a name used as a sample value in dictionary examples, for example `{"name": "Ada"}`. It refers to Ada Lovelace, an early programmer. It has no special meaning to Python, and any other string would work the same way.

## keys() {#keys}
`keys()` returns a view of all the keys of a dictionary. It is what you get when you loop over a dictionary directly. A view is live: if the dictionary changes afterwards, the view shows the change.

## items() {#items}
`items()` returns a view of all the key and value pairs, as tuples. It is the usual way to loop over both parts at once: `for key, value in d.items():`.

## get() {#get}
`get()` returns the value for a key, or a default if the key is missing. `d.get("age")` gives `None` when there is no such key, and `d.get("age", 0)` gives 0. It avoids the error that the square bracket form raises for a missing key.

## A worked example {#example}
A small record:

```python
person = {"name": "Ada", "born": 1815}
print(person["name"])  # Ada
person["city"] = "London"
person["born"] = 1816
print(person)  # {'name': 'Ada', 'born': 1816, 'city': 'London'}
print(len(person))  # 3
```

Adding `city` created a new entry. Assigning to `born` replaced the old value, so there are still 3 entries, not 4.

Looking up a missing key:

```python
person = {"name": "Ada"}
print(person.get("age"))       # None
print(person.get("age", 0))    # 0
print("age" in person)         # False
```

The square bracket form `person["age"]` would stop the program with an error. `get()` lets you decide what a missing key means.

Looping:

```python
prices = {"tea": 3, "milk": 2, "jam": 5}
print(list(prices.keys()))  # ['tea', 'milk', 'jam']
for item, price in prices.items():
    print(item, price)
# tea 3
# milk 2
# jam 5
total = sum(prices.values())
print(total)  # 10
```

`values()` is the third view, giving the values only.

Counting with a dictionary is a classic use:

```python
words = ["a", "b", "a", "c", "a", "b"]
counts = {}
for w in words:
    counts[w] = counts.get(w, 0) + 1
print(counts)  # {'a': 3, 'b': 2, 'c': 1}
```

For each word, `get(w, 0)` gives the current count, or 0 for a new word, and 1 is added. 6 words give 3 distinct keys, with counts 3, 2 and 1, which add to 6.

Hashable keys:

```python
grid = {(0, 0): "start", (1, 2): "treasure"}
print(grid[(1, 2)])  # treasure
```

A tuple of numbers works as a key. A list would not: `{[0, 0]: "start"}` stops with an error.

Speed: a lookup by key takes roughly the same short time for 10 entries or 10 million, while searching a list gets slower as the list grows.

| Operation | Code | Missing key |
|---|---|---|
| Read | `d[k]` | Error |
| Read safely | `d.get(k, default)` | The default |
| Test | `k in d` | `False` |
| Set | `d[k] = v` | Creates it |

## A common mistake {#mistake}
Using `d[k]` when the key may be missing. Use `get()` or test with `in` first. Another is changing a dictionary while looping over it, which raises an error. Loop over a copy, such as `list(d)`.

## Key points {#key-points}
- A dictionary maps unique keys to values and finds them quickly.
- Keys must be hashable: numbers, strings and tuples of those, but not lists.
- `keys()`, `values()` and `items()` give views to loop over.
- `get()` gives a default for a missing key.
