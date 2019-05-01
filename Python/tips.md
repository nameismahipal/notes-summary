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

