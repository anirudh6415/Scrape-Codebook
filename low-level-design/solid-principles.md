# SOLID Principles

These five software development principles are guidelines to follow when building software so that it is easier to scale and maintain. They were made popular by a software engineer, [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_C._Martin).

<figure><img src="../.gitbook/assets/SOLID.png" alt=""><figcaption></figcaption></figure>

### For better visual understanding:

[The S.O.L.I.D Principles in Pictures](https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898)

### The SOLID Principles <a href="#id-29fe" id="id-29fe"></a>

> TO MAKE EASIER AND SIMPLE TO FOLLOW.&#x20;
>
> 1. I WILL BE USING WORD `"CLASS"`  FOR SIMPLICITY.
> 2. THESE BELOW PRINCIPLES CAN ALSO BE APPLIED TO `"FUNCTIONS", "METHOD" OR "MODULE".`

#### S - Single Responsibility Principle (SRP) <a href="#id-8699" id="id-8699"></a>

> A `CLASS` should only have one reason to change. i.e. one responsibility

**Why:** If a class has multiple responsibilities, changes to one responsibility can unintentionally affect others, increasing the risk of bugs.

**Goal:** Separate behaviors so that changes in one area do not impact unrelated areas.

**Example:** A `Report` class should only handle report generation, not saving or emailing reports. Those should be separate classes

```python
```

***

#### O - Open-Closed Principle (OCP) <a href="#id-3984" id="id-3984"></a>

> A `CLASS` should be open for extension but closed for modification.

**Why:** Modifying existing code can introduce bugs in systems that rely on that code. Instead, extend functionality by adding new code.

**Goal:** Allow a class’s behavior to be extended without changing its existing code, reducing the risk of breaking other parts of the system.

**Example:** Add new features by creating subclasses or using interfaces, rather than altering the original class.

```python
```

***

#### **L -** **Liskov Substitution P**rinciple (LSP)

> If S is a subtype of T, then objects of type T in a program may be replaced with objects of type S without altering any of the desirable properties of that program.

**Why:** If a subclass cannot perform the same actions as its parent, it can cause unexpected bugs.

**Goal:** Ensure that derived classes extend the base class without changing its expected behavior.

**Example:** If a `Coffee` class has a `Cappuccino` subclass, the subclass should behave like a `Coffee` In all contexts where `Coffee` is expected.

```python
```

***

#### **I -** **Interface Segregation** Principle (ISP)

> Classes should not be forced to depend on methods they do not use.

**Why:** Large, general-purpose interfaces force classes to implement unnecessary methods, leading to bloated and fragile code.

**Goal:** Split large interfaces into smaller, client-specific ones so that classes only implement what they need.

**Example:** Instead of one big interface for all printer functions, have separate interfaces for scanning, printing, and faxing.

```python
```

***

#### **D -** **Dependency Inversion** Principle (DIP)

> High-level modules should not depend on low-level modules. Both should depend on the abstraction.
>
> Abstractions should not depend on details. Details should depend on abstractions.

**Why:** Direct dependencies between high-level and low-level modules make code rigid and hard to change.

**Goal:** Reduce coupling by introducing interfaces or abstract classes, making the system more flexible and testable.

**Example:** A `LightSwitch` class should depend on a `SwitchableDevice` interface, not directly on a `LightBulb` class

```python
```

***
