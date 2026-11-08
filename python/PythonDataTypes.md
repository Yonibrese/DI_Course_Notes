# Python Data Types

In python all values, are **objects** but they have different **Types**.

## What is a type?

A **type** describes what a value is and which operations make sense for it. For example, you can add two numbers together, but you can't add a number to a word.

Python is **dynamically typed**, which means you don't need to tell Python what type a value is when you create it. Python figures this out automatically. This makes Python easier to learn than languages where you must declare types ahead of time.

```python
x = 3          # int
y = "hello"    # str
z = [1, 2, 3]  # list
```

Use type(value) to see a value's type:

```python
>>> type(3)
<class 'int'>
>>> type("hello")
<class 'str'>
>>> type([1,2,3])
<class 'list'>
```

## why this matters

Understanding types is fundamental to Python programming. Every value has a type, and the type determines what you can do with that value. You can add numbers, concatenate strings, and append items to lists, but trying to add a string to a number or call list methods on a number will cause errors. Types help you write correct code by defining what operations are valid for each kind of data. As you learn Python, you'll constantly work with different types, so getting comfortable with the core types early will make everything else easier.

## Core built-in types

- Numbers: `int`, `float`, `complex`
- Text (string): `str`
- Booleans (true,false): `bool`
- Empty/“no value”: `None` (the single instance of type `NoneType`)
- Collections
  - `list` (mutable sequence)
  - `tuple` (immutable sequence)
  - `dict` (key/value mapping)
  - `set` / `frozenset` (unique unordered elements)

### Numbers (`int`,`float`, `complex`)

Python has three types of numbers:

```python
>>> 42           # int (whole numbers like 1, 2, 100, -5)
>>> 3.14         # float (decimal numbers like 3.14, 2.5, -0.1)
>>> 1 + 2j       # complex (for advanced math)
```

For now, focus on `int` and `float`. You'll use `int` for counting things (1, 2, 3) and `float` for measurements and calculations with decimals (3.14, 2.5).

#### basic number operations

you can use all sorts of basic math operations with numbers

- addition

  ```python
  >>> 3 + 18
  21
  ```

- subtraction
  
  ```python
  >>> 67 - 18
  49
  ```

- multipication
  
  ```python
  >>> 3 * 9
  27
  ```

- Division
  
  ```python
  >>> 12/5
  2.4
  ```

There are more operator, but I will show them indepth in a different guide

### Strings (`str`)

Text is stored in strings, enclosed in quotes. Strings are **immutable**, which means once you create a string, you can't change individual characters in it. If you need to change a string, you create a new one instead.

```python
>>> "hello"
'hello'
>>> "hi " + "there"
'hi there'
>>> "ha" * 3
'hahaha'
```

you can use some of the math operators with strings(see the above example)

#### Basic string methods

the following are examples of some of the built in string functions (or methods)

- You can capitalize a string
  
  ```python
  >>> 'hello'.capitalize()
  'Hello'
  ```

- Uppercase a string
  
  ```python
  >>> 'hello'.upper()
  'HELLO'
  ```

- Lowercase a string
  
  ```python
  >>> 'HELLO'.lower()
  'hello'
  ```

- Replace content inside of a string
  
  ```python
  >>> 'goodnight moon'.replace('goodnight', 'hello')
  'hello moon'
  ```

### Booleans (`bool`)

`True` and `False` are booleans—they represent *yes/no* or *on/off* states (or you can think of them as binary states 1/0). You'll use these when you learn about conditionals (making decisions in your code).

```python
>>> True
True
>>> False
False
>>> bool(0)      # 0 converts to False
False
>>> bool("text") # Non-empty strings convert to True
True
```

>[!important]
>You'll learn more about how Python treats different values as "truthy" or "falsy" in the truthiness guide. For now, just know that True and False are the boolean values.

### None

`None` represents "no value" or "nothing here." It's Python's way of saying "this doesn't have a value yet" or "this is empty."

```python
result = None
print(result)  # None
```

You'll use `None` when you want to indicate that a variable doesn't have a value yet, or when a function doesn't return anything. We'll see more examples of this as you learn more about Python.

### Collections (containers)

Collections are types that can hold multiple values (and each of those values can be different types):

```python
>>> [1, 2, 3]                 # list - ordered, can be changed
>>> (1, 2, 3)                 # tuple - ordered, cannot be changed
>>> {"a": 1, "b": 2}          # dict - stores key/value pairs
>>> {1, 2, 3}                 # set - stores unique items
```

**Mutable vs Immutable**:

- **Mutable** types (like `list`) can be changed after you create them. You can add, remove, or modify items.

- **Immutable** types (like `tuple` and `str`) cannot be changed after creation. If you need to change them, you create a new one.

we will go more indepth with collections later

## Checking types

you can check what type a value is by using `type()`:

```python
>>> type(3)
<class 'int'>
>>> type("hello")
<class 'str'>
```

You can also use `isinstance()` to check if a value is a specific type:

```python
>>> isinstance(3, int)
True
>>> isinstance(3.5, int)
False
```

## converting between types

Sometimes you need to convert a value from one type to another. Python provides functions for this:

```python
>>> int("5")        # Convert string to integer
5
>>> float("3.14")   # Convert string to decimal number
3.14
>>> str(42)         # Convert number to string
'42'
>>> list("abc")     # Convert string to list of characters
['a', 'b', 'c']
```

>[!warning]
>Be careful: converting invalid input will cause an error. For example, you can't convert the text "not a number" into a number
