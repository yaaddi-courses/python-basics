# Iteration, iterables, comprehensions and generators

**Covers:** Iteration, Iterable, List comprehension, Generator

## Doing something for every item {#overview}
Repetition is what computers are for. Python gives you `for` loops to go through the items of a collection, and a few shorter or more economical forms for the common cases. In this article we move from the basic idea to the tools that make loops compact and gentle on memory.

## Iteration {#iteration}
Iteration means repeating a block of code once for every item in a sequence, such as doing the same thing for each number in a list. Each single pass through the loop body is one iteration. A `for` loop takes the items one at a time: `for item in items:`. A `while` loop repeats as long as a condition holds.

## Iterable {#iterable}
What can you put after `in`? Any iterable, which is any object that can hand out its items one by one. Lists, tuples, strings, dictionaries and ranges all qualify, and the `for` loop does not care which kind it receives. A string gives its characters, a dictionary gives its keys, and a range gives numbers.

## List comprehension {#list-comprehension}
Building a new list with a loop takes several lines. A list comprehension does it in one: `[expression for item in iterable]`. You can add an `if` at the end to keep only some items. It does the work of a loop that creates an empty list and appends to it.

## Generator {#generator}
A generator produces values one at a time, on demand, instead of building the whole sequence in memory first. If you put parentheses around a comprehension instead of square brackets, you get a generator expression. You can also write generators as functions that use `yield`. A generator can be used only once, because after it is exhausted it is empty.

## A worked example {#example}
Let's see a loop and its comprehension, side by side.

```python
squares = []
for n in range(5):
    squares.append(n * n)
print(squares)  # [0, 1, 4, 9, 16]

squares2 = [n * n for n in range(5)]
print(squares2)  # [0, 1, 4, 9, 16]
```

`range(5)` gives 0, 1, 2, 3, 4, which is five iterations. Both versions give the same list. The second has 1 line instead of 3.

Now add a filter:

```python
evens = [n for n in range(10) if n % 2 == 0]
print(evens)  # [0, 2, 4, 6, 8]
```

The `if` keeps an item only when its condition passes. Of 10 numbers, 5 pass.

Any iterable can go on the right:

```python
word = "hello"
print([ch.upper() for ch in word])  # ['H', 'E', 'L', 'L', 'O']
```

Now the generator, and why it matters for memory.

```python
import sys

as_list = [n * n for n in range(100000)]
as_gen = (n * n for n in range(100000))
print(sys.getsizeof(as_list) > 100 * sys.getsizeof(as_gen))  # True
print(sum(as_gen))  # 333328333350000
```

The list stores 100,000 numbers at once and uses hundreds of kilobytes. The generator stores only its place in the sequence, a couple of hundred bytes, and creates each square when asked. The sum is computed by asking for the values one by one.

A generator is used up after one pass:

```python
gen = (n for n in range(3))
print(list(gen))  # [0, 1, 2]
print(list(gen))  # []
```

The second time, nothing is left. If you need the values twice, build a list.

A generator function with `yield` looks like this:

```python
def countdown(start):
    while start > 0:
        yield start
        start -= 1

print(list(countdown(3)))  # [3, 2, 1]
```

Let's compare memory in a realistic case. Reading a file with 10 million lines into a list needs space for all 10 million at once, perhaps 1 GB. Processing it line by line with a generator needs space for one line at a time, a few hundred bytes. The result is the same, and the cost is very different.

| Tool | Builds | Memory | Use when |
|---|---|---|---|
| Loop with `append` | A list | All items | Several steps per item |
| List comprehension | A list | All items | One simple expression |
| Generator expression | Nothing yet | One item | Large data, one pass |

## A common mistake {#mistake}
A common slip is making a comprehension so long that nobody can read it, with nested loops and several conditions. Switch to a normal loop then.

A second slip is trying to read a generator twice and being surprised by an empty result.

## Key points {#key-points}
- Iteration repeats code for each item. Any iterable can be looped over.
- A list comprehension builds a list in one line, with an optional `if`.
- A generator makes values on demand and can be used once.
- Prefer a generator for large data that you read in one pass.
