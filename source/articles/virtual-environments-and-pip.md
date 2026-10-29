# Virtual environments, pip and requirements files

**Covers:** Virtual environment, pip, requirements.txt, -e, -r, setup.py

## Keeping projects apart {#overview}
One computer holds many Python projects, and they need different packages, sometimes different versions of the same package. If every project installs into one shared place, they eventually clash. A virtual environment gives each project its own space, and the tools in this article let you fill that space and recreate it on another machine.

## Virtual environment {#virtual-environment}
A virtual environment is an isolated Python setup with its own separately installed packages, kept apart from your system-wide Python and from other projects' environments. You create one per project with `python -m venv .venv`, and you activate it before working. Packages installed while it is active go into it only. If you delete the folder, everything goes with it, and no trace stays in the system.

## pip {#pip}
`pip` is Python's standard command-line tool for installing packages from the Python Package Index, for example `pip install requests`. It also lists, upgrades and removes packages. Inside an active environment, it works on that environment alone.

## requirements.txt {#requirements-txt}
How does a colleague get the same packages as you? By reading a `requirements.txt`. By convention, this plain text file lists a project's dependencies, one per line, so anyone can recreate the same set of packages. Lines may pin exact versions, such as `requests==2.31.0`, or give a range.

## -r {#r}
The flag `-r` belongs to `pip install` and installs every package listed in a requirements file: `pip install -r requirements.txt`. It is the standard first step after you get a copy of a project.

## -e {#e}
The flag `-e` belongs to `pip install` too, and it means editable mode. `pip install -e .` installs the project in the current folder so that changes to its code take effect at once, without reinstalling. It is meant for development.

## setup.py {#setup-py}
`setup.py` is an older file, kept at the top of a project, that tells the installer the package's name, version and dependencies. Newer projects use a `pyproject.toml` file for the same purpose, but you will still meet `setup.py` in older code.

## A worked example {#example}
Let's start a new project cleanly. These commands are for a Unix-like shell:

```bash
python -m venv .venv
source .venv/bin/activate
pip install requests==2.31.0
pip freeze > requirements.txt
```

1. The first line creates the folder `.venv` with a private copy of Python.
2. The second activates it. The prompt usually shows `(.venv)`. On Windows, the activation file is `.venv\Scripts\activate`.
3. The third installs one package, pinned to a version.
4. The fourth writes the exact list of installed packages, including indirect ones, to `requirements.txt`.

The file now holds lines like:

```text
certifi==2023.11.17
charset-normalizer==3.3.2
idna==3.6
requests==2.31.0
urllib3==2.1.0
```

One direct dependency produced five lines, because `requests` needs four others.

A colleague gets the project and prepares it:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

They get the same versions, so the program behaves the same on both machines.

Now picture the clash that environments avoid. Project A needs `library 1.0` and project B needs `library 2.0`. In a single shared space, only one can be installed, and one project breaks. With two environments, each project has its own. If you have 6 projects, that is 6 folders, each of a few tens of megabytes.

Check where you are:

```bash
which python
pip list
```

The first command should print a path inside `.venv`. If it prints the system Python, the environment is not active, and `pip install` would change the wrong place.

For your own library under development, use this:

```bash
pip install -e .
```

Now an edit to the code is visible at once to any script that imports it. Without `-e`, you would reinstall after every edit. For this, the project needs a `pyproject.toml` or a `setup.py` in its folder, which describes the package.

| Command | Does |
|---|---|
| `python -m venv .venv` | Creates an environment |
| `pip install name` | Installs a package |
| `pip install -r requirements.txt` | Installs the listed ones |
| `pip install -e .` | Editable install of this project |
| `pip freeze` | Lists exact versions |

Keep `.venv` out of version control by listing it in `.gitignore`, and commit `requirements.txt`. You can always rebuild the environment from the file.

## A common mistake {#mistake}
A common slip is installing packages without an active environment, which pollutes the system Python. Check the prompt before every `pip install`.

A second slip is not pinning versions, so a new release breaks the project months later. A third is committing the `.venv` folder itself.

## Key points {#key-points}
- Give every project its own virtual environment.
- `pip` installs packages. `requirements.txt` records them, and `-r` installs from it.
- `-e` makes an editable install for development. `setup.py` is the older package description.
- Commit the requirements file, not the environment.
