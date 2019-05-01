# On Terminal

py -3.7 (Win)

python3 --version (Mac)

## Creating a Virtual Environment and The Project Folder

A Virtual Environments in Python is a self-contained directory that contains a Python installation for a particular version of Python.

It’s a very useful way to make sure that we’re using the right Python version when we’re working on a particular project.

Win:

> cd $home

> mkdir pyworkshop

> cd pyworkshop

> py -3 -m venv env

> env\scripts\activate

env\scripts\activate is how you activate your virtual environment in Windows. You’ll want to do that each time you enter this Python project directory from a new shell.

Your prompt should now look like this, but with your own username.

(env) PS C:\Users\nina\pyworkshop>

For Mac / Linux:

Open a terminal window. Type the following.

>cd

>mkdir pyworkshop

>cd pyworkshop

>python3.7 -m venv env

>source env/bin/activate

### make a new empty python file called project.py

(env) $ touch project.py  (Mac)

(env)> fc > project.py  (Win)

A linter will give you code hints about a variety of different types of problems, style convention as per PEP8.

Interpreter version can be changed (in VS Code) by clicking on the status bar.

# REPL

The REPL is a handy tool for both beginner and advanced Python programmers.

REPL stands for Read, Evaluate, Print, Loop. 

It’s the place where you can instantly play around and try out Python code. 

The REPL is how you interact with the Python Interpreter

Unlike running a file containing Python code, 

in the REPL 
- you can type commands and instantly see the output printed out. 
- you can print out help for methods and objects in Python, list out what methods are available, and much more.

# Start REPL 

Cltrl/Cmd + Shift + P - opens the command palette, and start select **“Start REPL”**

*The advantage to starting the REPL from inside VS Code is that it respects the environment you already set up, that is the version of Python you chose earlier.*

In the REPL three arrows >>> indicate a line of input given at the prompt.

- "#" comments start with #. They will be ignored.
- ">>>" - this is the prompt. In example code, lines starting with >>> means they are input lines that don’t start with either of these are output that was produced by running input from the prompt

Three very useful methods in the REPL
- type() - pass object, to find the type of an object in Python.
- dir()  - stands for directory. dir(str) gives all the methods available on Strings in python.
- help() - to instantly see available documentation about the method, the parameters it expects, and what it returns. Ex: help(str.isupper)


## Creating python files with the *.py extension

- *.py extension

- Ctrl/Cmd + N to create new file. 

### Naming Tips

- Filenames should be *all lowercase
- Words should be separated with underscores _
- Filenames should be short

### What are *.pyc files?

- For optimization and other reasons, Python code can be compiled to intermediary .pyc files. 
- To safely delete them from the current project directory, run `find . -name "*.pyc" -delete` (on linux or macOS).

### Running

- Once you’ve opened your hello.py file and selected your new terminal window, open the VS Code command palette.

- open the terminal (select the termina with shelll, usually 1.)

- open the Command panel (Ctrl/Cmd +Shift+P), and type and select "Run Python File in Terminal"


# LIBRARIES

  ## Installing the requests library with pip

     3rd part library called requests to make light work of retrieving data from web APIs. 
     To install the requests library, run this on your command line:

    ```(env) $ python3.7 -m pip install requests```

    This runs the pip module and asks it to find the requests library on PyPI.org (the Python Package Index) and install it in your local system, so that it becomes available for you to import.
