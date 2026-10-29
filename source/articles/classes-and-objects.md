# Classes, instances, methods and constructors

**Covers:** Instance, Method, Self, Attribute, Constructor, __init__, bark()

## Bundling data with behavior {#overview}
Suppose your program tracks dogs, each with a name and an age, and each able to bark. With only variables and functions, you would keep lists of names and ages and pass them around together. That gets clumsy fast. A class lets you describe a dog once, and then create as many dogs as you need, each with its own data. Let's build up the vocabulary one piece at a time.

## Instance {#instance}
Think of a class as a blueprint and an instance as a house built from it. Calling `Dog("Rex")` creates an instance whose name is `"Rex"`, kept completely separate from any other `Dog`. You can make thousands of instances from one class.

## Method {#method}
Dogs bark, so the class needs a way to describe that. A method is a function defined inside a class that acts on one specific instance. You call it with a dot, as in `rex.bark()`. Python passes the instance in as the first argument for you, so the method knows which object it is working on.

## Self {#self}
That first argument has a name: `self`. It is the name programmers give by convention to a method's first parameter, and Python fills it in with the instance the method was called on. It is not a keyword, and you could spell it differently, but every Python programmer writes `self`, and any other name would confuse your readers.

## Attribute {#attribute}
An attribute is a piece of data stored on an object, reached with a dot, like `dog.name` or `dog.age`. You can read it, change it and even add new ones later. A method is also an attribute, one whose value happens to be a function.

## Constructor {#constructor}
Where do the starting values come from? From the constructor, a special method called `__init__` in Python. It is called automatically right after a new instance is created, and it prepares the instance, usually by storing the passed-in values as attributes. You never call it by name: writing `Dog("Rex", 3)` calls it for you.

## __init__ {#init}
Python fixes the name `__init__`, with two underscores on each side. Whatever you write in it happens once for each new object, before anything else can use the object. Its first parameter is `self`, followed by the values you want the caller to provide. It does not return anything.

## bark() {#bark}
`bark()` is the sample method used throughout this article: a function defined inside the `Dog` class that returns what the dog says. You call it on one dog, as in `rex.bark()`, and Python passes that dog in as `self`. The name has no special meaning to Python, and any method name would work the same way.

## A worked example {#example}
Let's read a complete class first.

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

Here is what happened:

1. `Dog("Rex", 3)` made a new instance and called `__init__` with `self` pointing at that new object, `name` as `"Rex"` and `age` as 3. The two assignments created the attributes.
2. `Dog("Fido", 5)` made a second, separate instance.
3. `rex.bark()` called the method. Python passed `rex` as `self`, so `self.name` was `"Rex"`.
4. `rex.birthday()` increased only Rex's age. Fido's stayed at 5, because each instance has its own attributes.

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

Attributes can differ between instances, and you can even add them afterwards:

```python
class Dog:
    def __init__(self, name):
        self.name = name

rex = Dog("Rex")
rex.color = "brown"
print(rex.color)  # brown
print(hasattr(Dog("Fido"), "color"))  # False
```

Only `rex` has `color`. This freedom is handy, and it is also risky, because a typo creates a new attribute silently. Creating every attribute inside `__init__` keeps your objects predictable.

A class attribute is shared by all instances, and you write it in the class body:

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

Use instance attributes for data that differs from object to object, and a class attribute for data that is the same for all.

Now compare this with plain data. Without a class, 1,000 dogs need two lists of 1,000 items that you must keep in step, plus a function that takes both. With a class, you have one list of 1,000 `Dog` objects, each carrying its own name, age and behavior.

| Word | What it is | Example |
|---|---|---|
| Class | Blueprint | `Dog` |
| Instance | Object made from it | `rex` |
| Attribute | Data on an object | `rex.age` |
| Method | Function on an object | `rex.bark()` |
| Constructor | Prepares a new object | `__init__` |

## A common mistake {#mistake}
A common slip is leaving `self` out of a method definition. Python still passes the instance, so the call fails with an unexpected extra argument.

A second slip is forgetting `self.` when assigning inside `__init__`. That creates a local variable, which disappears when the method ends.

A third is putting a changeable value, such as a list, in a class attribute, because every instance then shares that one list.

## Key points {#key-points}
- A class is a blueprint, and an instance is one object made from it.
- `__init__` is the constructor. It stores the starting data as attributes.
- A method gets the instance as `self`. `rex.bark()` means `Dog.bark(rex)`.
- Instance attributes differ per object. Class attributes are shared.
