# Inheritance, overriding and polymorphism

**Covers:** Inheritance, Overriding, Polymorphism, Animal, Cat, super()

## Reusing a class and changing parts of it {#overview}
Many classes share most of their content. Cats and dogs both have a name and make a sound. If you write the common part twice, you invite mistakes, because the two copies drift apart. Inheritance solves this: a new class starts from an existing one and changes only what differs. Let's see how that works.

## Animal {#animal}
In these examples, `Animal` is the base class. It holds what all animals have in common, such as a name and a `speak` method. The name is only for teaching. In real programs, you name a base class after the idea it represents.

## Cat {#cat}
`Cat` is a sample class that inherits from `Animal`. It keeps everything the base class provides, and it adds or changes only what is special about cats.

## Inheritance {#inheritance}
With inheritance, one class, the subclass, is defined in terms of another, its parent class, and it automatically gets all of the parent's attributes and methods. You write the parent's name in parentheses: `class Cat(Animal):`. The subclass can then add features of its own. Use it for an "is a" relation: a cat is an animal.

## Overriding {#overriding}
Sometimes the subclass needs different behavior for something the parent already does. Overriding means the subclass defines its own version of a method that the parent has, so calling that method on the subclass uses the new version. The name stays the same and the behavior changes.

## super() {#super}
When you override, you often still want the parent's version to happen too. `super()` gives you access to the parent class's methods from inside the subclass. The most common use is in `__init__`: the subclass calls `super().__init__(...)` so the parent's preparation still happens, and then adds its own.

## Polymorphism {#polymorphism}
Now the payoff. Polymorphism means that different classes can respond to the same method call, each in its own way. Calling `.speak()` works on any animal, whatever its class, and each one gives its own result. The calling code does not need to know which kind it holds.

## A worked example {#example}
Let's read the whole picture first:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return f"{self.name} makes a sound"

class Dog(Animal):
    def speak(self):
        return f"{self.name} says woof"

class Cat(Animal):
    def __init__(self, name, indoor):
        super().__init__(name)
        self.indoor = indoor

    def speak(self):
        return f"{self.name} says meow"

pets = [Animal("Generic"), Dog("Rex"), Cat("Tom", True)]
for pet in pets:
    print(pet.speak())
# Generic makes a sound
# Rex says woof
# Tom says meow
print(pets[2].indoor)  # True
```

Here is what the example shows:

1. **Inheritance.** `Dog` and `Cat` get `__init__` and `speak` from `Animal` without rewriting the name handling.
2. **Overriding.** Both subclasses define `speak` again. `Animal("Generic").speak()` uses the base version, and the others use their own.
3. **`super()`.** `Cat` needs an extra value, `indoor`. Its `__init__` calls `super().__init__(name)` so the base class stores the name, and then it stores its own attribute. Without that call, `self.name` would never exist, and `speak` would fail.
4. **Polymorphism.** The loop calls `pet.speak()` for each pet and never checks the kind. If you add a `Bird` class later, the loop needs no change.

You can check the relationship:

```python
class Animal:
    pass

class Cat(Animal):
    pass

tom = Cat()
print(isinstance(tom, Cat))     # True
print(isinstance(tom, Animal))  # True
print(issubclass(Cat, Animal))  # True
```

A `Cat` is also an `Animal`, so you can use it wherever an `Animal` is expected.

Let's count the saving. If ten animal classes each repeated the 4-line name setup, that is 40 lines, and a bug in it must be fixed in ten places. With a base class, it is 4 lines in one place.

Without polymorphism, the loop would need a chain of checks:

```text
if kind == "dog": ...
elif kind == "cat": ...
elif kind == "bird": ...
```

and every new kind would mean editing that chain. With polymorphism, you add new kinds as new classes and leave the loop untouched.

A word of caution about inheritance. It is a strong link, so changing the parent can break every subclass, and deep chains of many levels are hard to follow. Prefer shallow hierarchies. When the relation is "has a" instead of "is a", store the other object as an attribute.

| Idea | In one line |
|---|---|
| Inheritance | `class Cat(Animal):` reuses the parent |
| Overriding | A subclass redefines a parent method |
| `super()` | Reaches the parent's version from inside |
| Polymorphism | One call, many behaviors |

## A common mistake {#mistake}
A common slip is overriding `__init__` without calling `super().__init__()`, so the parent's attributes never exist.

A second slip is using inheritance only to share a few lines of code between classes that are not really the same kind of thing.

## Key points {#key-points}
- A subclass inherits attributes and methods from its parent.
- Overriding replaces a method in the subclass. `super()` reaches the parent's version.
- Polymorphism lets code call the same method on different classes.
- Use inheritance for "is a" relations.
