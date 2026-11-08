# Comprehensions
Comprehensions provide a concise, readable way to create lists, dictionaries, and sets by transforming or filtering data. They're a Pythonic alternative to loops when you're building collections, making your code more compact and often faster.

# What are comprehensions?
A **comprehension** is a compact way to create collections (lists, dictionaries, or sets) from iterables. Instead of writing a loop with `append()` or similar methods, comprehensions let you express the transformation in a single line.
# List comprehensions
List comprehensions create lists by transforming or filtering items from an iterable.
## Basic syntax
```python
[<expression> for <item> in <iterable>]
```
## Simple Example

```python
# Create a list of squares
squares = [x**2 for x in range(5)]  # See [ranges](./ranges) for more on range()
print(squares)  # [0, 1, 4, 9, 16]
```
This is equivalent to:
```python
squares = []
for x in range(5):
    squares.append(x**2)
```

## with conditions (filtering)
Add an `if` clause to filter items:
```python
# Only even numbers
evens = [x for x in range(10) if x % 2 == 0]  
print(evens)  # [0, 2, 4, 6, 8]
```
This is equivalent to:
```python
evens = []
for x in range(10):
    if x % 2 == 0:
        evens.append(x)
```
## Transforming Items
Apply operations to each item:
```python
# Convert strings to uppercase
words = ['hello', 'world', 'python']
uppercase = [word.upper() for word in words]
print(uppercase)  # ['HELLO', 'WORLD', 'PYTHON']

# Extract first character
first_chars = [word[0] for word in words]
print(first_chars)  # ['h', 'w', 'p']

# Multiply numbers
numbers = [1, 2, 3, 4, 5]
doubled = [x * 2 for x in numbers]
print(doubled)  # [2, 4, 6, 8, 10]
```
## Filtering and transforming
Combine filtering and transformation:
```python
# Square only even numbers
squares_of_evens = [x**2 for x in range(10) if x % 2 == 0]
print(squares_of_evens)  # [0, 4, 16, 36, 64]
```
## Multiple conditions
Use and to combine multiple conditions:
```python
# Numbers divisible by 2 and 3
divisible_by_6 = [x for x in range(30) if x % 2 == 0 and x % 3 == 0]
print(divisible_by_6)  # [0, 6, 12, 18, 24]
```
## Nested
List comprehensions can be nested:
```python
# Flatten a matrix (2D list)
matrix = [
    [1, 2, 3], 
    [4, 5, 6], 
    [7, 8, 9]
    ]
flattened = [num for row in matrix for num in row]
print(flattened)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```
This is equivalent to:
```python
flattened = []
for row in matrix:
    for num in row:
        flattened.append(num)
```
## Complex examples
```python
# Extract lengths of words longer than 3 characters
words = ['apple', 'an', 'banana', 'it', 'orange']
lengths = [len(word) for word in words if len(word) > 3]
print(lengths)  # [5, 6, 6]

# Create tuples from two lists
names = ['Alice', 'Bob', 'Charlie']
ages = [25, 30, 35]
pairs = [(name, age) for name, age in zip(names, ages)]
print(pairs)  # [('Alice', 25), ('Bob', 30), ('Charlie', 35)]
```

# Dictionary comprehensions
Dictionary comprehensions create dictionaries from iterables.
## Basic syntax
```python
{<key_expression>: <value_expression> for <item> in <iterable>}
```
## simple example
```python
# Square each number as both key and value
squares = {x: x**2 for x in range(5)}
print(squares)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```
## From existing dictionary
Transform an existing dictionary:
```python
# Uppercase all keys
original = {'name': 'Alice', 'age': 30, 'city': 'Boston'}
upper_keys = {k.upper(): v for k, v in original.items()}
print(upper_keys)  # {'NAME': 'Alice', 'AGE': 30, 'CITY': 'Boston'}
```
# Comparing comprehensions and loops
## When to use comprehensions
Comprehensions are great when:

- You're creating a new collection (list, dict, or set)
- The transformation is simple and readable
- You want concise, Pythonic code
```python
# Good: Simple transformation
squares = [x**2 for x in range(10)]

# Good: Simple filtering
evens = [x for x in range(10) if x % 2 == 0]
```
## when to use loops
Use loops when:

- The logic is complex or has multiple steps
- You need side effects (like printing) beyond creating a collection
- You need to break or continue based on conditions
- Readability would suffer with a comprehension

```python
# Better with a loop: Complex logic
results = []
for x in range(10):
    squared = x**2
    if squared > 20:
        print(f"Found large square: {squared}")
        results.append(squared)
    elif squared == 0:
        continue
    else:
        results.append(squared)
```

