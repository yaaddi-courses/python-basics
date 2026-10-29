# Encapsulation, name mangling and decorators

**Covers:** Encapsulation, Name mangling, Decorator, _name

## Protecting the inside of an object {#overview}
A class that exposes all of its data lets any code change it in any way, including ways that break its rules. A bank account should not accept a negative deposit. Encapsulation is the practice of keeping the data behind a controlled door. Python takes a relaxed approach: it relies on conventions and offers tools to help.

## Encapsulation {#encapsulation}
Encapsulation means bundling an object's data together with the methods that work on it inside a class, and hiding the internal details from outside code that does not need them. Outside code uses the public methods, and does not touch the inner variables. This lets you change the inside later without breaking the users.

## _name {#underscore-name}
A name starting with one underscore, such as `_balance`, marks an attribute as non-public by convention. Python does not stop anyone from using it. It is a signal to other programmers: "this is internal, do not rely on it". Tools and code reviewers respect it.

## Name mangling {#name-mangling}
Name mangling is what Python does to an attribute whose name starts with two underscores, like `__balance`: it renames it internally, to `_ClassName__balance`. This makes accidental use from outside, or from a subclass, harder, though not impossible. It is meant to avoid name clashes in subclasses, and is not a security feature.

## Decorator {#decorator}
A decorator is a function written with an `@` symbol right above another function or method, which wraps it and changes how it behaves without editing its code. The best-known example is `@property`, which lets a method be read like an attribute, so that `account.balance` runs code.

## A worked example {#example}
A bank account with a protected balance:

```python
class Account:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("deposit must be positive")
        self._balance += amount

acct = Account("Ada", 100)
acct.deposit(50)
print(acct.balance)  # 150
try:
    acct.deposit(-10)
except ValueError as error:
    print(error)  # deposit must be positive
try:
    acct.balance = 1000000
except AttributeError:
    print("cannot set balance directly")  # cannot set balance directly
```

What each part does:

1. `_balance` is the real value, marked as internal by its underscore.
2. `@property` makes `balance` readable like an attribute (`acct.balance`, no parentheses). Since no setter is defined, assigning to it fails.
3. `deposit` is the only door for changing the money, and it checks the rule. A negative deposit raises an error.

The rule is kept in one place. If the bank later adds a limit of 10,000 per deposit, only `deposit` changes, and 100 places that use accounts are unaffected.

Without the protection, code could write `acct.balance = -5000`, and nobody would be able to find where the bad value came from.

Name mangling in action:

```python
class Safe:
    def __init__(self):
        self.__code = 1234

s = Safe()
print(hasattr(s, "__code"))  # False
print(s._Safe__code)         # 1234
```

The attribute is not found under its written name, but exists under the mangled name. So it is not secret, only awkward to reach by accident. If a subclass also defines `__code`, the two do not collide, since each is mangled with its own class name.

Compare the three levels:

| Style | Meaning | Enforced? |
|---|---|---|
| `balance` | Public | No need |
| `_balance` | Internal, by convention | No |
| `__balance` | Mangled, to avoid clashes | Partly, by renaming |

A decorator can be written by you too:

```python
def shout(func):
    def wrapper():
        return func().upper()
    return wrapper

@shout
def greet():
    return "hello"

print(greet())  # HELLO
```

`@shout` above `greet` means `greet = shout(greet)`. The original function is wrapped, and its result changed. It is used for logging, timing, access checks and many other cross-cutting jobs.

## A common mistake {#mistake}
Believing that two underscores make data private and secure. They do not; anyone can read the mangled name. Treat privacy in Python as a courtesy and design the public interface carefully. Another is making getters and setters for everything, as other languages do, when a plain attribute would do until a rule is needed. `@property` lets you add the rule later without changing callers.

## Key points {#key-points}
- Encapsulation puts the data and its rules in one class, with a small public interface.
- `_name` means internal by convention. `__name` is renamed to avoid clashes.
- A decorator wraps a function. `@property` turns a method into a read-only attribute.
- Put validation in the one place that changes the data.
