# Operators and boolean values

**Covers:** Modulo, Exponentiation, Boolean, True, False, XOR

## Calculating and deciding {#overview}
Operators are the symbols that do the work in an expression. Some produce numbers, such as plus and times. Others produce yes-or-no answers, which a program uses to make decisions. In this article you will meet two special arithmetic operators and then the yes-or-no values.

## Modulo {#modulo}
Modulo is the `%` operator. It divides two numbers and gives you the remainder, not the result of the division. `7 % 2` is 1, because 2 goes into 7 three times with 1 left over. It is the standard tool for questions such as "is this number even?" and "does this repeat every 7 days?".

## Exponentiation {#exponentiation}
Exponentiation is the `**` operator, which raises a number to a power. `2 ** 3` is 2 multiplied by itself 3 times, giving 8. Watch out for one trap: the caret `^` is not a power operator in Python.

## Boolean {#boolean}
A boolean is a value that is either `True` or `False`, Python's way of representing a yes or no answer. Comparisons such as `5 > 3` produce booleans, and so do logical operators.

## True {#true}
`True` is the boolean value meaning yes. Its type is `bool`, and you write it with a capital T.

## False {#false}
`False` is the boolean value meaning no. Its type is `bool`, and you write it with a capital F.

## XOR {#xor}
XOR, exclusive or, says yes when exactly one of two conditions holds, and no when both or neither does. On numbers it is written `^`, and it works on the bits of the numbers. For booleans, `a != b` gives the same answer.

## A worked example {#example}
Let's begin with arithmetic:

```python
print(7 % 2)    # 1
print(10 % 5)   # 0
print(2 ** 3)   # 8
print(9 ** 0.5) # 3.0
print(7 // 2)   # 3
```

`10 % 5` is 0 because 5 goes into 10 exactly. A power of 0.5 is a square root. Floor division `//` gives the whole-number part of the division, 3, while `%` gives the remainder, 1. Together they rebuild the number: 3 × 2 + 1 = 7.

Here is modulo finding even and odd numbers:

```python
for n in (4, 7, 10):
    print(n, n % 2 == 0)
# 4 True
# 7 False
# 10 True
```

The comparison `n % 2 == 0` is a boolean. An even number leaves remainder 0 when divided by 2.

Another use is wrap-around. Think of a clock with 12 hours. If it is 10 o'clock now, what time is it in 5 hours?

```python
print((10 + 5) % 12)  # 3
```

15 divided by 12 leaves 3. The same idea gives days of the week: the day number `% 7`.

Booleans combine with `and`, `or` and `not`:

```python
age = 20
has_ticket = True
print(age >= 18 and has_ticket)  # True
print(age < 18 or not has_ticket)  # False
```

For XOR, imagine two lights and an alarm that should sound if exactly one is on:

```python
a = True
b = False
print(a != b)        # True
print(True != True)  # False
```

The alarm sounds when the two differ. On numbers, `^` works on bits:

```python
print(5 ^ 3)  # 6
```

In binary, 5 is 101 and 3 is 011. XOR compares each position and gives 1 where they differ, which produces 110, and that is 6.

| a | b | a and b | a or b | a XOR b |
|---|---|---|---|---|
| False | False | False | False | False |
| False | True | False | True | True |
| True | False | False | True | True |
| True | True | True | True | False |

Exponentiation also shows compound growth. Money at 5% a year for 10 years grows by a factor of 1.05 ** 10, which is about 1.63.

```python
print(round(1.05 ** 10, 2))  # 1.63
```

## A common mistake {#mistake}
A common slip is writing `2 ^ 3` for a power. It works without an error but gives 1, because `^` is XOR on bits. Use `**`.

A second slip is writing `true` in lowercase, which Python reads as an undefined name.

A third is surprise at `%` with a negative number. Python gives the result the sign of the divisor, so `-1 % 5` is 4.

## Key points {#key-points}
- `%` gives a remainder, and `**` raises to a power.
- `True` and `False` are the two boolean values, spelled with capitals.
- Comparisons produce booleans.
- XOR says yes when exactly one side does.
