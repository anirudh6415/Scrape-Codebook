# Creational Design Patterns

## AbstractSingleton

> A design pattern that guarantees a class has only one instance and is used as a global point of access to it.
>
> That is the one class will instantiate itself and make sure to create one instance.

<figure><img src="../.gitbook/assets/image (25).png" alt="" width="375"><figcaption></figcaption></figure>

Singletons are the simplest patterns, but harder to implement. There are multiple ways to implement the Singleton pattern.

{% hint style="info" %}
For Detailed explanation: [https://blog.algomaster.io/p/singleton-design-pattern](https://blog.algomaster.io/p/singleton-design-pattern)

For codes in Python: [https://github.com/ashishps1/awesome-low-level-design/tree/main/design-patterns/python/singleton](https://github.com/ashishps1/awesome-low-level-design/tree/main/design-patterns/python/singleton)
{% endhint %}

### 1. Lazy Initialization

* The singleton instance is created only when it is first requested.
* Saves memory if the instance is never used.
* Simple to implement, but not thread-safe in its basic form.

### 2. Thread-safe Singleton&#x20;

* Ensures only one instance is created, even in multithreaded environments.
* Uses a mutex or lock to synchronize access during instance creation.
* Slightly slower due to locking overhead.

### 3. Double-check locking

* Reduces locking overhead by checking the instance twice: before and after acquiring the lock.
* Thread-safe and more efficient than always locking.
* Slightly more complex to implement correctly

### 4. Eager initialization

* The instance is created at program startup, regardless of whether it’s used.
* Simple and inherently thread-safe.
* It may waste resources if the instance is never needed.

### 5. Bill Pugh Singleton

* Uses a static local variable or inner class for lazy, thread-safe initialization.
* An instance is created only when needed, and thread safety is guaranteed.

### 6. Enum Singleton

* Uses an enum type to guarantee a single instance.
* Simple and prevents issues with serialization and reflection.

### 7. Stack Block Initialization

* Uses a static variable declared inside a method for thread-safe, lazy initialization.
* Simple, efficient, and thread-safe.

### Pros and Cons of the Singleton pattern

| Pros                                                                                            | Cons                                                                                                                     |
| ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| 1. Ensures a single instance of a class and provides a global point of access to it.            | 1. Violates the Single Responsibility Principle: The pattern solves two problems at the same time.                       |
| 2. Only one object is created, which can be particularly beneficial for resource-heavy classes. | 2. In multithreaded environments, special care must be taken to implement Singletons correctly to avoid race conditions. |
| 3. Provides a way to maintain global state within an application.                               | 3. Introduces global state into an application, which might be difficult to manage.                                      |
| 4. Supports lazy loading, where the instance is only created when it's first needed.            | 4. Classes using the singleton can become tightly coupled to the singleton class.                                        |
| 5. Guarantees that every object in the application uses the same global resource.               | 5. Singleton patterns can make unit testing difficult due to the global state it introduces.                             |

***

## Factory Method

<figure><img src="../.gitbook/assets/image.png" alt="" width="375"><figcaption></figcaption></figure>

> The **Factory Method** is a creational design pattern that provides an **interface for creating objects in a superclass**, but **allows subclasses to alter the type of objects that will be created**.
>
> * It delegates the responsibility of object instantiation to subclasses.
> * Helps adhere to the **Open/Closed Principle**: You can introduce new products without modifying existing code.
> * Useful when your code needs to decide which class to instantiate at runtime.

### Simple Factory – Why Not Use It?

A **Simple Factory** is not a formal design pattern in the Gang of Four (GoF) book, but it’s one of the most practical and widely used refactoring techniques in real-world codebases.

In a **simple factory**, you typically use a single method with lots of `if`/`elif`/`switch` statements to create different types of objects based on input.

```python
class Dog:
    def speak(self):
        print("Bark!")

class Cat:
    def speak(self):
        print("Meow!")

class AnimalFactory:
    @staticmethod
    def create_animal(animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        else:
            raise ValueError("Unknown animal type")

# Usage
animal = AnimalFactory.create_animal("cat")
animal.speak()  # Output: Meow!
```

**Drawbacks of Simple Factory:**

* **Violates Open/Closed Principle**: You must modify the `create_animal()` method each time a new type is added.
* Harder to maintain: One centralized function with many conditionals.
* Not extensible: Client code can’t easily extend behavior without editing the factory.

### Enter: Factory Method

Instead of using if-else logic in one place, Factory Method pushes object creation to **subclasses** via a dedicated method.

#### **Structure:**

* **Product**: Base interface or abstract class.
* **ConcreteProduct**: Actual classes implementing the Product.
* **Creator**: Abstract class defining the factory method.
* **ConcreteCreator**: Subclass that overrides factory method to return a specific ConcreteProduct.

```python
from abc import ABC, abstractmethod

# Product interface
class Animal(ABC):
    @abstractmethod
    def speak(self):
        pass

# Concrete Products
class Dog(Animal):
    def speak(self):
        print("Bark!")

class Cat(Animal):
    def speak(self):
        print("Meow!")

# Creator (Abstract Factory)
class AnimalFactory(ABC):
    @abstractmethod
    def create_animal(self):
        pass

# Concrete Creators
class DogFactory(AnimalFactory):
    def create_animal(self):
        return Dog()

class CatFactory(AnimalFactory):
    def create_animal(self):
        return Cat()

# Usage
factory = CatFactory()
animal = factory.create_animal()
animal.speak()  # Output: Meow!
```

#### Why It’s Better

* **Open/Closed Principle**: Easily add new products without touching existing code.
* **Separation of concerns**: Each class has a single responsibility.
* **Extensibility**: Clients can introduce new products by subclassing.

#### When to Use Factory Method

* When the exact type of object needs to be determined at **runtime**.
* When you need to **delegate instantiation logic** to subclasses.
* When your system must support **plug-and-play product families**.

| Pros                                                                                                 | Cons                                                         |
| ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Adheres to the **Open/Closed Principle** – new products can be added without modifying existing code | Introduces **more classes** compared to simple factories     |
| **Decouples client code** from concrete product implementations                                      | Can be **overkill** for simple object creation scenarios     |
| Each **creator subclass** can have its **own specialized logic**                                     | Slightly **higher learning curve** due to use of inheritance |

***

## Abstract Factory

> The **Abstract Factory** is a **creational design pattern** that provides an interface for creating **families of related or dependent objects**, without specifying their concrete classes.

### When to Use

Use this pattern when:

* You need to create **related objects that are always used together**, such as UI elements (e.g., button + checkbox).
* You want to support **multiple product variants**, such as for **different platforms or configurations** (Windows/macOS/Linux; Android/iOS).
* You want to **enforce consistency** across products that come from the same "factory".
* You want to **isolate object creation** to easily switch product families at runtime.

### The Problem – Food Delivery App

You're building a **food delivery application** that supports multiple restaurants. Each restaurant offers a **consistent family of menu items**:

* **Appetizer**
* **Main Course**
* **Dessert**

You want to ensure that switching from Italian to Mexican cuisine automatically shows the appropriate, consistent set of menu items.

### Naive Implementation :

```python
# Italian Menu
class ItalianAppetizer:
    def create(self):
        print("Bruschetta")

class ItalianMainCourse:
    def create(self):
        print("Margherita Pizza")

class ItalianDessert:
    def create(self):
        print("Tiramisu")

# Mexican Menu
class MexicanAppetizer:
    def create(self):
        print("Guacamole")

class MexicanMainCourse:
    def create(self):
        print("Tacos")

class MexicanDessert:
    def create(self):
        print("Churros")

# Client code (Tightly coupled)
def order_meal(cuisine):
    if cuisine == "italian":
        appetizer = ItalianAppetizer()
        main_course = ItalianMainCourse()
        dessert = ItalianDessert()
    elif cuisine == "mexican":
        appetizer = MexicanAppetizer()
        main_course = MexicanMainCourse()
        dessert = MexicanDessert()
    else:
        raise ValueError("Unsupported cuisine")

    appetizer.create()
    main_course.create()
    dessert.create()

# Usage
order_meal("italian")
order_meal("mexican")
```

This works, but…

* Tightly coupled to specific classes
* No abstraction or polymorphism
* Code repetition
* Violates the **Open/Closed Principle** – adding new cuisines requires editing client logic

### What We Actually Need

* Group related components together as **families**
* Use polymorphism to treat components generically
* Encapsulate platform- or cuisine-specific creation logic
* Add new products or product families **without changing core logic**

### Class Diagram Overview

<table><thead><tr><th width="189.10089111328125">Component</th><th>Description</th></tr></thead><tbody><tr><td><strong>Abstract Factory</strong></td><td><code>RestaurantFactory</code> – defines <code>create_appetizer()</code>, <code>create_main_course()</code>, <code>create_dessert()</code></td></tr><tr><td><strong>Abstract Products</strong></td><td><code>Appetizer</code>, <code>MainCourse</code>, <code>Dessert</code></td></tr><tr><td><strong>Concrete Factories</strong></td><td><code>ItalianRestaurantFactory</code>, <code>MexicanRestaurantFactory</code>, <code>BakeryFactory</code></td></tr><tr><td><strong>Concrete Products</strong></td><td><code>Bruschetta</code>, <code>Tacos</code>, <code>Croissant</code>, etc.</td></tr><tr><td><strong>Client</strong></td><td>Food delivery app that works with abstract interfaces<br></td></tr></tbody></table>

### Abstract Factory Implementation

```python
from abc import ABC, abstractmethod

# Abstract Products
class Appetizer(ABC):
    @abstractmethod
    def prepare(self): pass

class MainCourse(ABC):
    @abstractmethod
    def prepare(self): pass

class Dessert(ABC):
    @abstractmethod
    def prepare(self): pass

# Concrete Products - Italian
class Bruschetta(Appetizer):
    def prepare(self): print("Preparing Bruschetta")

class MargheritaPizza(MainCourse):
    def prepare(self): print("Baking Margherita Pizza")

class Tiramisu(Dessert):
    def prepare(self): print("Serving Tiramisu")

# Concrete Products - Mexican
class Guacamole(Appetizer):
    def prepare(self): print("Preparing Guacamole")

class Tacos(MainCourse):
    def prepare(self): print("Assembling Tacos")

class Churros(Dessert):
    def prepare(self): print("Frying Churros")

# Abstract Factory
class RestaurantFactory(ABC):
    @abstractmethod
    def create_appetizer(self): pass

    @abstractmethod
    def create_main_course(self): pass

    @abstractmethod
    def create_dessert(self): pass

# Concrete Factories
class ItalianRestaurantFactory(RestaurantFactory):
    def create_appetizer(self): return Bruschetta()
    def create_main_course(self): return MargheritaPizza()
    def create_dessert(self): return Tiramisu()

class MexicanRestaurantFactory(RestaurantFactory):
    def create_appetizer(self): return Guacamole()
    def create_main_course(self): return Tacos()
    def create_dessert(self): return Churros()

# Client
def order_meal(factory: RestaurantFactory):
    appetizer = factory.create_appetizer()
    main = factory.create_main_course()
    dessert = factory.create_dessert()

    appetizer.prepare()
    main.prepare()
    dessert.prepare()

# Usage
order_meal(ItalianRestaurantFactory())
order_meal(MexicanRestaurantFactory())

```

#### What We Achieved

* **Consistency**: Each cuisine has a full set of related dishes.
* **Platform/Cuisine independence**: App logic works regardless of restaurant type.
* **Open/Closed Principle**: New cuisines can be added without touching client logic.
* **Polymorphism**: Client works with abstract interfaces, not concrete classes.

### Pros and Cons

| Pros                                                 | Cons                                                                  |
| ---------------------------------------------------- | --------------------------------------------------------------------- |
| Ensures **consistency** across product families      | Adds complexity due to many interfaces and classes                    |
| Supports **easy switching** between product variants | Difficult to add new product types without modifying abstract factory |
| Encourages **separation of concerns**                | Can be **overkill** for simple use cases                              |
| Adheres to **Open/Closed Principle**                 |                                                                       |

***

```mermaid
classDiagram
    class RestaurantFactory {
        <<interface>>
        +create_appetizer()
        +create_main_course()
        +create_dessert()
    }

    class Appetizer {
        <<interface>>
    }

    class MainCourse {
        <<interface>>
    }

    class Dessert {
        <<interface>>
    }

    class ItalianRestaurantFactory {
        +create_appetizer()
        +create_main_course()
        +create_dessert()
    }

    class MexicanRestaurantFactory {
        +create_appetizer()
        +create_main_course()
        +create_dessert()
    }

    class BakeryRestaurantFactory {
        +create_appetizer()
        +create_main_course()
        +create_dessert()
    }

    class Bruschetta {
        +prepare()
    }

    class Tacos {
        +prepare()
    }

    class Croissant {
        +prepare()
    }

    class Client {
        +order_meal(factory)
    }

    RestaurantFactory <|-- ItalianRestaurantFactory
    RestaurantFactory <|-- MexicanRestaurantFactory
    RestaurantFactory <|-- BakeryRestaurantFactory

    Appetizer <|-- Bruschetta
    Appetizer <|-- Tacos
    Appetizer <|-- Croissant

    Client --> RestaurantFactory
    ItalianRestaurantFactory --> Bruschetta
    MexicanRestaurantFactory --> Tacos
    BakeryRestaurantFactory --> Croissant
```

## Builder



***

## Prototype



***
