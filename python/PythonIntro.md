# Intro to python

## Welcome to python

Python is a powerful, beginner-friendly programming language used for everything from web development to data science, automation, and artificial intelligence. It's known for its clean, readable syntax that reads almost like English, making it an excellent choice for your first programming language.

## why python

Python is designed to be easy to learn and use. Unlike some languages that require lots of setup and complex syntax, Python lets you focus on solving problems rather than fighting with the language itself. It's also incredibly versatile, so the same language you use to write a simple script can power major applications used by millions of people.

## what you need

Inorder to get started with python you will need the following

- python installed on to your machine (3.8 or later)
  - windows/macOS: if not installed go to this link and download: [python download](https://www.python.org/downloads/)
  - linux/wsl: python should already be installed but, you need to varify the installation (see below)
- **A Text editor** or a **code editor** (vs code)
- A **Terminal or Command prompt** to run the code

## Checking if python is installed

open your terminal or command prompt and type

```bash
python --version
```

or

```bash
python3 --version
```

If python is installed on your machine then you will see a response like `Python 3.12.3` If there is an error then you need to install python on to your machine

## Your first python program

lets run the taditional first coding progam, printing a string to your terminal

### option 1: using python interactively

python comes with a built in interactive shell, that you can run through your terminal

to run this you need to go to your termainal and type `python` or `python3`

The interactive shell is useful for learning or quick testing of some code

```python
>>>print("hello world!")
hello world!
```

(you just ran a python program)

### option2: creating a python file

for real (more complex) programs you will create a file. A python file is defined with the `.py` file type

create a python file and type the following line

```python
print("Hello world!")
```

then to run the code in your termainal write

```bash
python <Filename>.py
```

or

```bash
python3 <Filename.py>
```

you should see

```bash
hello world
```

## what just happened?

To break down what you just did

- `print()` is a built-in python that prints strings (or other values) to the screen
- `"Hello world"` Is a *string* which is a peice of text enclosed in quotes (`""` or `''`)
- when you ran the program Python excuted the print function

## How python runs code

>[!important]
>Understanding how Python executes your code is crucial for learning Python effectively and for running your code properly

### Top to bottom execution

Python executes your code line by line, from top to bottom, just like reading a book:

```python
print("First")
print("Second")
print("Third")
```

output:

```python
First
Second
Third
```

python does not look a head or jumps around. it reads line by line

### Runtime execution

Python executes code **when you run the program** (this is called "runtime"), not when you write it or save the file. This means:

- Python doesn't check for errors until it tries to run a line
- You can write code that has errors, and Python won't complain until it reaches that line
- The file can be opened and viewed without any problems. The errors only appear when Python tries to execute the code

```python
print("Hello")
print(unknown_thing)  # This line has an error, but Python doesn't know until it tries to run it
print("World")
```

When you run this, Python will:

1. Successfully run line 1 and print "Hello"
2. Try to run line 3 and then discover the error (because `unknown_thing` doesn't exist)
3. not print "world" because the program has stopped

Python runs the code up to the error, then stops.

## Python Help Tools

when you are in the Interactive shell you can use two built in functions that can help you understand some of what python is doing in the background

### The help system

Python's built-in `help()` system provides interactive access to documentation, modules, classes

example:

```python
>>> help(str)
Help on class str in module builtins:

class str(object)
|  str(object='') -> str
|  str(bytes_or_buffer[, encoding[, errors]]) -> str
.....
```

you can also use the `dir()` function to check the buit-in methods and attributes of an object (without the full documentation)

```python
>>>bob = "This is a string"

>>>['__add__', '__class__', '__contains__', '__delattr__',
 '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
 '__getitem__', '__getnewargs__', '__getslice__', '__gt__',
 ...
 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines',
 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper',
 'zfill']

 >>>dir(bob.join)
 <built-in method join of str object at 0x7599f8aabcb0>
```
