# Variables, functions, classes and modules

**Covers:** Variable, Function, Class, Module

## Four ways to organize a program {#overview}
A program is a sequence of instructions, but a long sequence is hard to understand and harder to change. Python gives you four tools to give your code structure: variables to name values, functions to name actions, classes to name kinds of things, and modules to name files. This article introduces all four in the order you will meet them.

## Variable {#variable}
A variable is a name you attach to a value, so that you can refer to it later. In Python you do not declare a type in advance, and the same name can later hold a value of a different type. Assignment uses a single equals sign: `price = 20`. To read the value back, you use the name.

A variable does not contain the value the way a box contains an item. It points at it. Two names can point at the same value, which matters later when values can be changed.

Names may contain letters, digits and underscores, and they cannot start with a digit. Python treats `Price` and `price` as different names. By convention, ordinary variables are written in lowercase with underscores.

## Function {#function}
A function is a reusable, named block of code that can take inputs and hand back a result with `return`, so you do not rewrite the same logic every time. You define it once with `def`, and you use it as often as you like by writing its name followed by parentheses. Inside the definition, the inputs are called parameters.

A function without `return` still finishes, but it gives back the special value `None`. If you name a function after what it does, such as `total_price`, the calling code reads like a sentence.

## Class {#class}
A class is a blueprint for creating objects that share the same data and behavior. You define it once with `class`, and then create as many individual objects from it as you need. A `Dog` class, for example, can describe what every dog has and does, and each dog you create is separate. Later in the course, classes get a full treatment. For now, think of them as a way to bundle data with the functions that work on it.

## Module {#module}
A module is a single file of Python code that you can import and reuse elsewhere with `import`, instead of copying its content wherever you need it. The standard library is a large collection of modules, such as `math` and `random`. You can write your own too, because any file ending in `.py` is a module.

## A worked example {#example}
Let's build a small program that computes the price of an order, step by step.

```python
price = 20
quantity = 3
total = price * quantity
print(total)  # 60
```

Here `price`, `quantity` and `total` are variables. Next comes a function, so the logic can be reused:

```python
def order_total(price, quantity):
    return price * quantity

print(order_total(20, 3))  # 60
print(order_total(5, 10))  # 50
```

There is one definition and two uses, with different inputs. If the rule changes, for example because you add a discount, only the function changes. Suppose the program calls the calculation in 12 places. Without a function, you need 12 edits, and missing one gives a wrong price. With a function, you need 1 edit.

A class bundles the data and the action:

```python
class Order:
    def __init__(self, price, quantity):
        self.price = price
        self.quantity = quantity

    def total(self):
        return self.price * self.quantity

order = Order(20, 3)
print(order.total())  # 60
```

Do not worry about the details of the special first lines yet. The idea is that `order` is one object made from the blueprint `Order`.

Finally, a module. Here we use something from the standard library:

```python
import math

print(math.sqrt(81))  # 9.0
```

Other people wrote the file `math`, and you use it here without copying it. Suppose you save the function `order_total` in a file `pricing.py`. In another file, `from pricing import order_total` makes it available. The program is now split into files that each have one job.

| Tool | Names | Use it to |
|---|---|---|
| Variable | A value | Remember something |
| Function | An action | Reuse logic |
| Class | A kind of thing | Bundle data and actions |
| Module | A file | Share code between files |

## A common mistake {#mistake}
A common slip is using one-letter names such as `x` and `p` throughout a long program, so nobody remembers what they mean.

A second slip is copying and pasting the same lines in several places instead of making a function. A third is giving a variable the name of a built-in, such as `sum`, which hides the built-in from the rest of the file.

## Key points {#key-points}
- A variable names a value. In Python it has no declared type.
- A function packages reusable logic. It gives back a result with `return`.
- A class is a blueprint for objects. A module is a file you can import.
- Make a function when you notice yourself copying code.
