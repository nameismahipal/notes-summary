## Data types
Python variables can’t start with with a number. 

In general, they’re named all lower case, separated by underscores.

## Variables

We assign values to variables by putting the value to the right of an equal sign.

Because Python is a dynamic language, we don’t need to declare the type of the variables before we store data in them.

Ex: x = 18

type of what’s contained in Python variables can change at any time.

Ex: 
- x = 18
- x = "One"

Because Python is a dynamic language and you don’t have type hints to explain what’s stored inside a variable while reading code, you should do your best naming your variables to describe what is stored inside of them.

:warning: Python will let you override built-in methods and types without a warning so don’t name your Python variables things like **list**, **str**, or **int**.


### No-Value, None, or Null Value
There’s a special type in Python that signifies no value at all. In other languages, it might be called Null. 

In Python, it’s called None.

## Numbers

three different types of numbers in Python: 
- int, Ex: x = 4 
- Float, Ex: x = 1.45
- Complex. Ex: x = 4j

## Strings

Strings in Python can be enclosed either with single quotes like 'hello' or double quotes, like "hello".

concatenation: + operator

escape character: \ - backwards slash

""" (triple quotes): Long multi-line strings can be represented in between 

Ex:
```
long_greeting = """
                Greetings and salutations, dear Mahi.
                I'm superfluous with my words,
                and require more space to say Hello!"
                """
```

Printing Strings: print(#variable-ame#)

### String Formatting

f-strings

f-strings allow you to simply and easily reference variables in your code.
Ex:
```
name = "Nina"
greeting = f"Hello, {name}"
```