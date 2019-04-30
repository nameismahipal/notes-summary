## Advanced Data types

- Lists
    - can be changed (mutable), = ["1","2"]
- Tuples
    - can not be changed (immutable), = ("1","2")
- Sets
    - stores only mutable items (mutable), not sorted, no duplicates, = {"1","2"}
- Dictionaries
    - stores data in key, value pairs (mutable) , = {1:"one",2:"two"}


###  `cheat sheet`

| type             	| `list`   | `Tuples` | `Sets` | `Dictionaries`|                                                                             	
|-------------	| --------	|--------	|--------	|--------	|
| use | for storing similar items | for storing a snapshot of related items when we don’t plan on modifying, adding, or removing data. | for storing immutable data types uniquely. Easy to compare the items in sets | for storing data in key, value pairs. Keys used must be immutable data types | 
| mutable?     | Yes   | No| Yes | Yes|
| items | ["1", "2"] | ("1", "2") | {"1", "2"} | {1:"one", 2:"two"} |
| empty | `[]` or `list()` | `()` or `tuple()` | `set()` | `{}` |
| order preserved? | Yes. Items can be accessed by index. | Yes. Items can be accessed by index. | No. Items can’t be accessed by index.  |Sort of. As of Python 3.6 a dict is sorted by insertion order. Items can’t be accessed by index, only by key. | 
|common methods| `len(my_list)`, `append(item)`, `insert(index, item)`, `pop()` | can't change | my_set.add(item), my_set.discard(item), my_set.update(other_set) | my_dict[key] (error if not presesnt),  my_dict.get(key) - no error if not present, my_dict.items(), my_dict.keys(), and my_dict.values() |
| search methods | my_list.index(item), item in my_list | my_tuple.index(item), item `in` my_tuple | item `in` my_set | key `in` my_dict |
| in-place sortable? | Yes. my_list.sort() will sort the list in-place. my_list.sort(reverse=True) will sort the list in-place in descending order. my_list.reverse() will reverse the items in my_list in-place. | No | No, because items aren’t ordered. | No. dicts don’t have an index, only keys.|
| search speed | slow (each item to be checked) | slow (each item to be checked) | v fast | fast |


# Lists


Empty List

can be created by calling `list()` or `[]`
```python
>>> list()
[]
>>> []
[]
>>> type(list())
<class 'list'>
>>> type([])
<class 'list'>
```

```python
>>> names = ["mahi","vicky","lucky","ramu"]
>>> names
['mahi', 'vicky', 'lucky', 'ramu']
>>> len(names)
4
>>> 
>>> names[3] = "chitti"
>>> names
['mahi', 'vicky', 'lucky', 'chitti']
```
To increase readability
```python
>>> names = [
... "mahi",
... "vicky",
... "lucky",
... "chitti"
... ]
```

#### Sorting

- `sorted(my_list)` function on your list to return a new list, sorted in increasing (ascending) order. 
- Or use `sorted(my_list, reverse=True)` to create a new list sorted backwards, in decreasing (or descending) order.

these operations will return a *new* list.

##### Sorting the list in-place

- `my_list.sort()` on your list to sort it in increasing (ascending) order, 
- or `my_list.sort(reverse=True)` on the list to sort it backwards, in decreasing (or descending) order. 

This operation will modify the underlying list, and doesn’t return a value.

##### Reverse the list in-place
`my_list.reverse()`

`dir(list)` will show all the functions for list.

### Adding, Removing, Changing, and Finding Items in `list`s cheat sheet

| action                                           	| method                                	| returns           	| possible errors                            	|
|--------------------------------------------------	|---------------------------------------	|-------------------	|--------------------------------------------	|
| check length                                     	| `len(my_list)`                        	| `int`             	|                                            	|
| **add:** to the end                              	| `my_list.append(item)`                	| -                 	|                                            	|
| **insert:** at position                          	| `my_list.insert(pos, item)`           	| -                 	|                                            	|
| **update:** at position                          	| `my_list[pos] = item`          	| -        -         	| `IndexError` if `pos` is >= `len(my_list)`                                          	|
| **extend:** add items from another list          	| `my_list.extend(other_list)`          	| -                 	|                                            	|
| is item in list?                                 	| `item in my_list`                     	| `True` or `False` 	|                                            	|
| **index** of item                                	| `my_list.index(item)`                 	| `int`             	| `ValueError` if `item` is not in `my_list` 	|
| **count** of item                                	| `my_list.count(item)`                 	| `int`             	|                                            	|
| **remove** an item                               	| `my_list.remove(item)`                	| -                 	| `ValueError` if `item` not in `my_list`    	|
| **remove** the last item, or an item at an index 	| `my_list.pop()` or `my_list.pop(pos)` 	| `item`            	| `IndexError` if `pos` >= `len(my_list)`    	|

# Tuples

- Light weight collections
- Tuples are immutable (once created, the items in it can’t be modified).
Why ? Are lists not enought ?

While **`lists`** are generally used to store collections of similar items together, **`tuples`**, by contrast, can be used to contain a snapshot of data, which cant be modified.

                                                      	
:information_source: If you’re creating a one-item tuple, you must include a trailing comma, like this: (1, )
```python
>>> names = ("vicky", 6, "lucky", 9)
>>> names[2]
'lucky'
>>> names[0] = "chitti"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> 
```

Tuples for something called unpacking.

```python
>>> names = ("vicky",6,"lucky",9)
>>> first_dog, first_dog_age, second_dog, second_dog_age = names
>>> first_dog
'vicky'
>>> second_dog
'lucky'
>>> first_dog_age
6
>>> 
```

```py
>>> def http_status_code():
...     return 200, "OK"
... 
>>> code, value = http_status_code()
>>> code
200
>>> value
'OK'
>>> 
```

# Sets

- stores only immutable types, 
- dont have order, NOT sorted.(When you print, it may be print items randomly), so cant access with index number like [0], nor can add/remove items with index but with item name can.
- no duplicates allowed
- very fast membership testing
- can be used to de-duplicate items in list
- powerful operations like `union`, `intersection`, `difference`    	

### Examples

Empty Dict and Empty Set
```
>>> my_empty_dict = {}
>>> type(my_empty_dict)
<class 'dict'>
>>> my_empty_set = set()
>>> type(my_empty_set)
<class 'set'>
```

Set with items. No duplicates.
```
>>> names = {"vicky","lucky", "chitti", "vicky"}
>>> len(names)
3
>>> names
{'lucky', 'chitti', 'vicky'} 
```

`hash()` function
- `sets` allow you to quickly check if an item is contained in them or not is with an algorithm called a hash.
- an algorithm is a way of representing an immutable data type with a unique numerical representation.
- The hash() function only works on immutable data types. 

#### sets can be used to de-duplicate the items in a list

- pass the list into the set constructor.
```
>>> colors = ["Red", "Yellow", "Red", "Green", "Green", "Green"]
>>> set(colors)
{'Red', 'Green', 'Yellow'}
```

#### `set`s don't have an order

Sets don't have an order, so when you print them, the items won't be displayed in the order they were entered in the list.

```
>>> my_set = {1, "a", 2, "b", "cat"}
>>> my_set
{1, 2, 'cat', 'a', 'b'}
```

It also means that you *can't* access items in the `set` by position in subscript `[]` notation.

```python
>>> my_set = {"Red", "Green", "Blue"}
>>> my_set[0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'set' object does not support indexing
```

> :information_source: If your set contains items of the same type, and you want to sort the items, 
> - you'll need to convert the `set` to a `list` first. Or, 
> - you can use the built-in `sorted(sequence)` method, which will do the conversion for you.

```python
>>> my_set = {"a", "b", "cat", "dog", "red"}
>>> my_set
{'b', 'red', 'a', 'cat', 'dog'}
>>> sorted(my_set)
['a', 'b', 'cat', 'dog', 'red']
```

#### adding to and removing from `set`s

Since a set has no order, we can't add or remove items to it by index. We need to call the operations with the item itself.

##### Add items to a set with `my_set.add(item)`.

```python
>>> colors = {"Red", "Green", "Blue"}
>>> colors.add("Orange")
>>> colors
{'Orange', 'Green', 'Blue', 'Red'}
```

##### Remove items with `my_set.discard(item)`

You can remove an item from a `set` if it's present with `my_set.discard(item)`. If the set doesn't contain the item, no error occurs.

```python
>>> colors = {"Red", "Green", "Blue"}
>>> colors.discard("Green")
>>> colors
{'Blue', 'Red'}
>>> colors.discard("Green")
>>> colors
{'Blue', 'Red'}
```

You can also remove items from a `set` with `my_set.remove(item)`, which will raise a `KeyError` if the item doesn't exist.


##### Update a set with another sequence using `my_set.update(sequence)`

You can update a `set` by passing in another sequence, meaning another `set`, `list`, or `tuple`.

```python
>>> colors = {"Red", "Green"}
>>> numbers = {1, 3, 5}
>>> colors.update(numbers)
>>> colors
{1, 3, 'Red', 5, 'Green'}
```

> :information_source: Be careful passing in a `str`ing to `my_set.update(sequence)`. That's because a `str`ing is *also* a sequence. It's a sequence of characters.

> Ex:
>```python
> >> numbers = {1, 3, 5}
> >> numbers.update("hello")
> >> numbers
> {1, 3, 'h', 5, 'o', 'e', 'l'}
> ```
>Your set will update with each character of the `str`ing, which was probably not your intended result.

### `set` operations

`sets` allow quick and easy operations to compare items between two sets.

#### `set` operations cheat sheet

 method operation    	| symbol operation 	| result                                                                        	|
|---------------------	|------------------	|-------------------------------------------------------------------------------	|
| `s.union(t)`        	| <code>s &#124; t</code> | creates a new set with all the items **from both `s` and `t`**             |
| `s.intersection(t)` 	| `s & t`          	| creates a new set containing *only* items that are **both in `s` and in `t`** 	|
| `s.difference(t)`    	| `s ^ t`          	| creates a new set with items **in `s` but not in `t`**                        	|

#### examples


We have two sets, `rainbow_colors`, which contain the colors of the rainbow, and `favorite_colors`, which contain my favorite colors.

```python
>>> rainbow_colors = {"Red", "Orange", "Yellow", "Green", "Blue", "Violet"}
>>> favorite_colors = {"Blue", "Pink", "Black"}
```

- Union

    let's combine the sets and create a new `set` that contains all of the items from `rainbow_colors` and `favorite_colors` 
    
    - you can use `my_set.union(other_set)` method, or symbol for union `|=`

    ```python
    >>> rainbow_colors | favorite_colors
    {'Orange', 'Red', 'Yellow', 'Green', 'Violet', 'Blue', 'Black', 'Pink'}
    ```

- intersection. 

    creates a new `set` with *only* the items common in both `set`s.

    ```python
    >>> rainbow_colors & favorite_colors
    {'Blue'}
    ```

- difference. 

    creates a new set with the items that are in in one, but not the other. 

    ```python
    >>> rainbow_colors ^ favorite_colors
    {'Orange', 'Red', 'Yellow', 'Green', 'Violet', 'Black', 'Pink'}
    ```

 - subset
 - a superset
 - `frozenset`
    - if you need the functionality of a `set` in an immutable package (meaning that the contents can't be changed after creation).

# Dictionaires
- stores data in key, value pairs
- Dictionaries themselves are mutable, but, dictionary keys can only be immutable types.    
- Looking for a key in a large dictionary is extremely fast. Unlike lists, we don’t have to check every item for a match.

### Examples

Empty dicts
- with {}
- dict() method
    ```py
    >>> my_dict = {}
    >>> type(my_dict)
    <class 'dict'>

    >>> my_dict = dict()
    >>> type(my_dict)
    <class 'dict'>
    ```

- Creating dicts with items

    
    ```py
    >>> nums = {1: "one", 2: "two", 3: "three"}
    >>> len(nums)
    3
    ```


    - to create dicts with items in them, pass in key, value pairs. 
    - A dict is declared with curly braces {}, followed by a key and a value, separated with a colon **:**
    -  Multiple key and value pairs are separated with commas **,**

    - keys can be int, strings, tuples

- Accessing

    - Because a dictionary isn’t ordered, we can’t access the items in it by position. 
    - Instead, to access the items in it, we use square-bracket my_dict[key] notation.
    ```py
    >>> nums = {1: "one", 2: "two", 3: "three"}
    >>> nums[1]
    'one'
    >>> nums[2]
    'two'
    ```    

    - We’ll get a **KeyError: key** if we try to access my_dict[key] with square bracket notation, but key isn’t in the dictionary. 
    - `nums.get(key)` method. Using this method, if the key isn’t present, no error is thrown, and no value.
    - to provide a `default value` if the key is missing, we also pass an optional argument like so: `nums.get(key, default_val)`

- Adding, Removing

    - To add a new key value pair to the dictionary, you’ll use square-bracket notation.

    - putting a key into a dictionary that’s already there, you’ll just end up replacing it.
    
    - `in` keyword - to check if a particular key is in a dictionary 
    
    ```py
    >>> nums = {1: "one", 2: "two", 3: "three"}
    >>> nums[8] = "eight"

    >>> nums
    {1: 'one', 2: 'two', 3: 'three', 8: 'eight'}

    >>> nums[8] = "oops, overwritten"
    >>> nums
    {1: 'one', 2: 'two', 3: 'three', 8: ', overwritten'}
    >>> 8 in nums
    True
    ```    
- Updating
    - you can update the items in a dictionary with the items from another dictionary.
    ```py
    >>> colors = {"r": "Red", "g": "Green"}
    >>> numbers = {1: "one", 2: "two"}
    >>> colors.update(numbers)
    >>> colors
    {'r': 'Red', 'g': 'Green', 1: 'one', 2: 'two'}    
    ```

- Complex Dictionaries

    - One incredibly useful scenario for dictionaries is storing the values in a list or other sequence. 
    ```py            
    >>> colors = {"Green": ["Spinach"]}
    >>> colors
    {'Green': ['Spinach']}

    >>> colors["Green"].append("Apples")

    >>> colors
    {'Green': ['Spinach', 'Apples']}
    ```

#### Other Useful Methods

 Ex: nums = {1: 'one', 2: 'two', 3: 'three', 8: 'eight'}

- my_dict.keys()
    -   gets all the keys in dictionary
        ```
        nums.keys()
        dict_keys([1, 2, 3, 8])
        ```
- my_dict.values()
    - gets all the values in dictionary
        ```
        nums.values()
        dict_values(['one', 'two', 'three', 'eight'])
        ```
- my_dict.items()
    - gets all the (key,value) pairs in dictionary
        ```
        nums.items()
        dict_items([(1, 'one'), (2, 'two'), (3, 'three'), (8, 'eight')])
        ```


# ------------------------------------------
my_list = ["h", "e", "l", "l", "o"]
```py
# Let's add to my_list:
my_list.append("!")
#Now let's see it again:
>> my_list
>> len(my_list)
6
# everything b/n index 4 and 6
>> my_list[4:6]
# everything after index 4
>>> my_list[4:]
# Or, we can ask for just the last two items without caring how big the list is. This means "give me everything starting from two before the end":
>>> my_list[-2:]
```