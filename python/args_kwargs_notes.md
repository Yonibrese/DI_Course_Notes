# what are `*args` and `**kwargs`?

`*args` and `**kwargs` are special syntax in Python that allow functions to accept a variable number of arguments. The `*args` collects extra positional arguments into a tuple, while `**kwargs` collects extra keyword arguments into a dictionary. The names `args` and `kwargs` are just conventions, so you can use any names you want, but the `*` and `**` operators are what make them work.

```python
def example(*args, **kwargs):
    print("Positional arguments:", args)
    print("Keyword arguments:", kwargs)

example(1, 2, 3, name="Alice", age=30)
# Positional arguments: (1, 2, 3)
# Keyword arguments: {'name': 'Alice', 'age': 30}
```

## naming Conventions

While `args` and `kwargs` are just variable names, they're widely used conventions. You can use any names:

```python
def example(*numbers, **options):
    # numbers is a tuple
    # options is a dict
    pass
```

However, using `args` and `kwargs` makes your code more readable to other Python developers.

## why this matters

`*args` and `**kwargs` give you flexibility when you don't know ahead of time how many arguments a function will receive. They're essential for writing functions that can handle different numbers of inputs, which is common in real-world programming. For example, a function that prints multiple values, a function that processes a variable number of items, or a wrapper function that needs to pass arguments to another function all benefit from using `*args` and `**kwargs`. Understanding these concepts opens up powerful patterns in Python, from decorators to function wrappers to APIs that need to accept flexible input.

## `*args`: variable positional argument

The `*args` syntax collects any number of positional arguments into a tuple. The `*` operator unpacks arguments when calling a function, and collects them when defining a function.

### Basic `*args` example

```python
def sum_all(*args):
    """Sum all provided numbers."""
    total = 0
    for num in args:
        total += num
    return total

print(sum_all(1, 2, 3))         # 6
print(sum_all(10, 20, 30, 40))  # 100
print(sum_all(5))               # 5
print(sum_all())                # 0
```

Inside the function, `args` is a ***tuple*** containing all the positional arguments passed to the function:

```python
def show_args(*args):
    print(f"Type: {type(args)}")
    print(f"Value: {args}")

show_args(1, 2, 3)
# Type: <class 'tuple'>
# Value: (1, 2, 3)
```

### Combining regular parameters with `*args`

You can mix regular parameters with `*args`. Regular parameters must come before `*args`:

```python
def greet(greeting, *names):
    """Greet multiple people with the same greeting."""
    for name in names:
        print(f"{greeting}, {name}!")

greet("Hello", "Alice", "Bob", "Charlie")
# Hello, Alice!
# Hello, Bob!
# Hello, Charlie!
```

### Unpacking with `*args`

You can also use `*` to unpack a sequence when calling a function:

```python
def add(a, b, c):
    return a + b + c

numbers = [1, 2, 3]
print(add(*numbers))  # 6
# Equivalent to: add(1, 2, 3)
```

This is particularly useful when you have a list or tuple and want to pass its elements as separate arguments:

```python
def multiply(x, y, z):
    return x * y * z

values = [2, 3, 4]
result = multiply(*values)  # 24
```

## `**kwargs`: Variable keyword arguments

The `**kwargs` syntax collects any number of keyword arguments into a dictionary. The `**` operator unpacks keyword arguments when calling a function, and collects them when defining a function.

### Basic `**kwargs` example

```python
def print_info(**kwargs):
    """Print all provided keyword arguments."""
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=30, city="New York")
# name: Alice
# age: 30
# city: New York
```

Inside the function, `kwargs` is a ***dictionary*** containing all the keyword arguments:

```python
def show_kwargs(**kwargs):
    print(f"Type: {type(kwargs)}")
    print(f"Value: {kwargs}")

show_kwargs(a=1, b=2, c=3)
# Type: <class 'dict'>
# Value: {'a': 1, 'b': 2, 'c': 3}
```

### Combining regular parameters with `**kwargs`

you can mix regular parameters with `**kwargs`. Regular parameters and `*args` must come before `**kwargs`:

```python
def create_profile(name, **details):
    """Create a user profile with a name and optional details."""
    profile = {"name": name}
    profile.update(details)
    return profile

user = create_profile("Alice", age=30, city="NYC", role="Developer")
print(user)
# {'name': 'Alice', 'age': 30, 'city': 'NYC', 'role': 'Developer'}
```

### Unpacking with `**kwargs`

You can use `**` to unpack a dictionary when calling a function:

```python
def greet_person(name, age, city):
    return f"{name}, {age}, from {city}"

info = {"name": "Alice", "age": 30, "city": "New York"}
print(greet_person(**info))
# Alice, 30, from New York
```

This is useful when you have a dictionary and want to pass its key-value pairs as keyword arguments:

```python
def configure_app(host, port, debug):
    print(f"Host: {host}, Port: {port}, Debug: {debug}")

config = {"host": "localhost", "port": 8000, "debug": True}
configure_app(**config)
# Host: localhost, Port: 8000, Debug: True
```

## Using `*args` and `**kwargs` together

You can use both `*args` and `**kwargs` in the same function. The order must be: regular parameters, `*args`, then `**kwargs`:

```python
def example_func(required, *args, **kwargs):
    print(f"Required: {required}")
    print(f"Args: {args}")
    print(f"Kwargs: {kwargs}")

example_func("hello", 1, 2, 3, name="Alice", age=30)
# Required: hello
# Args: (1, 2, 3)
# Kwargs: {'name': 'Alice', 'age': 30}
```

### Practical example: Flexible logger

```python
def log(message, *values, level="INFO", **metadata):
    """Log a message with optional values and metadata."""
    print(f"[{level}] {message}")
    if values:
        print(f"  Values: {values}")
    if metadata:
        print(f"  Metadata: {metadata}")

log("Processing started", 1, 2, 3, level="DEBUG", user="Alice", task="import")
# [DEBUG] Processing started
#   Values: (1, 2, 3)
#   Metadata: {'user': 'Alice', 'task': 'import'}
```
