# Scope, the LEGB rule and the main guard

**Covers:** Scope, LEGB rule, LEGB, GEBL, BLEG, LGEB, __name__, __main__

## Where a name can be seen {#overview}
When you write a name in a program, Python must decide which variable you mean. The same name can exist in several places at once, and scope is the set of rules that says which one wins. A second, related question is about files. When another file imports yours, should your code do its work, or only when you start the file directly? We will answer both.

## Scope {#scope}
Scope is the region of code where a variable name is visible and usable. A variable created inside a function is local to it, and the code outside cannot reach it. A variable created at the top of a file is global to that file. Using scope well keeps your functions independent, because they cannot change each other's variables by accident.

## LEGB rule {#legb-rule}
When the same name appears in several places, Python searches in a fixed order. The LEGB rule says: Local scope first, then Enclosing scope (a function around the current one), then Global scope (the top level of the file), and finally Built-in (names such as `len` and `print`). The search stops at the first place where it finds the name.

## LEGB {#legb}
LEGB is the short form of that rule, made from the first letters of the four scopes in order. Remember it as one word, always in this order.

## GEBL, BLEG and LGEB {#scrambles}
You may see GEBL, BLEG and LGEB in a quiz. They are scrambled versions of the same four letters, and they are wrong. The correct order is LEGB. Any other arrangement would search in the wrong sequence, for example by looking at the built-in names first.

## __name__ {#name}
`__name__` is a built-in variable that holds the name of the current module. When another file imports a file, the imported file's `__name__` is its own name. When you start a file directly, `__name__` is the text `"__main__"`.

## __main__ {#main}
`__main__` is the name Python gives to the module you start directly. You see it in the guard `if __name__ == "__main__":`. Code inside that guard happens when the file is started directly and is skipped when the file is imported.

## A worked example {#example}
Let's see scope in a few lines:

```python
x = "global"

def show():
    x = "local"
    print(x)

show()   # local
print(x) # global
```

The function has its own `x`, which the local step of the search finds first. The global `x` is untouched.

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

Inside `inner`, `message` is not local and not in the enclosing function, so Python finds it in the global scope. `suffix` is not local either, but it is found in the enclosing function. Together they make "hello" + "!".

A built-in is the last stop:

```python
print(len("abc"))  # 3
```

`len` is not defined anywhere in the file, so Python finds it in the built-in scope. But if you define your own `len`, Python finds yours earlier, and it hides the built-in. That is why naming a variable `list` or `sum` causes strange errors later.

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

Without the `global` line, Python would treat `count` as local and fail. In larger programs, it is better to pass values in and return results than to change globals.

Now the guard. Imagine a file `tools.py`:

```text
def double(x):
    return x * 2

if __name__ == "__main__":
    print(double(21))
```

When you start it directly with `python tools.py`, it prints 42, because `__name__` is `"__main__"`. When another file does `import tools`, nothing is printed, because `__name__` is `"tools"`, and the guard fails. Without the guard, every import would print 42, which nobody wants.

You can check the value yourself:

```python
print(__name__)  # __main__
```

Started as a script, this prints `__main__`.

Let's count the benefit of the guard. Take a module with 20 helper functions and a test at the bottom. Without a guard, each of the 5 files that import it would start the test at start-up, adding noise and time. With the guard, the test happens only when you want it.

| Name | Value in `tools.py` started directly | Value when imported |
|---|---|---|
| `__name__` | `"__main__"` | `"tools"` |

A phrase helps you remember the order: "Look Everywhere, Go Back". Or say LEGB.

## A common mistake {#mistake}
A common slip is using a global variable inside a function and changing it without declaring `global`. The change either fails or makes a separate local variable.

A second slip is naming a variable after a built-in. A third is putting test code at the bottom of a module without the main guard.

## Key points {#key-points}
- Names are searched in the order Local, Enclosing, Global, Built-in: LEGB.
- A local name hides a global or built-in name of the same spelling.
- `__name__` is `"__main__"` only in the file that was started directly.
- Put script code under `if __name__ == "__main__":`.
