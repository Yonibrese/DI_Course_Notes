# Python OOP Introduction

## What is object-oriented programming?

**Object-Oriented Programming (OOP)** is a way of organizing code around **objects** instead of just functions and data. An object bundles together:

- **data**(attributes): what the object *has*
- **Behavior** (methods or built in functions) — what the object *does*

In Python, almost everything is an object: strings, lists, functions, even modules and classes themselves.

```python
name = "Alice"
print(type(name)) # <class 'str'> the class header denotes that the variable is a str object
print(name.upper())   # 'ALICE'  (method on a string object)
```

When you define your own classes, you create your own types of objects to model things in your program: users, orders, game characters, shapes, and more.

## Why this matters

OOP becomes important when your programs grow beyond a few functions. It helps you:

- **Group related data and behavior together** (easier to understand and change)
- **Avoid repetition** by sharing behavior between related classes
- **Model real-world concepts** directly in code (User, Car, BankAccount, etc.)
- **Encapsulate complexity** so other parts of your program don't need to know internal details

Most large Python codebases use OOP heavily, so understanding classes, instances, and methods is key to reading and writing professional Python.

## Key concepts

### Classes and instances

A *class* is a blueprint for creating objects. An *instance* is a specific object created from that blueprint. Think of a class like a cookie cutter that you can use to make many cookies (instances), all shaped the same way.

```python
# Car is the class (the blueprint)
class Car:
    pass

# my_car is an instance (a specific object created from Car)
my_car = Car()
```

### Attributes

**Attributes** are pieces of data stored on objects. Each instance can have its own attribute values:

```python
my_car.color = "Blue"  # attribute storing the car's color
your_car.color = "Red"  # different instance, different value
```

### Methods

**Methods** are functions attached to a class that work with the instance's data:

```python
my_car.start_engine()  # calling a method on the instance
```

I will go over this in depth later

### Encapsulation (preview)

**Encapsulation** means keeping an object's internal state private (by convention) and exposing a clear public interface through methods and attributes.

Python doesn't enforce strict privacy like some languages, but it uses naming conventions:

- Names without underscores: part of the public interface
- Single leading underscore: "internal use"
- Double leading underscore: name-mangled to avoid accidental overrides

### Inheritance and polymorphism (preview)

(we will go over this in depth next week)

Two more key ideas in OOP:

- **Inheritance**: create new classes that reuse and extend existing ones
- **Polymorphism**: different classes can be used interchangeably if they share a common interface
  
Example of simple inheritance:

```python
class Animal:
    def speak(self):
        return "Some sound"

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

animals = [Dog(), Cat()]
for a in animals:
    print(a.speak())  # "Woof!", then "Meow!"
```

Here, `Dog` and `Cat` inherit from `Animal`, and both override speak, but you can treat them all as "animals that can speak."
