### String Formatting

f-strings

f-strings allow you to simply and easily reference variables in your code.
Ex:
```
name = "Nina"
greeting = f"Hello, {name}"
```

- \n for new line
- \t for tab

```py
# Use \n to add a new line, before, in the middle of, or after a string.
print("\nExtra New Line Before")
print("Extra New Line After\n")

# Use \t to add a tab.
print("\t Here's some tabbed output.")
```

### Pretty Printing with pprint

- extra formatting, like having each element of a long list on a new line, you can use the included pprint module 
```py
>>> long_list = list(range(5))
>>> print(long_list)
[0, 1, 2, 3, 4]
>>> from pprint import pprint
>>> pprint(long_list)
[0,
 1,
 2,
 3,
 4]
```

### Parsing

Converting between types in Python is one of the most powerful language features.

You can quickly convert between strings, numbers, and various data-types to supercharge quickly solving problems.
You can even use powerful data structures like sets to your advantage.

## Converting Between Numbers and Strings

Converting between numbers and strings is easy with `str()` and `int()`:

```python
>>> my_string = str(100)
>>> my_string
'100'
>>> type(my_string)
<class 'str'>
>>> my_int = int(my_string)
>>> my_int
100
>>> type(my_int)
<class 'int'>
```

`float()` to convert strings into floating point numbers:

```python
>>> float("3.1415")
3.1415
```

```python
>>> int(3.1415)
3
```

## Converting Between Lists and Strings

```python
>>> my_list = list("hello")
>>> my_list
['h', 'e', 'l', 'l', 'o']
>>> str(my_list)
"['h', 'e', 'l', 'l', 'o']"
```
running any object through `str()` will usually return a literal string of that object.

string's built-in `join()` method - to *join* the elements of the list (into a string). 

```python
>>> ''.join(my_list)
'hello'

>>> ','.join(my_list)
'h,e,l,l,o'
>>> '-'.join(my_list)
'h-e-l-l-o'
```

string's `split()` method - Another common way of converting a list to a string.

```python
>>> my_string = "the,quick,brown,fox"
>>> my_string.split(",")
['the', 'quick', 'brown', 'fox']
```

