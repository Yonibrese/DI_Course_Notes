# Python variables

## what are variables

**Variables** are names that refer to values. They let you store data and give it meaningful labels, making your code readable and maintainable. Python doesn't use type declarations; a name is bound to an object at runtime and can later be rebound to another.

```python
message = "hello"   # name -> string object
count = 3           # name -> int object
pi = 3.14159        # name -> float object
```

## Why this matters

Variables are essential for writing useful programs. Without variables, you'd have to repeat values throughout your code, making it hard to maintain and update. Variables let you store user input, intermediate calculations, and results that you'll use later. They also make your code self-documenting. A well-named variable like `user_age` is much clearer than just using the number 25 scattered throughout your code. Understanding how variables work, especially how they reference objects and how mutability affects them, is crucial for avoiding bugs and writing clean Python code.

## Nameing rules and styles

- Letters, digits, and underscores. Cannot start with a digit.
- Case-sensitive: `name` and `Name` are different.
- Avoid keywords like `for`, `if`, `class`. (these are used as python codes)
- Follow PEP 8: use `lower_snake_case` for variables.

```python
user_name = "ada"
max_items = 10
```

## Assignment and rebinding

`=` binds a name to a value. Reassigning (rebinding) changes the binding, not the original object.

```python
x = 1
x = x + 2   # x now refers to 3
```

Multiple assignment is supported:

```python
a, b = 1, 2
a, b = b, a      # swap
```

Underscore is often used as a throwaway name:

```python
first, _, third = [10, 20, 30]
```

## Mutablity vs Immutability

Rebinding works the same for all objects, but mutating an object changes it in place.

```python
nums = [1, 2, 3]
alias = nums
alias.append(4)
print(nums)   # [1, 2, 3, 4] (same list, two names)
```

For immutable objects, you can only rebind:

```python
name = "hi"
name = name + " there"   # creates a new string; old one unchanged
```

## Identity vs equality

This is an important concept that confuses many beginners. There are two ways to compare values in Python:

- **Equality** (`==`): Checks if two values have the same **contents** or are equivalent.
- **Identity** (`is`): Checks if two names refer to the **exact same object** in memory.

Think of it like this, equality asks "do these have the same value?" while identity asks "are these the exact same thing?"

```python
>>> a = [1, 2, 3]
>>> b = [1, 2, 3]
>>> a == b    # Do they have the same contents? Yes!
True
>>> a is b    # Are they the same object? No! They're two different lists
False
```

Here's what's happening: `a` and `b` are two separate lists that happen to contain the same values. They're like two identical shopping lists written on different pieces of paper. They have the same contents, but they're different objects.

But if you assign one name to another:

```python
>>> a = [1, 2, 3]
>>> b = a      # b now refers to the same list as a
>>> a is b     # Are they the same object? Yes!
True
>>> a == b     # Do they have the same contents? Yes!
True
```

Now both `a` and `b` point to the same list object.
