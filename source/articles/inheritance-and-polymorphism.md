# Inheritance, overriding and polymorphism

**Covers:** Inheritance, Overriding, Polymorphism, Animal, Cat, super()

## Reusing a class and changing parts of it {#overview}
Many classes share most of their content. Cats and dogs both have a name and make a sound. Writing the common part twice invites mistakes. Inheritance lets a new class start from an existing one, and change only what differs.

## Animal {#animal}
`Animal` is the sample base class used in these examples. It holds what all animals have in common, such as a name and a `speak` method. It is only a teaching name. In real programs, base classes are named after the idea they represent.

## Cat {#cat}
`Cat` is a sample class that inherits from `Animal`. It keeps everything the base class provides, and adds or changes only what is special about cats.

## Inheritance {#inheritance}
Inheritance is a mechanism in which one class, the subclass, is defined in terms of another, its parent class, and automatically gets all of the parent's attributes and methods. You write the parent's name in parentheses: `class Cat(Animal):`. The subclass can then add new features of its own. Use it for an "is a" relation: a cat is an animal.

## Overriding {#overriding}
Overriding is when a subclass defines its own version of a method that its parent already has, so that calling that method on the subclass uses the new version instead. The name stays the same, and the behavior changes.

## super() {#super}
`super()` gives access to a parent class's methods from inside the subclass. Its most common use is in `__init__`: the subclass calls `super().__init__(...)` so that the parent's setup still runs, and then adds its own.

## Polymorphism {#polymorphism}
Polymorphism means that different classes can respond to the same method call, each in its own way. Calling `.speak()` works on any animal, whatever its class, and each gives its own result. The calling code does not need to know which kind it holds.

## A worked example {#example}
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

What the example shows:

1. **Inheritance.** `Dog` and `Cat` get `__init__` and `speak` from `Animal` without rewriting the name handling.
2. **Overriding.** Both subclasses define `speak` again. `Animal("Generic").speak()` uses the base version, and the others use their own.
3. **`super()`.** `Cat` needs an extra value, `indoor`. Its `__init__` calls `super().__init__(name)` so the base class stores the name, and then sets its own attribute. Without that call, `self.name` would never be set, and `speak` would fail.
4. **Polymorphism.** The loop calls `pet.speak()` for each pet and does not check the kind. Adding a `Bird` class later requires no change to the loop.

Check the relationship:

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

A `Cat` is also an `Animal`, so it can be used wherever an `Animal` is expected.

Count the saving. If ten animal classes each repeated the 4-line name setup, that is 40 lines, with the same bug fixable in ten places. With a base class, 4 lines in one place.

Without polymorphism, the loop would need a chain of checks:

```text
if kind == "dog": ...
elif kind == "cat": ...
elif kind == "bird": ...
```

and every new kind means editing that chain. With it, new kinds are added as new classes, and the loop is untouched.

A word of caution about inheritance. It is a strong link: changing the parent can break every subclass. Deep chains of many levels are hard to follow. Prefer shallow hierarchies, and when the relation is "has a" and not "is a", store the other object as an attribute instead.

| Idea | In one line |
|---|---|
| Inheritance | `class Cat(Animal):` reuses the parent |
| Overriding | A subclass redefines a parent method |
| `super()` | Runs the parent's version from inside |
| Polymorphism | One call, many behaviors |

## A common mistake {#mistake}
Overriding `__init__` without calling `super().__init__()`, so the parent's attributes are never set. Another is using inheritance only to share a few lines of code, between classes that are not really the same kind of thing.

## Key points {#key-points}
- A subclass inherits attributes and methods from its parent.
- Overriding replaces a method in the subclass. `super()` reaches the parent's version.
- Polymorphism lets code call the same method on different classes.
- Use inheritance for "is a" relations.
