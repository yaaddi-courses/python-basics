# Function arguments and lambdas

**Covers:** Positional argument, Lambda

## Giving a function its inputs {#overview}
A function is useful because it works on different inputs each time. Python lets you pass them in several ways, and also lets you write a tiny function in one line when a full definition would be too heavy.

## Positional argument {#positional-argument}
A positional argument is a value passed into a function call that is matched to a parameter by its order, not by its name. The first value fills the first parameter, the second fills the second, and so on. Swapping two positional values changes the meaning of the call.

The alternative is a keyword argument, written `name=value`, which is matched by name and can come in any order. Positional arguments must come before keyword arguments in a call. Parameters can also have default values, used when the caller does not supply them.

## Lambda {#lambda}
A lambda is a small, unnamed, single-expression function, written `lambda x: x * 2`. It is useful for short, throwaway logic that does not deserve a full `def` and a name. A lambda can take several parameters, `lambda a, b: a + b`, and its result is the value of its one expression. It cannot contain statements such as assignments or loops.

## A worked example {#example}
A function with three parameters:

```python
def describe(name, age, city):
    return f"{name} is {age} and lives in {city}"

print(describe("Ada", 36, "London"))  # Ada is 36 and lives in London
print(describe("London", 36, "Ada"))  # London is 36 and lives in Ada
```

In the second call, the values were passed in the wrong order, and the function cannot know. The result is nonsense, without any error. With keyword arguments, order does not matter:

```python
def describe(name, age, city):
    return f"{name} is {age} and lives in {city}"

print(describe(city="London", name="Ada", age=36))  # Ada is 36 and lives in London
```

Mix both, with positional values first:

```python
def describe(name, age, city="Paris"):
    return f"{name} is {age} and lives in {city}"

print(describe("Ada", 36))            # Ada is 36 and lives in Paris
print(describe("Ada", 36, "Rome"))    # Ada is 36 and lives in Rome
print(describe("Ada", 36, city="Rome"))  # Ada is 36 and lives in Rome
```

The default for `city` is used only when the caller leaves it out. Writing `describe(age=36, "Ada")` would be a syntax error, since a positional value cannot follow a keyword one.

Collecting any number of arguments:

```python
def total(*numbers):
    return sum(numbers)

print(total(1, 2, 3))  # 6
print(total())         # 0
```

`*numbers` gathers all the positional values into a tuple. `total()` with no values gives 0.

Now lambdas. The same function in two forms:

```python
def double(x):
    return x * 2

double2 = lambda x: x * 2
print(double(4))   # 8
print(double2(4))  # 8
```

They are equivalent. The real use of a lambda is passing it directly to something that wants a function:

```python
words = ["banana", "fig", "apple"]
print(sorted(words, key=lambda w: len(w)))  # ['fig', 'apple', 'banana']
```

The lambda says "sort by length". Without it, you would write a named function that is used once. A lambda with several parameters:

```python
add = lambda a, b: a + b
print(add(2, 3))  # 5
```

When not to use a lambda: if it needs more than a short expression, or if you assign it to a name as above, a normal `def` is clearer and gives better error messages. In the example, `double2` is shown only for comparison.

Count the readability cost. A named function costs 2 lines and a name. A lambda costs 1 inline expression. For one use it is shorter. For five uses, the named function is better, since the logic sits in one place.

| Style | Matched by | Good for |
|---|---|---|
| Positional | Order | Few, obvious parameters |
| Keyword | Name | Many parameters, clarity |
| Default | Skipped | Optional settings |
| `*args` | Order, any number | Unknown count of values |

## A common mistake {#mistake}
Passing many positional values in an unclear order, so that two of the same type are swapped. Use keywords for calls with more than two or three parameters. Another is writing a mutable default such as `def f(x=[])`, which is shared between calls.

## Key points {#key-points}
- Positional arguments are matched by order. Keyword arguments are matched by name.
- Defaults fill in what the caller omits. Positional values come first in a call.
- A lambda is a one-expression function without a name.
- Use a lambda for short, one-time logic. Use `def` otherwise.
