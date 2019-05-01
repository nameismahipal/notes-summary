# Functions

### Anatomy of a function

This is the recipe for defining a Python function:

- ```def```  the def keyword, telling Python we’re about to start a function definition 
- a name for the function
- ```(``` opening parenthesis
- (optional) the names of one or more arguments, separated with ```,```
- (optional) the names and values of one or more default arguments, separated with ```(,)``` 
- ```)``` closing parenthesis
- ```:``` a colon

A function in Python is defined with the **def** keyword, followed by the **function names**, **zero** or **more argument** names contained in parenthesis **()**, and a colon **:** to indicate the start of the function.

Then an optional return statement.

Ex:
```
def hello_world():
    print("Hello, World!")

def add_numbers(x, y):
    return x + y    
```

#### Function Content

Indentation is necessary.
```
def foo():
    x = 5
    return x
```

If a function doesn’t have a return statement, it implicitly returns ```None```.

#### Indentation
One of the most important aspects of functions is ```indentation```.

Python doesn’t use curly braces to figure out what’s inside a function like other languages like JavaScript or Java.

Python knows what code is related to a function by how it’s indented. Use Tab to add indentation.

#### Arguments

##### Positional arguments

arguments must be given in the order they are declared.

For example:
```
def say_greeting(name, greeting):
...     print(f"{greeting}, {name}.")
...
>>> say_greeting("Mahi", "Hello!")
Hello!, Mahi
```
#### Keyword arguments with default value

Arguments that have default values are called keyword arguments, and they can be overridden when needed.

```
def say_greeting(name, greeting="Hey!"):
...     print(f"{greeting}, {name}.")
...
>>> say_greeting("Mahi", "Hello!")
Hello!, Mahi

>>> say_greeting("Mahi")
Hey!, Mahi
```
:warning: Never use mutable types, like lists as a default argument. **Why?** Because it won’t work like you’d expect it to. **Why?** In Python, default arguments are evaluated only once – when the function is defined, Not each time the function is called. 

:information_source: Use Marker Instead

```
# Don't do this!
>>> def add_five_to_list(my_list=[]):
...     my_list.append(5)
...     return my_list

```

#### Function Scope

once the indentation is broken, that will be the end of that function scope.

scoping in Python happens with whitespace.

variable defined in a function, will not be accessable outside of the function.

a function can access a variable that is defined outside a function, but Note, that if we try to change the value of a variable defined outside of our function, we’ll see the changes in the function, but not outside of it.

You can’t change variables defined outside of the function inside of the function. 

If you try to, your changes will only apply while the function is running. Once the function is done running, the value goes back to what it was before your function ran.
```
name = "mahi"
>>> print(f"value is {name}")
value is mahi
>>> def change_value():
...     name="bob"
...     print(f"value is {name}")
... 
>>> change_value()
value is bob
>>> print(f"value is {name}")
value is mahi
>>> 
```

:information_source: Appropriate Use - Constant values which can be used at mutliple places

