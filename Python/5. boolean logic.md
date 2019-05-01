### cheat sheet

| type                                        	| truthiness                                                                     	|   
|---------------------------------------------	|--------------------------------------------------------------------------------	|
| `int`                                       	| `0` is `False`, all other numbers are `True` (**including negative**)              	| 
| containers - `list`, `tuple`, `set`, `dict` 	| empty container evaluates to `False`, container with items evaluates to `True) 	| 
| `None`                                      	| `False`                                                                        	| 

### Numbers

In Python, the integer `0` is always `False`, while every other number, *including negative numbers*, are `True`. 

```
>>> bool(0)
False
>>> bool(1)
True
>>> bool(-1)
True
```


### Sequences

Empty sequences in Python always evaluate to `False`, **including empty `str`ings.**

```py
>>> bool("")    # String
False
>>> bool([])    # Empty List
False
>>> bool(set()) # Empty Set
False
>>> bool({})    # Empty Dictionary
False
>>> bool(())    # Empty Tuple
False
```

Sequences with at least one value will evaluate to `True`.

```python
>>> bool("Hello")   # String
True
>>> bool([1])       # List
True
>>> bool({1})       # Set
True
>>> bool({1: 1})    # Dictionary
True
>>> bool((1,))      # Tuple
True
```

### `None`

The `None` type in Python represents nothing. No returned value. 

```python
>>> bool(None)
False
```

# Comparisions

In Python, comparing numbers is pretty straight forward.

```python
>>> 1 < 10  # 1 is less than 10? True
True
>>> 20 <= 20  # 20 is less than or equal to 20? True
True
>>> 10 > 1  # 10 is greater than 1? True
True
>>> -1 > 1  # -1 is greater than 1? False
False
>>> 30 >= 30  # 30 is greater than or equal to 30? True
True
```

Strings are compared by the ASCII value of the character (lexicographically), besides that capital letters come before lower case ones.

Each character in the two strings is checked one by one, until a character is found that is of a different value. That determines the order. 

```python
>>> "T" < "t"  # Upper case letters are "lower" valued.
True
>>> "a" < "b"
True
>>> "bat" < "cat"
True
```

Even though a and b are two different lists, their contents are still the same. So compared two lists containing the same values with == will return True.
```
>>> a = [1, 2, 3]
>>> b = [1, 2, 3]
>>> a == b
True
>>> a != b
False
```

|Operator|Means|
|---|---|
|`is`| *is* the same object in memory? |
|`is not`| *is not* the same object in memory? |

The `is` keywords tests if the two compared objects are stored in the same memory location.
- remember not to use `is` when what you actually want to check for is equality.

```python
>>> a = [1, 2, 3]
>>> b = [1, 2, 3]

>>> a == b  # Testing for equality. a and b contain the same values
True
>>> a is b  # Testing for identity. a and b are NOT the same object.
False
```

```py
>>> a = True
>>> a is True
True

>>> b = False
>>> b is False
True
>>> b is not True   # Opposite of is b True. aka is b False?
True

>>> c = None
>>> c is None
True
>>> c is not None
False
```

# AND, OR, NOT

- AND 
```py
>>> a = True    # a is True
>>> b = True
>>> a and b     # True is returned. (value of b)
True

>>> a = False   # a is False
>>> b = True
>>> a and b     # False is returned. (value of a)
False

>>> a = False   # a is False
>>> b = False
>>> a and b     # False is returned. (value of a)
False
```

- OR

```
>>> a = True    # a is true
>>> b = True
>>> a or b      # True is returned (value of a)
True

>>> a = False   # a is false
>>> b = True
>>> a or b      # True is returned (value of b)
True

>>> 0 or 1      # 0 is false. Return 1.
1
```

- NOT

```
>>> a = True
>>> not a  # not returns the opposite. True -> False
False

>>> a = False
>>> not a  # not returns the opposite. False -> True
True
```