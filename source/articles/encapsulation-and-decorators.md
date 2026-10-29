# Encapsulation, name mangling and decorators

**Covers:** Encapsulation, Name mangling, Decorator, _name

## Protecting the inside of an object {#overview}
Think about a bank account. It should never accept a negative deposit. But if a class exposes all of its data, any code can change that data in any way, including ways that break the account's rules. Keeping the data behind a controlled door fixes this. Python takes a relaxed approach to that door: it relies on conventions and gives you a few tools to help. Let's look at them.

## Encapsulation {#encapsulation}
Encapsulation means bundling an object's data together with the methods that work on it, inside a class, and hiding the internal details from outside code that does not need them. Outside code uses the public methods and leaves the inner variables alone. The payoff is that you can change the inside later without breaking the people who use the class.

## _name {#underscore-name}
How do you tell other programmers that something is internal? With a single underscore at the start of a name, such as `_balance`. Python does not stop anyone from using it. It is a signal: "this is internal, do not rely on it". Tools and code reviewers respect that signal.

## Name mangling {#name-mangling}
Two underscores do something more. When an attribute name starts with two underscores, like `__balance`, Python renames it internally to `_ClassName__balance`. That is name mangling. It makes accidental use from outside, or from a subclass, harder, though not impossible. Its purpose is to avoid name clashes in subclasses, so do not think of it as a security feature.

## Decorator {#decorator}
A decorator is a function that you write with an `@` symbol right above another function or method. It wraps that function and changes how it behaves, without editing its code. The best-known example is `@property`, which lets a method be read like an attribute, so `account.balance` runs code behind the scenes.

## A worked example {#example}
Let's build a bank account with a protected balance:

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

Here is what each part does:

1. `_balance` holds the real value, and its underscore marks it as internal.
2. `@property` makes `balance` readable like an attribute (`acct.balance`, no parentheses). No setter is defined, so assigning to it fails.
3. `deposit` is the only door for changing the money, and it checks the rule. A negative deposit raises an error.

Notice that the rule lives in one place. If the bank later adds a limit of 10,000 per deposit, only `deposit` changes, and the 100 places that use accounts are unaffected.

Without this protection, any code could write `acct.balance = -5000`, and nobody could find where the bad value came from.

Now name mangling in action:

```python
class Safe:
    def __init__(self):
        self.__code = 1234

s = Safe()
print(hasattr(s, "__code"))  # False
print(s._Safe__code)         # 1234
```

The attribute is not found under its written name, but it exists under the mangled name. So it is not secret, only awkward to reach by accident. If a subclass also defines `__code`, the two do not collide, because each is mangled with its own class name.

Compare the three levels:

| Style | Meaning | Enforced? |
|---|---|---|
| `balance` | Public | No need |
| `_balance` | Internal, by convention | No |
| `__balance` | Mangled, to avoid clashes | Partly, by renaming |

You can write a decorator yourself too:

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

`@shout` above `greet` means `greet = shout(greet)`. The original function is wrapped and its result changed. Programmers use this for logging, timing, access checks and many other jobs that cut across a program.

## A common mistake {#mistake}
A common slip is believing that two underscores make data private and secure. They do not, because anyone can read the mangled name. Treat privacy in Python as a courtesy, and design the public interface carefully.

A second slip is writing getters and setters for everything, as other languages do, when a plain attribute would do until a rule is needed. `@property` lets you add the rule later without changing callers.

## Key points {#key-points}
- Encapsulation puts the data and its rules in one class, with a small public interface.
- `_name` means internal by convention. `__name` is renamed to avoid clashes.
- A decorator wraps a function. `@property` turns a method into a read-only attribute.
- Put validation in the one place that changes the data.
