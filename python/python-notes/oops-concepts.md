---
description: >-
  OOPs are nothing but a programming paradigm that provides a structure for
  programs, so the properties and behavior are bundled into individual
  "Objects".
---

# OOPs Concepts

### **Class**

A class is a **blueprint for creating objects**. It defines the **attributes** (data) and **methods (functions)** that objects (instances) will possess.

```python
class Dog:
    species = "German shepherd" ## class attribute
    def __init__(self, name, breed):
        self.name = name ## object attribute
        self.breed = breed ## object attribute

    def bark(self): ## object method
        print(f"{self.name} says woof!")
        
dog1 = Dog("tommy","dalmatian") ## Object creation 
print(dog1.bark()) ## Object calling class method

## ====== output =============
## tommy says woof!
```

> **Usage Scenarios**: Used to model real-world entities (e.g., Dog, Car, Student) in software, application etc.\
> **Pros**: Encourages code reusability and structure, simplifies modeling complex systems, helps in task specific organization.\
> **Cons**: May add unnecessary complexity for small or simple scripts.

***

### **Inheritance**

Inheritance allows a new class (child) to inherit the attributes and methods from an existing class (parent)

There are multiple types of inheritance: Single, Multiple, Multilevel, Hierarchical, and Hybrid. [<sub>refer</sub>](https://www.geeksforgeeks.org/inheritance-in-python/)

```python
class Animal: ### Parent class
    def __init__(self,name):
        self.name = name
    def speak(self):
        return f"{self.name} sounds"

class Dog(Animal): ### Child class
    def speak(self):
        return f"{self.name} Woof!!!"
        
dog = Dog("Tommy")
print(dog.speak())

## ====== output =============
## Tommy Woof!!!
```

> **Usage Scenarios**: Useful for creating specialized versions of existing classes.\
> **Pros**: Promotes code reuse, improves readability.\
> **Cons**: Can lead to tightly coupled code; deep hierarchies may become hard to manage.

***

### **Encapsulation**

Encapsulation restricts access to internal data and methods of a class, exposing only what's necessary.&#x20;

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance        # private
        self._account_type = "Savings"  # protected
        self.owner = "Alice"            # public

    def deposit(self, amount):          # public
        self.__balance += amount
        print(f"[BankAccount] Deposited ${amount}. New Balance: ${self.__balance}")

    def access_private(self):           # public method accessing private
        self.__show_balance()

    def __show_balance(self):           # private
        print(f"[BankAccount] Balance: ${self.__balance}")


class ChildAccount(BankAccount):
    def __init__(self, balance, guardian):
        super().__init__(balance)
        self.guardian = guardian  # public

    def show_details(self):
        print(f"[ChildAccount] Owner: {self.owner}")            # ✅ Public
        print(f"[ChildAccount] Account Type: {self._account_type}")  # ✅ Protected
        # print(f"[ChildAccount] Balance: {self.__balance}")    ❌ Private (Error)
        # self.__show_balance()                                ❌ Private (Error)
        self.access_private()  # ✅ Access private through public method


# Creating objects
child = ChildAccount(1000, "John")

# Access public method and attribute
print(child.owner)                 # ✅ Output: Alice
print(child.guardian)             # ✅ Output: Bob
child.deposit(500)                # ✅ Output: Deposited $500...

# Access protected method and attribute
print(child._account_type)        # ✅ Output: Savings (discouraged outside class)
child.show_details()              # ✅ Shows protected and private access

# Accessing private attribute directly
# print(child.__balance)         ❌ AttributeError

# Accessing private using name mangling (not recommended)
print(child._BankAccount__balance)  # ✅ Output: 2500

```

> **Usage Scenarios**: Used in cases when data needs to be hided or ristricted (example: banking , API design etc.)\
> **Pros**: Protects internal state, enforces integrity.\
> **Cons**: Might require boilerplate getter/setter methods.

***

### **Polymorphism**

Polymorphism is like "H2O", which means water; it changes according to temperature.  Polymorphism allows `methods(functions) with the same name in different classes to perform distinct tasks`.

```python
class Dog:
    def speak(self):
        return "Woof"

class Cat:
    def speak(self):
        return "Meow"

def make_sound(animal):
    print(animal.speak())
    
dog = Dog()
cat = Cat()

make_sound(dog) ## Woof
make_sound(cat) ## Meow

```

> **Usage Scenarios**: Makes code more flexible and extensible.\
> **Pros**: Promotes code reuse and modularity.\
> **Cons**: Can be harder to trace bugs due to dynamic method resolution. it can cause confusion if not documented well.

***

### **Data Abstraction**

Abstraction hides complex details and shows only the essential functionality.

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start_engine(self):
        pass  

    @property
    def type_of_vehicle(self):
        return "Unknown Vehicle"

class Car(Vehicle):
    def start_engine(self):
        print("Starting car engine")

    @property
    def type_of_vehicle(self):
        return "Car"

class Boat(Vehicle):
    def start_engine(self):
        print("Starting boat engine")

    @property
    def type_of_vehicle(self):
        return "Boat"

# Example usage
car = Car()
car.start_engine()  # Output: Starting car engine
print(car.type_of_vehicle)  # Output: Car

boat = Boat()
boat.start_engine()  # Output: Starting boat engine
print(boat.type_of_vehicle)  # Output: Boat


```

> **Usage Scenarios**: Designing frameworks or APIs where implementation is hidden.\
> **Pros**: Encourages design clarity, enforces contract-based programming.\
> **Cons**: Requires more upfront planning and design.

***

### **`@staticmethod`**

Defines a method that doesn't access class or instance data.

```python
class Math:
    @staticmethod
    def add(x, y):
        return x + y

# Example usage
result = Math.add(5, 3)  # Calling static method without creating an instance
print(result)  # Output: 8

```

> **Usage Scenarios**: Utility functions that logically belong to the class but don't need access to instance or class state.\
> **Pros**: Cleaner organization of helper methods.\
> **Cons**: Can confuse if used where class context is actually needed.

***

### **`@classmethod`**

Accesses class itself (not instance).

```python
class Person:
    species = "Homo sapiens"

    @classmethod
    def info(cls):
        return cls.species

# Example usage
print(Person.info())  # Output: Homo sapiens
```

> **Usage Scenarios**: Factory methods or altering class state.\
> **Pros**: Better when the method logically relates to the class, not instance.\
> **Cons**: Can’t access instance-specific data.

***

### **`super()`**

Calls a method from the parent class.

```python
class Parent:
    def greet(self):
        print("Hello from Parent")

class Child(Parent):
    def greet(self):
        super().greet()  # Call the parent class method
        print("Hello from Child")

# Example usage
child = Child()
child.greet()  # Output: Hello from Parent \n Hello from Child

```

> **Usage Scenarios**: Calling parent methods in child classes.\
> **Pros**: Enables method extension.\
> **Cons**: Can break if inheritance hierarchy changes unexpectedly.

***

### **Method Resolution Order (MRO)**

Order in which Python looks for methods in multiple inheritance.

```python
class A: pass
class B(A): pass
class C(A): pass
class D(B, C): pass

print(D.__mro__)
## ==== Output ============
(<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

```

> **Usage Scenarios**: Important in multiple inheritance scenarios.\
> **Pros**: Helps understand method calls in complex hierarchies.\
> **Cons**: Can be confusing in diamond inheritance patterns.

***

### **Dunder (Magic) Methods**

Special methods like `__str__`, `__repr__`, `__eq__`, etc.

```python
class Book:
    def __init__(self, title):
        self.title = title

    def __str__(self):
        return f"Book: {self.title}"

book1 = Book("Python Programming")
print(book1)

## ===== output ==========
## Book: Python Programming
```

> **Usage Scenarios**: Customizing object behavior with built-in functions.\
> **Pros**: Improves integration with Python internals.\
> **Cons**: Too many magic methods can make code harder to maintain.

***

### **Composition vs Inheritance**

Composition uses object relationships; inheritance uses class hierarchies.

```python
class Engine:
    def start(self):
        print("Engine started")

class Car:
    def __init__(self):
        self.engine = Engine()  # Composition: Car has an Engine

    def start(self):
        self.engine.start()  # Delegation: Car delegates the start to Engine

# Example usage
car = Car()
car.start() 

## Output 
## Engine started
```

> **Usage Scenarios**: Composition when "has-a", inheritance when "is-a".\
> **Pros**: Composition offers better flexibility and decoupling.\
> **Cons**: Inheritance can become rigid and tightly coupled.

***

### **Operator Overloading**

Customizing behavior of operators.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

# Example usage
v1 = Vector(2, 3)
v2 = Vector(4, 5)
result = v1 + v2
print(result)  # Output: Vector(6, 8)

```

> **Usage Scenarios**: Math classes or data structures.\
> **Pros**: Improves intuitiveness of custom objects.\
> **Cons**: Misuse can lead to confusing code.

***

### Shallow Copy Vs Deep Copy

<div align="center" data-full-width="true"><figure><img src="../../.gitbook/assets/image (1) (1).png" alt="" width="563"><figcaption><p><a href="https://www.google.com/url?sa=i&#x26;url=https%3A%2F%2Fjavascript.plainenglish.io%2Funderstanding-deep-copy-and-shallow-copy-in-javascript-150116ab84bf&#x26;psig=AOvVaw2h-sP5XNU0W-b_XHrHYo86&#x26;ust=1746069232639000&#x26;source=images&#x26;cd=vfe&#x26;opi=89978449&#x26;ved=0CBcQjhxqFwoTCJjOovXk_owDFQAAAAAdAAAAABAW">source</a></p></figcaption></figure></div>

[Source](https://stackoverflow.com/questions/17246693/what-is-the-difference-between-shallow-copy-deepcopy-and-normal-assignment-oper):&#x20;

Normal assignment operations will simply point the new variable towards the existing object. The [docs](http://docs.python.org/2/library/copy.html) explain the difference between shallow and deep copies:

> The difference between shallow and deep copying is only relevant for compound objects (objects that contain other objects, like lists or class instances):
>
> * A shallow copy constructs a new compound object and then (to the extent possible) inserts references into it to the objects found in the original.
> * A deep copy constructs a new compound object and then, recursively, inserts copies into it of the objects found in the original.

Here's a little demonstration:

```python
import copy

a = [1, 2, 3]
b = [4, 5, 6]
c = [a, b]
```

Using normal assignment operatings to copy:

```python
d = c

print id(c) == id(d)          # True - d is the same object as c
print id(c[0]) == id(d[0])    # True - d[0] is the same object as c[0]
```

Using a shallow copy:

```python
d = copy.copy(c)

print id(c) == id(d)          # False - d is now a new object
print id(c[0]) == id(d[0])    # True - d[0] is the same object as c[0]
```

Using a deep copy:

```python
d = copy.deepcopy(c)

print id(c) == id(d)          # False - d is now a new object
print id(c[0]) == id(d[0])    # False - d[0] is now a new object
```

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>
