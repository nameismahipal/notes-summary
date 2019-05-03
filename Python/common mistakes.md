## Common Mistakes

1. Mismatched String quotes

    - Ex: “Hello‘

2. Concatenate Number with String

    - 3 + "apples"

    instead str(3) + "apples" will work.
    
3. Looping over Dictionary

What if you try to loop over key, value pairs, and forget to use my_dict.items()?
```
>>> for color, hex_value in hex_colors:
...     print(f"For color {color}, the hex value is: {hex_value}")
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: too many values to unpack (expected 2)
```
You’ll see ValueError: too many values to unpack (expected 2) if you forget to call my_dict.items(), and try to loop over what you’d expect to be key, value pairs.   

4. forgetting space while building query

This throws error
```py
def create_query(languages, min_stars=50000):
    query = f"stars:>{min_stars}"

    for language in languages:
        query += f"language:{language}"

    return query 
``` 
This Succeeds
```py
def create_query(languages, min_stars=50000):
    query = f"stars:>{min_stars} "

    for language in languages:
        query += f"language:{language}"

    return query 
```    