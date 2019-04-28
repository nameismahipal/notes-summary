## Advanced Data types

# Lists

### `list` cheat sheet

| type             	| `list`                                                                                	|
|------------------	|---------------------------------------------------------------------------------------	|
| use              	| Used for storing similar items, and in cases where items need to be added or removed. 	|
| creation         	| `[]` or `list()` for empty list, or `[1, 2, 3]` for a list with items.                            	|
| search methods   	| `my_list.index(item)` or `item in my_list`                                                                           	|
| search speed     	| Searching in an item in a large list is slow. Each item must be checked.                               	|
| common methods   	| `len(my_list)`, `append(item)` to add, `insert(index, item)` to insert in the middle, `pop()` to remove.         	|
| order preserved? 	| Yes. Items can be accessed by index.                                                  	|
| mutable?         	| Yes                                                                                   	|
| in-place sortable?        	| Yes. `my_list.sort()` will sort the list in-place. `my_list.sort(reverse=True)` will sort the list in-place in *descending* order. `my_list.reverse()` will *reverse the items* in `my_list` in-place.

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

### `tuple` cheat sheet

| type               	| `tuple`                                                                                                 	|
|--------------------	|---------------------------------------------------------------------------------------------------------	|
| use                	| Used for storing a snapshot of related items when we don't plan on modifying, adding, or removing data. 	|
| creation           	| `()` or `tuple()` for empty tuple. `(1, )` for one item, or `(1, 2, 3)` for a tuple with items.         	|
| search methods     	| `my_tuple.index(item)` or `item in my_tuple`                                                            	|
| search speed       	| Searching for an item in a large tuple is slow. Each item must be checked.                              	|
| common methods     	| Can't add or remove from tuples.                                                                        	|
| order preserved?   	| Yes. Items can be accessed by index.                                                                    	|
| mutable?           	| **No**                                                                                                  	|
| in-place sortable? 	| **No**                                                                                                  	|

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