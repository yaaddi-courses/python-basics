# Classes, instances, methods and constructors

**Covers:** Instance, Method, Self, Attribute, Constructor, __init__

## Bundling data with behavior {#overview}
Suppose a program tracks dogs, each with a name and an age, and each able to bark. With only variables and functions, you would keep lists of names and ages and pass them around. A class lets you describe a dog once, and then create as many as you need, each with its own data.

## Instance {#instance}
An instance is one specific object built from a class. Calling `Dog("Rex")` creates an instance whose name is `"Rex"`, kept completely separate from any other `Dog`. The class is the blueprint, and an instance is a thing made from it. You can make thousands of instances from one class.

## Method {#method}
A method is a function defined inside a class that acts on one specific instance. You call it with a dot, as in `rex.bark()`. Python automatically passes the instance in as the first argument, so the method knows which object it is working on.

## Self {#self}
`self` is the name given by convention to a method's first parameter. Python fills it in with the instance that the method was called on. It is not a keyword, and could be spelled differently, but every Python programmer writes `self`, and other names confuse readers.

## Attribute {#attribute}
An attribute is a piece of data stored on an object, reached with a dot, like `dog.name` or `dog.age`. A method is really just an attribute that happens to be a function. Attributes can be read, changed, and even added later.

## Constructor {#constructor}
A constructor is a special method, called `__init__` in Python, that runs automatically right after a new instance is created. It sets the instance up, usually by storing the values passed in as attributes. You do not call it by name: writing `Dog("Rex", 3)` calls it for you.

## __init__ {#init}
The name `__init__` is fixed by Python, with two underscores on each side. Whatever you write in it runs once for each new object, before anything else can use the object. Its first parameter is `self`, followed by the values you want the caller to provide. It does not return anything.

## A worked example {#example}
```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def bark(self):
        return f"{self.name} says woof"

    def birthday(self):
        self.age += 1

rex = Dog("Rex", 3)
fido = Dog("Fido", 5)
print(rex.name)    # Rex
print(rex.bark())  # Rex says woof
rex.birthday()
print(rex.age)     # 4
print(fido.age)    # 5
```

What happened:

1. `Dog("Rex", 3)` made a new instance and ran `__init__` with `self` set to that new object, `name` to `"Rex"` and `age` to 3. The two assignments created the attributes.
2. `Dog("Fido", 5)` made a second, separate instance.
3. `rex.bark()` called the method. Python passed `rex` as `self`, so `self.name` was `"Rex"`.
4. `rex.birthday()` increased only Rex's age. Fido's stayed at 5. Each instance has its own attributes.

The call `rex.bark()` is shorthand for `Dog.bark(rex)`. You can write the second form and get the same result, which shows what `self` really is:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):
        return f"{self.name} says woof"

rex = Dog("Rex")
print(rex.bark())      # Rex says woof
print(Dog.bark(rex))   # Rex says woof
```

Attributes can differ between instances, and can even be added afterwards:

```python
class Dog:
    def __init__(self, name):
        self.name = name

rex = Dog("Rex")
rex.color = "brown"
print(rex.color)  # brown
print(hasattr(Dog("Fido"), "color"))  # False
```

Only `rex` has `color`. This flexibility is handy, and risky: a typo creates a new attribute silently. Setting every attribute in `__init__` keeps objects predictable.

A class attribute is shared by all instances, and is defined in the class body:

```python
class Dog:
    species = "dog"

    def __init__(self, name):
        self.name = name

a = Dog("Rex")
b = Dog("Fido")
print(a.species, b.species)  # dog dog
print(Dog.species)           # dog
```

Use instance attributes for data that differs, and a class attribute for data that is the same for all.

Compare with plain data. Without a class, 1,000 dogs need two lists of 1,000 items that must be kept in step, and a function that takes both. With a class, a list of 1,000 `Dog` objects, each carrying its own name, age and behavior.

| Word | What it is | Example |
|---|---|---|
| Class | Blueprint | `Dog` |
| Instance | Object made from it | `rex` |
| Attribute | Data on an object | `rex.age` |
| Method | Function on an object | `rex.bark()` |
| Constructor | Sets up a new object | `__init__` |

## A common mistake {#mistake}
Leaving out `self` in a method definition, so that Python passes the instance as an unexpected extra argument, and the call fails. Another is forgetting `self.` when assigning in `__init__`, which creates a local variable that disappears. And mutable class attributes, such as a list shared by all instances.

## Key points {#key-points}
- A class is a blueprint, and an instance is one object made from it.
- `__init__` is the constructor. It stores the starting data as attributes.
- A method gets the instance as `self`. `rex.bark()` means `Dog.bark(rex)`.
- Instance attributes differ per object. Class attributes are shared.
