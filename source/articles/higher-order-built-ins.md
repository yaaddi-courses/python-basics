# Higher-order built-ins: map, filter, zip, enumerate, any and all

**Covers:** Higher-order function, map(), filter(), zip(), enumerate(), any(), all()

## Functions that work on whole collections {#overview}
Python has a group of built-in tools that take a collection and process it item by item, without a visible loop. Some take another function as an argument. Learning them shortens code and, often, makes its purpose clearer.

## Higher-order function {#higher-order-function}
A higher-order function is a function that takes another function as one of its arguments, or returns a function as its result. `map()` and `filter()` are examples, and so is `sorted()` when used with its `key=` argument. The idea is that behavior can be passed around like data.

## map() {#map}
`map()` applies a function to every item of an iterable. It returns a lazy object, so wrap it in `list()` to see the results.

## filter() {#filter}
`filter()` keeps only the items for which a function returns true. Like `map()`, it is lazy.

## zip() {#zip}
`zip()` pairs up items from several iterables: the first of each, then the second of each, and so on. It stops at the shortest one.

## enumerate() {#enumerate}
`enumerate()` yields each item together with its index. It is the clean replacement for keeping your own counter in a loop. A second argument sets the first number.

## any() {#any}
`any()` returns `True` if at least one item is true. For an empty collection, it returns `False`.

## all() {#all}
`all()` returns `True` only if every item is true. For an empty collection, it returns `True`, since nothing contradicts it.

## A worked example {#example}
The same task in two styles. Square each number:

```python
numbers = [1, 2, 3, 4]
print(list(map(lambda n: n * n, numbers)))  # [1, 4, 9, 16]
print([n * n for n in numbers])             # [1, 4, 9, 16]
```

A comprehension does the same, and many people prefer it. Know both, since you will meet both.

Filtering:

```python
numbers = [1, 2, 3, 4, 5, 6]
evens = list(filter(lambda n: n % 2 == 0, numbers))
print(evens)  # [2, 4, 6]
```

Three of the six items pass.

Pairing with `zip()`:

```python
names = ["Ada", "Bo", "Cy"]
ages = [36, 28, 41]
print(list(zip(names, ages)))  # [('Ada', 36), ('Bo', 28), ('Cy', 41)]
print(dict(zip(names, ages)))  # {'Ada': 36, 'Bo': 28, 'Cy': 41}
```

If one list is shorter, the extra items of the longer one are dropped. With 3 names and 2 ages, you would get 2 pairs.

Numbering with `enumerate()`:

```python
for index, name in enumerate(["Ada", "Bo", "Cy"], start=1):
    print(index, name)
# 1 Ada
# 2 Bo
# 3 Cy
```

Compare the manual way, with a counter variable that you must create, increase and remember. `enumerate()` has no such risk of forgetting the increase.

Testing a whole collection:

```python
ages = [17, 22, 30]
print(any(a >= 18 for a in ages))  # True
print(all(a >= 18 for a in ages))  # False
print(any([]))  # False
print(all([]))  # True
```

At least one person is an adult, but not all. For an empty list, `any` is false and `all` is true. The second surprises people, but it follows logic: a claim about "every item" cannot be broken by an item that does not exist.

Chain them:

```python
prices = [5, 12, 8, 20]
total_cheap = sum(filter(lambda p: p < 10, prices))
print(total_cheap)  # 13
```

The items under 10 are 5 and 8, which add up to 13.

| Tool | Takes a function? | Result |
|---|---|---|
| `map(f, xs)` | Yes | `f` applied to each item |
| `filter(f, xs)` | Yes | Items for which `f` is true |
| `zip(a, b)` | No | Pairs |
| `enumerate(xs)` | No | Pairs of index and item |
| `any(xs)` / `all(xs)` | No | One true or false |

## A common mistake {#mistake}
Printing a `map` or `filter` object and seeing something like `<map object at 0x...>`. They are lazy. Wrap them in `list()`. Another is expecting `zip` to keep all items of the longer input. It stops at the shortest.

## Key points {#key-points}
- A higher-order function takes or returns a function.
- `map` transforms, `filter` selects. Both are lazy.
- `zip` pairs inputs. `enumerate` adds an index.
- `any` is true if some item is true. `all` is true if every item is, and is true for an empty collection.
