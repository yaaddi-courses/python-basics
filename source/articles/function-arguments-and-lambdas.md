# Function arguments and lambdas

**Covers:** Positional argument, Lambda

## Giving a function its inputs {#overview}
A function is useful because it can work on different inputs each time. Python gives you several ways to pass those inputs in. It also lets you write a tiny function in one line, for the moments when a full definition would feel too heavy. Let's take the inputs first.

## Positional argument {#positional-argument}
A positional argument is a value you pass in a function call, and Python matches it to a parameter by its order, not by its name. The first value fills the first parameter, the second fills the second, and so on. If you swap two positional values, you change the meaning of the call.

The alternative is a keyword argument, written `name=value`. Python matches it by name, so it can come in any order. In a call, positional arguments must come before keyword arguments. Parameters can also have default values, which are used when the caller does not supply one.

## Lambda {#lambda}
A lambda is a small, unnamed function with a single expression, written `lambda x: x * 2`. It suits short, throwaway logic that does not deserve a full `def` and a name. A lambda can take several parameters, as in `lambda a, b: a + b`, and its result is the value of its one expression. It cannot contain statements such as assignments or loops.

## A worked example {#example}
Let's start with a function that has three parameters:

```python
def describe(name, age, city):
    return f"{name} is {age} and lives in {city}"

print(describe("Ada", 36, "London"))  # Ada is 36 and lives in London
print(describe("London", 36, "Ada"))  # London is 36 and lives in Ada
```

In the second call, the values went in the wrong order, and the function cannot know. The result is nonsense, with no error. With keyword arguments, the order stops mattering:

```python
def describe(name, age, city):
    return f"{name} is {age} and lives in {city}"

print(describe(city="London", name="Ada", age=36))  # Ada is 36 and lives in London
```

You can mix both, with positional values first:

```python
def describe(name, age, city="Paris"):
    return f"{name} is {age} and lives in {city}"

print(describe("Ada", 36))            # Ada is 36 and lives in Paris
print(describe("Ada", 36, "Rome"))    # Ada is 36 and lives in Rome
print(describe("Ada", 36, city="Rome"))  # Ada is 36 and lives in Rome
```

The default for `city` is used only when the caller leaves it out. Writing `describe(age=36, "Ada")` would be a syntax error, because a positional value cannot follow a keyword one.

You can also collect any number of arguments:

```python
def total(*numbers):
    return sum(numbers)

print(total(1, 2, 3))  # 6
print(total())         # 0
```

`*numbers` gathers all the positional values into a tuple. `total()` with no values gives 0.

Now lambdas. Here is the same function in two forms:

```python
def double(x):
    return x * 2

double2 = lambda x: x * 2
print(double(4))   # 8
print(double2(4))  # 8
```

They are equivalent. The real use of a lambda is to pass it directly to something that wants a function:

```python
words = ["banana", "fig", "apple"]
print(sorted(words, key=lambda w: len(w)))  # ['fig', 'apple', 'banana']
```

The lambda says "sort by length". Without it, you would write a named function that you use once. Here is a lambda with several parameters:

```python
add = lambda a, b: a + b
print(add(2, 3))  # 5
```

When should you avoid a lambda? If it needs more than a short expression, or if you assign it to a name as above, a normal `def` is clearer and gives better error messages. In the example, `double2` appears only for comparison.

Let's weigh the readability cost. A named function takes 2 lines and a name. A lambda takes 1 inline expression. For a single use, the lambda is shorter. For five uses, the named function is better, because the logic sits in one place.

| Style | Matched by | Good for |
|---|---|---|
| Positional | Order | Few, obvious parameters |
| Keyword | Name | Many parameters, clarity |
| Default | Skipped | Optional settings |
| `*args` | Order, any number | Unknown count of values |

## A common mistake {#mistake}
A common slip is passing many positional values in an unclear order, so that two of the same type get swapped. Use keywords when a call has more than two or three parameters.

A second slip is writing a changeable default such as `def f(x=[])`. That one list is shared between calls.

## Key points {#key-points}
- Positional arguments are matched by order. Keyword arguments are matched by name.
- Defaults fill in what the caller omits. Positional values come first in a call.
- A lambda is a one-expression function without a name.
- Use a lambda for short, one-time logic. Use `def` otherwise.
