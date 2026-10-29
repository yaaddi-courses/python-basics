# Scope, the LEGB rule and the main guard

**Covers:** Scope, LEGB rule, LEGB, GEBL, BLEG, LGEB, __name__, __main__

## Where a name can be seen {#overview}
When you write a name in a program, Python must decide which variable it means. The same name can exist in several places at once, and the rules about which one wins are the rules of scope. A second, related question concerns files: when a file is imported, should its code run, or only when the file is started directly?

## Scope {#scope}
Scope is the region of code where a particular variable name is visible and usable. A variable created inside a function is local to it, and is not reachable from outside. A variable created at the top of a file is global to that file. Using scope well keeps functions independent: they cannot change each other's variables by accident.

## LEGB rule {#legb-rule}
The LEGB rule is the order in which Python searches for a name when it is used: Local scope first, then Enclosing scope (a function around the current one), then Global scope (the top level of the file), and finally Built-in (names such as `len` and `print`). The search stops at the first place where the name is found.

## LEGB {#legb}
LEGB is the short form of that rule, taken from the first letters of the four scopes in order. Remember it as one word, always in this order.

## GEBL, BLEG and LGEB {#scrambles}
GEBL, BLEG and LGEB are scrambled versions of the same four letters. They are wrong. The correct order is LEGB. Any other arrangement would search in the wrong sequence, for example looking at the built-in names first.

## __name__ {#name}
`__name__` is a built-in variable that holds the name of the current module. When a file is imported by another file, its `__name__` is the file's own name. When it is run directly, `__name__` is the text `"__main__"`.

## __main__ {#main}
`__main__` is the name Python gives to the module that is run directly. It is used in the guard `if __name__ == "__main__":`. Code inside that guard runs when the file is started directly, and does not run when the file is imported.

## A worked example {#example}
Scope in a few lines:

```python
x = "global"

def show():
    x = "local"
    print(x)

show()   # local
print(x) # global
```

The function has its own `x`, found first by the local step of the search. The global `x` is untouched.

When a name is not found locally, the search moves outward:

```python
message = "hello"

def outer():
    suffix = "!"
    def inner():
        return message + suffix
    return inner()

print(outer())  # hello!
```

Inside `inner`, `message` is not local and not in the enclosing function, so it is found in the global scope. `suffix` is not local but is found in the enclosing function. Together: "hello" + "!".

A built-in is the last stop:

```python
print(len("abc"))  # 3
```

`len` is not defined anywhere in the file, so Python finds it in the built-in scope. But if you define your own `len`, it is found earlier, and hides the built-in. That is why naming a variable `list` or `sum` causes strange errors later.

Assigning to a global from inside a function needs the `global` keyword:

```python
count = 0

def add_one():
    global count
    count += 1

add_one()
add_one()
print(count)  # 2
```

Without the `global` line, Python would treat `count` as local, and fail. In larger programs it is better to pass values in and return results, instead of changing globals.

Now the guard. Imagine a file `tools.py`:

```text
def double(x):
    return x * 2

if __name__ == "__main__":
    print(double(21))
```

Run directly with `python tools.py`, it prints 42, because `__name__` is `"__main__"`. When another file does `import tools`, nothing is printed, because `__name__` is `"tools"`, and the guard is false. Without the guard, every import would print 42, which is unwanted.

Check the value yourself:

```python
print(__name__)  # __main__
```

Run as a script, this prints `__main__`.

Count the benefit of the guard. A module with 20 helper functions and one test run at the bottom: without a guard, each of the 5 files that import it would run the test at start-up, adding noise and time. With the guard, it runs only when you want it.

| Name | Value in `tools.py` run directly | Value when imported |
|---|---|---|
| `__name__` | `"__main__"` | `"tools"` |

Remember the order with a phrase: "Look Everywhere, Go Back". Or just LEGB.

## A common mistake {#mistake}
Using a global variable inside a function, and changing it without declaring `global`. The change either fails or makes a separate local variable. Another is naming a variable after a built-in. And putting test code at the bottom of a module without the main guard.

## Key points {#key-points}
- Names are searched in the order Local, Enclosing, Global, Built-in: LEGB.
- A local name hides a global or built-in name of the same spelling.
- `__name__` is `"__main__"` only in the file that was started directly.
- Put script code under `if __name__ == "__main__":`.
