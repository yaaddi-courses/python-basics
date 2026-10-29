# File names, sorting options and type hints that only look real

**Covers:** dependencies.txt, packages.json, pipfile.txt, packages.txt, env.py, sort_by, Maybe, Nullable

## More plausible wrong answers {#overview}
The same trap exists outside function names. A project file may be named in many sensible-looking ways, and only one is the convention. An option of a function may have an obvious name that is not the real one. And a type hint can be borrowed from another language. Here are the lookalikes that appear in the course, and the real answers.

## dependencies.txt {#dependencies-txt}
`dependencies.txt` is not the name pip expects. The convention for a list of required packages is `requirements.txt`. Nothing stops you from calling a file `dependencies.txt`, but then `pip install -r` must be given its name, and other people will not know where to look.

## packages.json {#packages-json}
`packages.json` is a name from other tools. It is a JSON file, a different format, used for example by JavaScript projects. pip reads a plain list of packages, one per line, and not JSON.

## pipfile.txt {#pipfile-txt}
`pipfile.txt` mixes two things. Pipenv, a separate tool, uses a file called `Pipfile`, with no extension and its own format. That is not a pip requirements file either.

## packages.txt {#packages-txt}
`packages.txt` has the right format, a text file with one package per line, but the wrong name. The expected name is `requirements.txt`. Tools, editors and automated services look for that one.

## env.py {#env-py}
`env.py` is an ordinary Python file name. The command `python -m venv env` makes a folder called `env`. It does not make a file, and `env.py` would be code, not an environment.

## sort_by {#sort-by}
`sort_by` is not a parameter of `sorted()`. Other languages and libraries use that name. In Python, you pass `key=` with a function that gives each item its sorting value.

## Maybe {#maybe}
`Maybe` is a type name from languages such as Haskell, where it means "a value that may be missing". Python has no `Maybe` hint. The hint for a type or `None` is `Optional`.

## Nullable {#nullable}
`Nullable` comes from C# and similar languages. In Python, the same idea is written `Optional[int]`, or `int | None`.

## A worked example {#example}
Using the correct names:

```python
words = ["pear", "fig", "banana"]
print(sorted(words, key=len))  # ['fig', 'pear', 'banana']
try:
    sorted(words, sort_by=len)
except TypeError:
    print("sorted() has no sort_by")  # sorted() has no sort_by
```

The real `key=` option sorted the words by length. The made-up `sort_by` raised a `TypeError`, because `sorted()` got an argument it does not know.

Type hints:

```python
from typing import Optional
import typing

def count(items: Optional[list] = None) -> int:
    return 0 if items is None else len(items)

print(count())  # 0
print(count([1, 2]))  # 2
print(hasattr(typing, "Optional"))  # True
print(hasattr(typing, "Maybe"))  # False
print(hasattr(typing, "Nullable"))  # False
```

`Optional` is in the `typing` module and `Maybe` and `Nullable` are not. A hint with an unknown name fails at once when the module loads, so such a mistake does not survive for long.

Files. A new project uses the convention from the start:

```bash
python -m venv .venv
pip install requests
pip freeze > requirements.txt
pip install -r requirements.txt
```

The environment is a folder named `.venv`. The list of packages is `requirements.txt`. Anyone who clones the project runs the last line and gets the same set. With `packages.txt` or `dependencies.txt`, that last step would need a note that explains the name.

Count the cost of a surprising name in a team of 10 people: if each person loses 5 minutes looking for the file, that is 50 minutes, and it recurs for every new member. A convention costs nothing and removes that.

| Looks real | What is real |
|---|---|
| `dependencies.txt`, `packages.txt`, `pipfile.txt` | `requirements.txt` |
| `packages.json` | A JSON file for other tools |
| `env.py` | A folder made by `python -m venv env` |
| `sort_by=` | `key=` |
| `Maybe`, `Nullable` | `Optional` |

## A common mistake {#mistake}
Naming the requirements file after your own taste. It works until someone else, or an automated service, expects the standard name. Another is writing a hint from another language and expecting an editor to accept it.

## Key points {#key-points}
- The pip requirements file is `requirements.txt`.
- `sorted()` takes `key=`, not `sort_by`.
- The hint for "a type or None" is `Optional`, not `Maybe` or `Nullable`.
- Check names in the documentation or with `help()`.
