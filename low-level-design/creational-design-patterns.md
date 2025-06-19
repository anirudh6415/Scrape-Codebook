# Creational Design Patterns

## Singleton

> A design pattern that guarantees a class has only one instance and is used as a global point of access to it.
>
> That is the one class will instantiate itself and make sure to create one instance.

<figure><img src="../.gitbook/assets/image (25).png" alt="" width="375"><figcaption></figcaption></figure>

Singletons are the simplest patterns, but harder to implement. There are multiple ways to implement the Singleton pattern.

{% hint style="info" %}
For code and Detailed explanation: [https://blog.algomaster.io/p/singleton-design-pattern](https://blog.algomaster.io/p/singleton-design-pattern)
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



***

## Factory Method



***

## Abstract Factory



***

## Builder



***

## Prototype



***
