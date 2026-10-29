# Dictionaries and hashable keys

**Covers:** Dictionary, Hashable, Ada, keys(), items(), get()

## Looking things up by name {#overview}
A list finds things by position. But often the natural question is different: what is the phone number of Ada? You do not care that she is the third entry. You care about her name. A dictionary answers this question directly, by taking a key and returning the value stored with it. Let's see how it works.

## Dictionary {#dictionary}
A dictionary maps keys to values, and you write it as `key: value` pairs inside curly braces. You look a value up by its key, never by position. Keys are unique, so assigning to an existing key replaces its value. A dictionary remembers the order in which keys were first added, and it is mutable, which means you can add, change and remove entries.

## Hashable {#hashable}
Not every value can be a key. A key must be hashable, which means its value can never change. The same rule applies to the items of a set. Numbers, strings and tuples of such values are hashable. Lists are not, because a list can change after it was used as a key, and then the dictionary could not find it again. Python raises an error if you try.

## Ada {#ada}
You will see the name Ada in many dictionary examples, such as `{"name": "Ada"}`. It refers to Ada Lovelace, an early programmer. It has no special meaning to Python, and any other string would behave the same way.

## keys() {#keys}
`keys()` gives you a view of all the keys of a dictionary, and it is also what you get when you loop over a dictionary directly. A view is live: if the dictionary changes afterwards, the view shows the change.

## items() {#items}
`items()` gives you a view of all the key and value pairs, as tuples. It is the usual way to loop over both parts at once: `for key, value in d.items():`.

## get() {#get}
What if the key you ask for is missing? The square bracket form raises an error. `get()` is the gentler alternative. It returns the value for a key, or a default if the key is missing: `d.get("age")` gives `None` when there is no such key, and `d.get("age", 0)` gives 0.

## A worked example {#example}
Let's start with a small record:

```python
person = {"name": "Ada", "born": 1815}
print(person["name"])  # Ada
person["city"] = "London"
person["born"] = 1816
print(person)  # {'name': 'Ada', 'born': 1816, 'city': 'London'}
print(len(person))  # 3
```

Adding `city` created a new entry. Assigning to `born` replaced the old value, so there are still 3 entries, not 4.

Now look up a missing key:

```python
person = {"name": "Ada"}
print(person.get("age"))       # None
print(person.get("age", 0))    # 0
print("age" in person)         # False
```

The square bracket form `person["age"]` would stop the program with an error. `get()` lets you decide what a missing key means.

Next, looping:

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

`values()` is the third view, and it gives the values only.

Counting with a dictionary is a classic use:

```python
words = ["a", "b", "a", "c", "a", "b"]
counts = {}
for w in words:
    counts[w] = counts.get(w, 0) + 1
print(counts)  # {'a': 3, 'b': 2, 'c': 1}
```

For each word, `get(w, 0)` gives the current count, or 0 for a new word, and then 1 is added. Six words give 3 distinct keys with counts 3, 2 and 1, which add up to 6.

Now hashable keys:

```python
grid = {(0, 0): "start", (1, 2): "treasure"}
print(grid[(1, 2)])  # treasure
```

A tuple of numbers works as a key. A list would not: `{[0, 0]: "start"}` stops with an error.

A word about speed. A lookup by key takes about the same short time for 10 entries or 10 million, while searching a list gets slower as the list grows.

| Operation | Code | Missing key |
|---|---|---|
| Read | `d[k]` | Error |
| Read safely | `d.get(k, default)` | The default |
| Test | `k in d` | `False` |
| Assign | `d[k] = v` | Creates it |

## A common mistake {#mistake}
A common slip is using `d[k]` when the key may be missing. Use `get()`, or test with `in` first.

A second slip is changing a dictionary while looping over it, which raises an error. Loop over a copy, such as `list(d)`.

## Key points {#key-points}
- A dictionary maps unique keys to values and finds them quickly.
- Keys must be hashable: numbers, strings and tuples of those, but not lists.
- `keys()`, `values()` and `items()` give views to loop over.
- `get()` gives a default for a missing key.
