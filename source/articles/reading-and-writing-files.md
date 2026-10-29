# Reading and writing files

**Covers:** File I/O, open(), read(), readlines(), close()

## Data that outlives the program {#overview}
Variables disappear when a program ends. To keep information, a program writes it to a file, and to use it later, it reads the file back. Python makes this short, but a few details decide whether it is safe, and this article walks you through them.

## File I/O {#file-io}
File I/O means reading data from a file on disk, or writing data to one. The letters stand for input and output. Reading brings the file's content into the program, and writing puts the program's data into the file.

## open() {#open}
Every file operation starts with `open()`. It opens a file and gives you a file object. The first argument is the file's path. The second is the mode: `"r"` for reading, which is the default, `"w"` for writing, which erases any existing content first, and `"a"` for appending, which adds to the end. For text, also give an encoding such as `encoding="utf-8"`, to avoid surprises with non-English letters.

## read() {#read}
`read()` gives you the whole content of the file as one string. For a very large file, that takes a lot of memory.

## readlines() {#readlines}
`readlines()` gives you the lines of the file as a list of strings. Each line keeps its newline character at the end.

## close() {#close}
`close()` closes the file, which releases its resources and saves any pending writes to disk. A file that stays open can lock it for other programs, and you may lose data if the program stops unexpectedly.

## The with statement {#with}
Remembering to close is easy to forget, so Python has a safer form. The `with` statement opens a file and closes it for you when the block ends, even if an error happens inside. It is the recommended way, because you cannot forget to close.

## A worked example {#example}
Let's write three lines and then read them back. The file is created in a temporary folder, so nothing is left behind.

```text
import os
import tempfile

folder = tempfile.mkdtemp()
path = os.path.join(folder, "notes.txt")

with open(path, "w", encoding="utf-8") as f:
    f.write("first\n")
    f.write("second\n")
    f.write("third\n")

with open(path, encoding="utf-8") as f:
    text = f.read()
print(repr(text))  # 'first\nsecond\nthird\n'

with open(path, encoding="utf-8") as f:
    lines = f.readlines()
print(lines)  # ['first\n', 'second\n', 'third\n']
print(len(lines))  # 3
```

`read()` gave one string of 19 characters (5 + 1, 6 + 1, 5 + 1). `readlines()` gave a list of 3 items, each with its newline. To remove the newlines, use `strip()`:

```python
lines = ["first\n", "second\n", "third\n"]
print([line.strip() for line in lines])  # ['first', 'second', 'third']
```

Looping directly over the file is the memory-friendly way to handle large files:

```text
import os
import tempfile

path = os.path.join(tempfile.mkdtemp(), "n.txt")
with open(path, "w", encoding="utf-8") as f:
    f.write("a\nb\n")

with open(path, encoding="utf-8") as f:
    for line in f:
        print(line.strip())
# a
# b
```

Only one line is held in memory at a time. A 2 GB log file can be read this way on a small computer, while `read()` would need 2 GB of memory.

Now appending against writing:

```text
import os
import tempfile

path = os.path.join(tempfile.mkdtemp(), "log.txt")
with open(path, "w", encoding="utf-8") as f:
    f.write("one\n")
with open(path, "a", encoding="utf-8") as f:
    f.write("two\n")
with open(path, encoding="utf-8") as f:
    print(f.read().count("\n"))  # 2
```

Mode `"a"` added a second line. Had the second `open` used `"w"`, the file would have lost "one" and held 1 line. This is a common way to destroy data.

For comparison, here is the manual close:

```text
import os
import tempfile

path = os.path.join(tempfile.mkdtemp(), "m.txt")
f = open(path, "w", encoding="utf-8")
f.write("x")
f.close()
print(f.closed)  # True
```

If an error happened between `open` and `close`, the file would stay open. With `with`, it is closed anyway.

| Mode | Reads | Writes | If the file exists |
|---|---|---|---|
| `"r"` | Yes | No | Opens it |
| `"w"` | No | Yes | Erases it |
| `"a"` | No | Yes | Adds to the end |

## A common mistake {#mistake}
A common slip is opening with `"w"` when you meant to add, which erases the old content.

A second slip is forgetting to close a file that you opened without `with`, so the last data never reaches the disk. A third is calling `read()` on a huge file.

## Key points {#key-points}
- Use `with open(...) as f:` so the file closes by itself.
- `"r"` reads, `"w"` erases and writes, `"a"` adds.
- `read()` gives one string, `readlines()` a list of lines, and a loop over the file reads line by line.
- Give an encoding for text files.
