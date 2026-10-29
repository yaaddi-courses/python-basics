# Working with strings

**Covers:** String, F-string, Indexing, Slicing, lower(), strip(), split(), toLowerCase()

## Text in Python {#overview}
Almost every program handles text: names, messages, file contents, web addresses. Python represents text as a string and gives you many ready-made operations for it. In this article you will learn how to build strings, take them apart and clean them.

## String {#string}
A string is a sequence of characters that represents text, written between quotes, like `'hello'` or `"hello"`. Both kinds of quotes work, so you can write `"it's"` without trouble. A string is immutable, which means that once it exists, it cannot be changed. Every operation on a string gives you a new string and leaves the original as it was.

## F-string {#f-string}
How do you put a variable inside a sentence? With an f-string, a string that has an `f` right before the opening quote. You write the variable in curly braces, like `f'Hello {name}'`. You can put any expression inside the braces, not only names. A format after a colon controls the look: `{price:.2f}` shows two decimal places.

## Indexing {#indexing}
Indexing means taking one character out of a string by its position, counting from 0. The first character is `s[0]`, and the last is `s[-1]`, since negative numbers count from the end. Asking for a position that does not exist is an error.

## Slicing {#slicing}
Slicing pulls out a range of characters with `s[start:end]`. You get everything from `start` up to, but not including, `end`. If you leave out a number, it means "from the beginning" or "to the end", and a third number chooses the step.

## lower() {#lower}
`lower()` gives you a copy of the string with every letter in lowercase. It is the usual way to compare text without caring about capitals.

## strip() {#strip}
`strip()` gives you a copy with whitespace removed from both ends: spaces, tabs and newlines. It is the first thing to do with text that a user typed or that came from a file.

## split() {#split}
`split()` breaks a string into a list of parts at a separator. With no argument, it splits at any run of whitespace. With an argument, such as `split(",")`, it splits at that text.

## toLowerCase() {#tolowercase}
`toLowerCase()` is the name of the same operation as `lower()` in JavaScript. Python has no such name, and using it gives an error. If you know JavaScript, remember that Python's string operations are written in lowercase, and `lower()` is the one you want.

## A worked example {#example}
Imagine a line of text arrives from a form. Let's clean it step by step, take it apart and build a message:

```python
raw = "  Maria Garcia, maria@example.com  "
clean = raw.strip()
print(clean)  # Maria Garcia, maria@example.com
parts = clean.split(", ")
print(parts)  # ['Maria Garcia', 'maria@example.com']
name = parts[0]
email = parts[1].lower()
print(email)  # maria@example.com
print(name[0])   # M
print(name[-1])  # a
print(name[:3])  # Mar
print(name[6:])  # Garcia
message = f"Hello {name.split()[0]}, we sent a note to {email}."
print(message)  # Hello Maria, we sent a note to maria@example.com.
```

First `strip()` removes the 2 spaces at each end. Then `split(", ")` makes a list of 2 parts. The email becomes lowercase for later comparison. `name[:3]` takes characters 0, 1 and 2. `name[6:]` takes everything from position 6: the space is at 5, and G is at 6. The f-string builds the final message.

Now try to change a string in place:

```python
word = "dog"
changed = word.upper()
print(word)     # dog
print(changed)  # DOG
```

The original is still `dog`, because strings cannot be changed. The operation made a new string.

Let's count the positions. `"Maria Garcia"` has 12 characters, numbered 0 to 11, so `name[11]` is `a`, and `name[12]` would raise an error. The same last character is `name[-1]`, a safer way to say "the last one".

| Task | Tool |
|---|---|
| Remove surrounding spaces | `strip()` |
| Compare ignoring capitals | `lower()` on both sides |
| Cut at commas | `split(",")` |
| Take the first three characters | `s[:3]` |
| Insert values into text | f-string |

## A common mistake {#mistake}
A common slip is writing `name.lower()` and expecting `name` to change. Strings are immutable, so you must keep the result: `name = name.lower()`.

A second slip is `toLowerCase()`, from JavaScript habit. A third is forgetting that a slice excludes its end, so `s[0:3]` has 3 characters, not 4.

## Key points {#key-points}
- A string is immutable text. Operations return new strings.
- Indexing starts at 0. Negative numbers count from the end.
- A slice `s[a:b]` includes `a` and excludes `b`.
- `strip()`, `lower()` and `split()` clean and cut text. An f-string builds it.
