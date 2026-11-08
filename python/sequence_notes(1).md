# python sequences

python sequences are orderd collection of where each item has a position (index).  
Sequences are useful because they help us organize data.  

## key characteristics

All sequences have the same properties

- **Ordered**: items mantain their position
- **Indexed**: each item has a numaric position, (in python all indexes start with 0)
- **Iterable**:you can loop through them (we will cover loops later)
- **Length**: there is a set length

## types of sequences

- **Lists**: Mutable, changeable collections
- **tuples**: Immutable, unchangeable collections
- **strings**: Immutable sequences of characters- used for text data (we already coverd strings but this is another way to think of them)
- **Ranges**:immutable sequences of numbers-memory-efficient for numeric sequences
- **Bytes**:Immutable sequences of bytes (for binary data, we need to know that it exists but we dont need to cover it in depth)
- **Bytearrays**:Mutable sequences of bytes (for binary data, we need to know that it exists but we dont need to cover it in depth)

---

## Basic Sequence concepts

### indexing

You can access individual items by their position using square brackets []. *Indexes start at 0* :

``` python
text = "python"
numbers = [1,2,3,4,5]

text[0] #p the first position
numbers[1] #2 the second position
```

### Slicing
You can slice (remove a section) the sequence to extract portions using ```[Start:End:Step]```

```python
text = "Python"
text[0:3] #'Pyt': slices the first three positions (defalut step is 1 so you dont need to write it out)
text[:3] #'Pyt': slices the first three positions (defalut of start is 0)
text[3:] #'hon': slices from index 3 (the fourth position to the end)
text[0:3:2] # 'Pt' starts at the first position, slices to the third, only brings the every other item 
```

### Length and membership

You can check how many items are in a sequence using ```len()``` and whether an item exists using ```in```:
```python
len("hello") # 5 there are 5 characters 
3 in [1,2,3,4,5] #True there is a number 3 in that list
'a' in 'python' #False; there is no 'a' in the python string
```
The ```in``` operator checks for the presence of a single element (like a number or a character), not a slice or pattern. It returns True only if that exact element is found in the sequence.

### Iteration (loops)
we will go over loops indepth later but all sequeces can be looped like the following example.

input:
```python
for num in [1,2,3,4,5]:
    print(num)
```

output:
```python
1
2
3
4
5
```
### Mutability: A key distinction
The biggest differences with the basic sequence types are if we can change them after we create them. This is called **Mutability**.

#### Imutable sequences

- Tuples:```(1,2,3)```
- Strings:```"hello world"```
- ranges: ```range(10)```
- Bytes: ```b"hello"```

once these data types were created we cannot add, remove, or change these items in anyway, we need to create a new item with the change.
When you assign a "changed" string (or other immutable type) to a variable, you're actually creating a new object and overwriting what the variable points to. The original string itself remains unchanged.

#### Mutable sequences

- lists ```[1,2,3,4]```
- Bytearrays: ```bytearray(b"hello")```

You can add, remove, or modify items in place without creating a new sequence.

The choice between mutable and immutable sequences depends on your needs. Immutable sequences are safer (they can't be accidentally changed) and can be used as dictionary keys. Mutable sequences are more flexible when you need to modify data.

