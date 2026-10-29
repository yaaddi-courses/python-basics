# Reading and writing files

**Covers:** File I/O, open(), read(), readlines(), close()

## Data that outlives the program {#overview}
Variables disappear when a program ends. To keep information, a program writes it to a file, and to use it later, reads it back. Python makes this short, but a few details decide whether it is safe.

## File I/O {#file-io}
File I/O means reading data from a file on disk, or writing data to one. The letters stand for input and output. Reading brings the file's content into the program, and writing puts the program's data into the file.

## open() {#open}
`open()` opens a file and returns a file object. The first argument is the file's path. The second is the mode: `"r"` for reading, which is the default, `"w"` for writing, which erases any existing content first, and `"a"` for appending, which adds to the end. Add an encoding for text, such as `encoding="utf-8"`, to avoid surprises with non-English letters.

## read() {#read}
`read()` returns the whole content of the file as one string. For a very large file, that is a lot of memory.

## readlines() {#readlines}
`readlines()` returns the lines of the file as a list of strings. Each line keeps its newline character at the end.

## close() {#close}
`close()` closes the file so that its resources are released and any pending writes are saved to disk. A file that stays open can lock the file for other programs, and may lose data if the program stops unexpectedly.

## The with statement {#with}
The `with` statement opens a file and closes it for you when the block ends, even if an error happens inside. It is the recommended way, since you cannot forget to close.

## A worked example {#example}
Write three lines, then read them back. The file is created in a temporary folder so that nothing is left behind.

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

Looping directly over the file is the memory-friendly way for large files:

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

Appending versus writing:

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

Mode `"a"` added a second line. Had the second `open` used `"w"`, the file would have lost "one" and contained 1 line. This is a common way to destroy data.

The manual close, for comparison:

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
Opening with `"w"` when you meant to add, which erases the old content. Another is forgetting to close a file written without `with`, so the last data never reaches the disk. And using `read()` on a huge file.

## Key points {#key-points}
- Use `with open(...) as f:` so the file closes by itself.
- `"r"` reads, `"w"` erases and writes, `"a"` adds.
- `read()` gives one string, `readlines()` a list of lines, and a loop over the file reads line by line.
- Give an encoding for text files.
