# Ranges

Ranges represent an immutable sequence of evenly spaced integers. They're memory-efficient sequences that generate values on demand. Unlike lists, ranges don't store all values in memory, making them perfect for working with large sequences of numbers.

## What are Ranges?

A **range** is an immutable sequence of integers that follows a pattern defined by start, stop, and step values. Ranges are:

- **Immutable** : Cannot be changed after creation
- **Memory efficient**:Don't store all values in memory
- **Lazy**:Generate values as needed
- **Indexed**:Can access values by position
- **Sequence-like**: (ranges are a sequence object) Support indexing, slicing the range, and membership testing

```python
# Range from 0 to 4 (not including 5)
numbers = range(5)
print(list(numbers))  # [0, 1, 2, 3, 4]
```

## why this matters?

Ranges are essential for creating numeric sequences efficiently, especially when working with loops. They're incredibly memory-efficient because they don't store all values, instead they calculate them on demand. This makes ranges perfect for representing large sequences (even millions of numbers) without consuming excessive memory. Ranges are commonly used with for loops to repeat code a specific number of times or to generate indices for accessing other sequences. Understanding ranges helps you write cleaner loop code and work with numeric sequences without materializing large lists in memory.

## Creating Ranges

### Basic syntax

Ranges use the range() function with three possible forms`(start,stop,step)`:

```python
range(stop)           # Start at 0, go up to (but not including) stop
range(start, stop)    # Start at start, go up to (but not including) stop
range(start, stop, step)  # Start at start, step by step, up to (but not including) stop
```

### `range(stop)`

Start at 0 and count up to (but not including) the stop value:

```python
# Range from 0 to 4
r = range(5)
print(list(r))  # [0, 1, 2, 3, 4]
print(r[0])     # 0
print(r[4])     # 4
```

### `range(start,stop)`

Specify both start and stop values:

``` python
# Range from 2 to 4
r = range(2, 5)
print(list(r))  # [2, 3, 4]
print(r[0])     # 2
print(r[-1])    # 4
```

### `range(start,stop,step)`

Add a step value to control the increment:

```python
# Range from 0 to 8, step by 2
r = range(0, 10, 2)
print(list(r))  # [0, 2, 4, 6, 8]
print(r[2])     # 4

# Negative step (count backwards)
r = range(10, 0, -2)
print(list(r))  # [10, 8, 6, 4, 2]
print(r[0])     # 10
```

## Accessing Range Values

### Indexing

Access individual values by index:

```python
r = range(0, 10, 2)
print(r[0])   # 0
print(r[1])   # 2
print(r[2])   # 4
# Negive Indexing
print(r[-1])  # 8 (last value)
print(r[-2])  # 6 (second to last)
```

### slicing

Slice the range to create new ranges:

```python
r = range(0, 10, 2)
print(list(r))       # [0, 2, 4, 6, 8]

sliced = r[1:3]      # range(2, 6, 2)
print(list(sliced))  # [2, 4]

# Slice from the end
end_slice = r[-2:]
print(list(end_slice))  # [6, 8]
```

### Membership Testing

Check if a value is in the range:

```python
r = range(0, 10, 2)
print(4 in r)   # True
print(5 in r)   # False
print(8 in r)   # True
print(-2 in r)  # False
```

>[!Note]
>Memebership testing with Ranges is very memeroy efficient because it is handled mathematically, not by looping through each value in the background

### Length

Get the number of values in a range:

``` python
print(len(range(10)))        # 10
print(len(range(2, 10)))     # 8
print(len(range(0, 10, 2)))  # 5
print(len(range(10, 0, -1))) # 10
```

## Range attributes

Access the start, stop, and step values directly:

```python
r = range(3, 9, 2)
print(r.start)  # 3
print(r.stop)   # 9
print(r.step)   # 2

# Works with all range forms
r1 = range(5)
print(r1.start)  # 0 (default)
print(r1.stop)   # 5
print(r1.step)   # 1 (default)
```

## Converting ranges

### To Lists

Convert ranges to lists when you need all values at once:

```python
numbers = list(range(5))           # [0, 1, 2, 3, 4]
evens = list(range(0, 10, 2))      # [0, 2, 4, 6, 8]
backwards = list(range(10, 0, -1)) # [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

### To Tuples

Convert ranges to tuples:

```python
numbers = tuple(range(5))  # (0, 1, 2, 3, 4)
evens = tuple(range(0, 10, 2))  # (0, 2, 4, 6, 8)
```

### Creating sequences from ranges

Ranges are commonly used to generate sequences:

```python
# Even numbers from 0 to 18
evens = list(range(0, 20, 2))  
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# Multiples of 5
multiples = list(range(0, 50, 5))  
# [0, 5, 10, 15, 20, 25, 30, 35, 40, 45]

# Reverse sequence
reverse = list(range(10, 0, -1))  
# [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

## Range Methods

### `.count()`

Count how many times a value appears (always 0 or 1 for ranges):

```python
r = range(0, 10, 2)
print(r.count(4))  # 1
print(r.count(5))  # 0
print(r.count(0))  # 1
```

### `.index()`

Find the index of a value:

```python
r = range(0, 10, 2)
print(r.index(4))  # 2
print(r.index(0))  # 0
print(r.index(8))  # 4

r.index(5)  # ValueError: 5 is not in range
```

## common useage patterns

### Checking if a number is in a range

```python
age = 25
if age in range(18, 66):  # 18 to 65 (inclusive)
    print("Working age")

score = 87
if score in range(90, 101):  # 90 to 100
    print("Grade A")
```

### Generating indices for sequences

Ranges are useful for generating valid indices for other sequences

``` python
items = ['a', 'b', 'c', 'd']
indices = range(len(items))  # range(0, 4)
print(list(indices))  # [0, 1, 2, 3]

# Check if an index is valid
index = 2
if index in range(len(items)):
    print(f"Valid index: {items[index]}")
```

### Creating number sequences

Generate specific number patterns:

```python
# First 10 numbers
first_10 = list(range(10))  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Numbers from 1 to 10
one_to_ten = list(range(1, 11))  # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Even numbers
evens = list(range(0, 20, 2))  # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# Odd numbers
odds = list(range(1, 20, 2))  # [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
```

### Reversing sequences

Create reverse-ordered ranges:

```python
# Countdown from 10 to 1
countdown = list(range(10, 0, -1))  # [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]

# Countdown from 9 to 0
countdown_to_zero = list(range(9, -1, -1))  # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

## Using Ranges within Loops

Ranges are commonly used with for loops to repeat actions a specific number of times or to iterate through sequences.

``` python
# Ranges work perfectly with for loops
# (Loops will be covered in detail later)
for i in range(5):
    print(i)  # Prints 0, 1, 2, 3, 4
```

## Best practices

- **Use ranges for large sequences**: They're much more memory-efficient than creating lists
- **Convert to lists only when needed**: Only call `list(range(...))` if you actually need all values
- **Remember ranges are exclusive**: The stop value is not included in the range
- **Use negative steps for reverse sequences**: More readable than complex calculations
- **Take advantage of lazy evaluation**: Ranges compute values on demand, saving memory
