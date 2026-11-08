# Lists
## what are lists
Lists are one of Python's most versatile data structures. They let you store and organize multiple items in a single variable, making it easy to work with collections of data.
A **list** is a collection of items (sequence) enclosed in square brackets ```[]```. Each Item in a list is seperated by a ```,```

lists are:

- orderd: have an defined position (index)
- Mutable: you can add, remove, or edit after creation
- flexable: can contain items of different types 
- Indexed: each items position is numbered starting from 0

examples:
```python
fruits = ["apple", 'Grapes', "mango"]
numbers = [1,2,3,4,5]
mixed_type_list = ["bob", 35, True, 18.7]
```

---
## creating Lists
A variable is defined as a list when it is closed in square brackets ```[]```
### empty lists
an empty list variable that you can add information to later (check the ```.append()``` method)
```python
shopping_list=[]
```
### lists with items
Put items inside square brackets, separated by commas:
```python
colors=['Red', "pruple", "Green"]
test_results=[14,26,78]
```

### using the ```list()``` constructor

```python
numbers = list([1,2,3])
blank_list = list([])
```

### converting other sequence types to lists
using the ```list()``` function you can convert other sequence types to a list

example:
```python
word_list = list("hello") # ['h', 'e', 'l', 'l', 'o']
tuple_list = list((1,2,3)) #[1,2,3]
range_list = list(range(5)) #[0,1,2,3,4]
```
---
## Accessing elements
### Indexing
Access individual items using their index (position). Indexes start at 0:
```python
fruits = ["apple", 'Grapes', "mango"]
fruits[0] # 'apple' (first item)
fruits[1] # 'Grapes' (second item)
fruits[2] # 'mango' (third item)
```
### Negative indexing
you can use negative numbers to get the index from the end

```python
fruits = ['apple', 'banana', 'orange']

fruits[-1]  # 'orange' (last item)
fruits[-2]  # 'banana' (second to last)
fruits[-3]  # 'apple' (third from last)
```

### Index error
If you try to access an element that does not exist in a list you get an index error
```python
fruits = ['apple', 'banana']
fruits[5]  # IndexError: list index out of range
```

## Modifing Lists
Lists are mutable, meaning you can change them after creation.
### changing Items
Assign a new value to an index:
```python
fruits = ['apple', 'banana', 'orange']
fruits[1] = 'grape'
print(fruits)  # ['apple', 'grape', 'orange']
```
### adding items
you can add items using the following methods (built-in functions)

#### ```.append()```
adds a single item to the end of a list
```python
fruits = ['apple', 'banana']
fruits.append('orange')
print(fruits)  # ['apple', 'banana', 'orange']
```
#### ```.insert()```
insert an item to a specific index location:
```python
fruits = ['apple', 'banana']
fruits.insert(1, 'grape')  # Insert 'grape' at index 1
print(fruits)  # ['apple', 'grape', 'banana'] changes 'banana' index from 1 to 2
```
#### ```.extend()```
Add multiple items from another list:
```python
fruits = ['apple', 'banana']
more_fruits = ['orange', 'grape']
fruits.extend(more_fruits)
print(fruits)  # ['apple', 'banana', 'orange', 'grape']
```
we can also use the ```+=``` operator to add more items
```python
fruits = ['apple', 'banana']
fruits += ['orange', 'grape']
print(fruits)  # ['apple', 'banana', 'orange', 'grape']
```
### Removing items
#### ```.remove()```
remove the first occurance of a value
```python
fruits = ['apple', 'banana', 'apple']
fruits.remove('apple')
print(fruits)  # ['banana', 'apple'] only removed the 'apple' at index 0
```
#### ```.pop()```
Remove and return an item by index (defaults to last item):
```python
fruits = ['apple', 'banana', 'orange']
last = fruits.pop()
print(last)    # 'orange'
print(fruits)   # ['apple', 'banana']

first = fruits.pop(0)
print(first)   # 'apple'
print(fruits)  # ['banana']
```
#### ```del``` statment
Delete an item by index:
```python
fruits = ['apple', 'banana', 'orange']
del fruits[1]
print(fruits)  # ['apple', 'orange']
```
#### ```.clear()```
Remove all items:
```python
fruits = ['apple', 'banana', 'orange']
fruits.clear()
print(fruits)  # []
```
## Slicing
Slicing the list lets you get a portion of a list using ```[start:end]```:
```python
numbers = [0, 1, 2, 3, 4, 5]

numbers[1:4]   # [1, 2, 3] (from index 1 up to, but not including, 4)
numbers[:3]    # [0, 1, 2] (from start to index 3)
numbers[2:]    # [2, 3, 4, 5] (from index 2 to end)
numbers[:]     # [0, 1, 2, 3, 4, 5] (copy of entire list)
```
### Step Slicing
When slicing the list, add a third number for the step ```[start:end:step]```: 
```python
numbers = [0, 1, 2, 3, 4, 5]

numbers[::2]      # [0, 2, 4] (every 2nd item)
numbers[1::2]     # [1, 3, 5] (every 2nd item starting at 1)
numbers[::-1]     # [5, 4, 3, 2, 1, 0] (reversed)
```
### Modifying slices
you can change a part of the list by slicing
```python
numbers = [0, 1, 2, 3, 4, 5]
numbers[1:4] = [10, 20, 30]
print(numbers)  # [0, 10, 20, 30, 4, 5]
```
## Common operations
### Length 
get the number of items with the ```.len()``` method. note this does not retrun the last index number
```python
fruits = ['apple', 'banana', 'orange']
print(len(fruits))  # 3
```
### Membership testing 
check if an item exists with ```in```:
```python
fruits = ['apple', 'banana', 'orange']

'apple' in fruits   # True
'grape' in fruits   # False
'apple' not in fruits  # False
```
### Concatenation
Combine lists with ```+```:
```python
list1 = [1, 2, 3]
list2 = [4, 5, 6]
combined = list1 + list2
print(combined)  # [1, 2, 3, 4, 5, 6]
```

### Repetition
Repeat a list with ```*```:
```python
numbers = [1, 2, 3]
repeated = numbers * 3
print(repeated)  # [1, 2, 3, 1, 2, 3, 1, 2, 3]
```
### Iteration (looping)
loop through the items with a ```for``` loop:
```python
fruits = ['apple', 'banana', 'orange']

for fruit in fruits:
    print(fruit)
```
Get both index and value with ```.enumerate()```:
```python
fruits = ['apple', 'banana', 'orange']

for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
```

### unpacking
**unpacking** lets you assign list items to multiple variables at once. The number of variables must match the number of items:
``` python
my_list = [10, 20, 30]
a, b, c = my_list

print(a)  # 10
print(b)  # 20
print(c)  # 30
```
This is especially useful when a function returns a list or when working with fixed-size lists:
```python
# Unpacking coordinates
point = [3, 4]
x, y = point
print(f"x: {x}, y: {y}")  # x: 3, y: 4

# Unpacking RGB values
color = [255, 128, 64]
red, green, blue = color
print(f"RGB: ({red}, {green}, {blue})")  # RGB: (255, 128, 64)
```
note when unpacking you can only do that with the **exact** number of items that you want to unpack

## Useful List Methods
### ```.count()```
counts how many of an item appears in the list
```python
numbers = [1, 2, 2, 3, 2, 4]
print(numbers.count(2))  # 3
``` 
### ```.sum()```
If the List is created from all numbers you can add them all together
```python
numbers = [3, 12, 1, -4]
sum(numbers) #12
```
This method only works for a number list
### ```.index()```
Find the index of the first occurrence:
```python
fruits = ['apple', 'banana', 'orange', 'banana']
print(fruits.index('banana'))  # 1
```
### ```.sort()```
Sort the list in place (modifies the original):
sorting numbers example
```python
numbers = [3, 1, 4, 1, 5, 9, 2]
numbers.sort()
print(numbers)  # [1, 1, 2, 3, 4, 5, 9]

# Sort in reverse
numbers.sort(reverse=True)
print(numbers)  # [9, 5, 4, 3, 2, 1, 1]
```
sorting letters/strings
```python
#sorting letters:
letters = ['d', 'a', 'g', 'b']
letters.sort()
print(letters) # ['a', 'b', 'd', 'g']

#sorting strings
strings = ["hello", "Apple", "world", "bob"]
strings.sort()
print(strings) # ['Apple', 'bob', 'hello', 'world']
```

sorting will not work with a mixed value list

```python
mixed = [3, 'd', 1, 'a']
sorted(mixed)
>> TypeError!
```
### ```.reverse()```
reverses the list in place
```python
numbers = [1, 2, 3, 4]
numbers.reverse()
print(numbers)  # [4, 3, 2, 1]
```
### ```.copy()```
Create a shallow copy:
```python
original = [1, 2, 3]
copied = original.copy()
copied.append(4)
print(original)  # [1, 2, 3] (unchanged)
print(copied)    # [1, 2, 3, 4]
```
## list comprehensions
a python unique system
**List comprehensions** provide a concise way to create lists.
```python
# Create a list of squares
squares = [x**2 for x in range(5)]
print(squares)  # [0, 1, 4, 9, 16]

# With condition
evens = [x for x in range(10) if x % 2 == 0]
print(evens)  # [0, 2, 4, 6, 8]
```
This is equivalent to:

```python
squares = []
for x in range(5):
    squares.append(x**2)
```
