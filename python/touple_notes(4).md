# Touples

Tuples are immutable sequences of items, similar to lists but with one key difference: once created, tuples cannot be modified. This immutability makes them perfect for fixed data, returning multiple values from functions, and using as dictionary keys.

## what are touples

A **Touple** is an ordered immutable collection of data They are enclosed in ```()```. Touples are:

- **orderd**: Items have a specific position (index)
- **Immutable**: Cannot be changed after creation
- **Indexed**: Each item has a position starting from 0
- **Flexible**:Can contain items of different types

```python
point = (3, 4)
colors = ('red', 'green', 'blue')
mixed = ('apple', 2, True, 3.14)  # Can mix types!
```

## why this matters?

Tuples are essential when you need fixed, unchangeable data. Their immutability makes them safe to use as dictionary keys and prevents accidental modifications. Tuples are perfect for representing fixed pairs or groups of related values, like coordinates, database records, or function return values. They're also slightly more memory-efficient than lists and can make your code's intent clearer. **If you use a tuple, you're signaling that the data shouldn't change.** Understanding when to use tuples versus lists helps you write more appropriate and efficient code.

## Creating Touples

### Tuples with items

Put items inside parentheses, separated by commas:

```python
fruits = ('apple', 'banana', 'orange')
numbers = (1, 2, 3, 4, 5)
```

### Parentheses are optional

For simple tuples, you can omit the parentheses:

```python
# These are equivalent
point1 = (3, 4)
point2 = 3, 4

print(point1 == point2)  # True
```

### Single-element tuples

A single-element tuple requires a trailing comma:

```python
# This is NOT a tuple (it's just an integer with parentheses)
not_a_tuple = (42)
print(type(not_a_tuple))  # <class 'int'>

# This IS a tuple
is_a_tuple = (42,)
print(type(is_a_tuple))  # <class 'tuple'>

# Without parentheses, comma makes it a tuple
also_tuple = 42,
print(type(also_tuple))  # <class 'tuple'>
```

### empty touples

Create an empty tuple with ():

```python
empty = ()
# or
empty = tuple()
```

### useing the ```touple()``` constructor

Convert other sequences into tuples using the ```tuple()``` constructor:

```python
numbers = tuple([1, 2, 3])  # (1, 2, 3)
chars = tuple("hello")      # ('h', 'e', 'l', 'l', 'o')
```

## Accessing Elements

### Indexing

Access individual items using their index (position), just like lists:

```python
fruits = ('apple', 'banana', 'orange')

fruits[0]   # 'apple' (first item)
fruits[1]   # 'banana' (second item)
fruits[2]   # 'orange' (third item)
```

### Negative indexing

Use negative numbers to count from the end:

``` python
fruits = ('apple', 'banana', 'orange')

fruits[-1]  # 'orange' (last item)
fruits[-2]  # 'banana' (second to last)
fruits[-3]  # 'apple' (third from last)
```

### Indexing error

Trying to access an index that doesn't exist raises an ```IndexError```:

```python
fruits = ('apple', 'banana')
fruits[5]  # IndexError: tuple index out of range
```

## Immutability

Tuples are immutable, meaning you cannot change them after creation:

```python
fruits = ('apple', 'banana', 'orange')

fruits[0] = 'grape'  # TypeError: 'tuple' object does not support item assignment
fruits.append('grape')  # AttributeError: 'tuple' object has no attribute 'append'
fruits.remove('apple')  # AttributeError: 'tuple' object has no attribute 'remove'
```

### Creating new tuples

Since tuples can't be modified, you create new tuples instead:

```python
fruits = ('apple', 'banana', 'orange')

# Create a new tuple with modified content
new_fruits = ('grape',) + fruits[1:]  # ('grape', 'banana', 'orange')

# Or replace the entire tuple
fruits = ('grape', 'banana', 'orange')
```

## slicing

You can slice tuples, just like lists ```[start:end]```:

```python
fruits = ('apple', 'banana', 'orange', 'grape')

fruits[0:2]   # ('apple', 'banana')
fruits[:2]    # ('apple', 'banana') (from start)
fruits[2:]    # ('orange', 'grape') (to end)
fruits[::2]   # ('apple', 'orange') (every 2nd item)
```

### step slicing

you can step slice tuples, just like lists ```[start:end:step]```:

```python
numbers = (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)

# Get every second element (even indices)
print(numbers[::2])  
# Output: (0, 2, 4, 6, 8)

# Get every third element
print(numbers[::3])  
# Output: (0, 3, 6, 9)
```

Slicing the tuple creates a new tuple. It doesn't modify the original.

## Common operations

### Length

Get the number of items with ```len()```:

```python
fruits = ('apple', 'banana', 'orange')
print(len(fruits))  # 3
```

### Membership Testing

Check if an item exists with ```in```:

```python
fruits = ('apple', 'banana', 'orange')

'apple' in fruits   # True
'grape' in fruits   # False
'apple' not in fruits  # False
```

### Concatenation

Combine tuples with ```+```:

```python
tuple1 = (1, 2, 3)
tuple2 = (4, 5, 6)
combined = tuple1 + tuple2
print(combined)  # (1, 2, 3, 4, 5, 6)
```

### Repetition

Repeat a tuple with ```*```:

```python
numbers = (1, 2)
repeated = numbers * 3
print(repeated)  # (1, 2, 1, 2, 1, 2)
```

### Iteration

Loop through items with ```for``` loops:

```python
fruits = ('apple', 'banana', 'orange')

for fruit in fruits:
    print(fruit)
```

## Packing and Unpacking

### Tuple packing

Creating a tuple by putting values together is called **packing**:

```python
# Packing: creating a tuple
point = 3, 4  # Packs 3 and 4 into a tuple
print(point)  # (3, 4)
```

### Tuple unpacking

Extracting values from a tuple into variables is called **unpacking**:

```python
point = (3, 4)

# Unpacking: extracting values
x, y = point
print(x)  # 3
print(y)  # 4
```

### Extended unpacking

Use ```*``` to capture multiple items:

```python
numbers = (1, 2, 3, 4, 5)

first, *rest = numbers
print(first)  # 1
print(rest)   # [2, 3, 4, 5] (note: rest is a list)

first, *middle, last = numbers
print(first)   # 1
print(middle)  # [2, 3, 4]
print(last)    # 5
```

### Multiple assignment

Tuple unpacking is often used for multiple assignment:

```python
# Swapping variables (no temporary variable needed!)
a = 5
b = 10

a, b = b, a
print(a)  # 10
print(b)  # 5
```

## Tuple methods

### ```.count()```

Count how many times an item appears:

```python
numbers = (1, 2, 2, 3, 2, 4)
print(numbers.count(2))  # 3
print(numbers.count(5))  # 0
```

### ```.index()```

Find the index of the first occurrence of a value:

```python
fruits = ('apple', 'banana', 'orange', 'banana')

print(fruits.index('banana'))  # 1
print(fruits.index('orange'))  # 2

fruits.index('grape')  # ValueError: tuple.index(x): x not in tuple
```

## Common use cases

### Returning multiple values from functions

>[Note!]
>we will learn about functions later (no need to stress about this now)

Tuples are perfect for returning multiple values:

```python
def get_name_and_age():
    return ("Alice", 30)

name, age = get_name_and_age()
print(f"{name} is {age} years old")
```

### Coordinates and pairs

Represent fixed pairs of values:

```python
point = (3, 4)
coordinates = (10, 20)

x, y = point
print(f"Point at ({x}, {y})")
```

### Dictionary keys

Tuples can be used as dictionary keys (unlike lists, which are mutable):
we will go over dictonaries later

```python
# Tuples as keys (works because they're immutable)
locations = {
    (0, 0): "Origin",
    (1, 1): "Corner",
    (2, 3): "Point"
}

print(locations[(1, 1)])  # "Corner"

# Lists can't be keys (they're mutable)
# locations[[0, 0]] = "Origin"  # TypeError: unhashable type: 'list'
```

### fixed sequences

Use tuples when the sequence shouldn't change:

```python
# Days of the week (never change)
days = ('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday')

# RGB color values (fixed)
red = (255, 0, 0)
green = (0, 255, 0)
blue = (0, 0, 255)
```

### Protecting data

Use tuples to ensure data integrity when you want to prevent accidental modifications:

```python
# Database record (shouldn't be modified)
student_record = ('Alice', '12345', 'A')
# Can't accidentally modify it - this would raise an error:
# student_record[0] = 'Bob'  # TypeError: 'tuple' object does not support item assignment

# Configuration settings that shouldn't change
app_config = ('localhost', 8080, True)  # host, port, debug_mode

# Constants or fixed values
PI_DIGITS = (3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5)
```

If you used a list instead, you could accidentally modify these values, which might cause bugs. Tuples prevent such accidental changes.

## Tuples vs lists

| Feature | Tuple | List |
| --- | --- | --- |
| Mutability | Immutable | Mutable |
| Syntax | ```()``` or commas | ```[]``` |
| Use case | fixed data or keys | Changing collections |
| Performance | Slightly faster | Slightly slower |
| Memory | Slightly less | Slightly more |

### when to use tuples

- When data shouldn't change
- When you need a dictionary key (we will cover dictonaries later)
- When returning multiple values from a function
- When you want to ensure data integrity
- For fixed, constant sequences

### When to use Lists

- When you need to add, remove, or modify items
- When working with sequences that change
- When you need list-specific methods

```python
# Good: Use tuple for fixed coordinates
point = (3, 4)

# Good: Use list for changing collection
shopping_list = ['apple', 'banana']
shopping_list.append('orange')

# Good: Use tuple as dictionary key
locations = {(0, 0): "start", (1, 1): "end"}

# Bad: Can't use list as dictionary key
# locations = {[0, 0]: "start"}  # TypeError
```

## Converting between tuples and lists

### Tuple to list

use the ```list()``` function

```python
t = (1, 2, 3)
l = list(t)
print(l)  # [1, 2, 3]
```

### List to Tuple

use the ```tuple()``` function

```python
l = [1, 2, 3]
t = tuple(l)
print(t)  # (1, 2, 3)
```
