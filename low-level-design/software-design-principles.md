# Software Design Principles

Design principles are essential for building maintainable, scalable, and robust software systems.&#x20;

### 1. DRY (Don't Repeat Yourself) <a href="#id-1-dry-dont-repeat-yourself" id="id-1-dry-dont-repeat-yourself"></a>

* **Definition:** The DRY principle emphasizes reducing code duplication by ensuring that each piece of logic has a single, authoritative source within the system.
* **Why:** Reduces maintenance effort and the risk of inconsistencies or bugs when changes are made.
* **How:** Use functions, classes, and modules to encapsulate reusable logic. In Python and C++, this means abstracting repeated code into functions or classes.
* **Example:** If a calculation appears in multiple places, create a function for it instead of copying the code everywhere.

This violates the DRY principle since logic is repeated.

```python
def calculate_subject1(marks):
    total = sum(marks)
    average = total / len(marks)
    return average

def calculate_subject2(marks):
    total = sum(marks)
    average = total / len(marks)
    return average

```

Refactor to a single reusable function:

```python
def calculate_average(marks):
    if not marks:
        return 0
    return sum(marks) / len(marks)

subject1_avg = calculate_average([80, 90, 85])
subject2_avg = calculate_average([70, 75, 65])
```

***

### 2. KISS (Keep It Simple, Stupid) <a href="#id-2-kiss-keep-it-simple-stupid" id="id-2-kiss-keep-it-simple-stupid"></a>

* **Definition:** KISS encourages developers to keep code and design as simple as possible, avoiding unnecessary complexity.
* **Why:** Simple code is easier to read, maintain, and extend.
* **How:** Break down complex problems into smaller, manageable functions or classes. Avoid over-engineering and focus on clear, concise solutions.
* **Example:** Instead of writing a deeply nested function, split logic into smaller, focused functions.

***

### 3. YAGNI (You Aren't Gonna Need It) <a href="#id-3-yagni-you-arent-gonna-need-it" id="id-3-yagni-you-arent-gonna-need-it"></a>

* **Definition:** YAGNI advises against implementing features or functionality until they are actually needed.
* **Why:** Prevents code bloat and reduces unnecessary complexity, making the codebase easier to maintain.
* **How:** Focus on current requirements and avoid speculative additions. Add features only when there is a clear need.
* **Example:** Don’t add extra configuration options or extensibility hooks unless there is a real use case.

***

### 4. Modularity <a href="#id-4-modularity" id="id-4-modularity"></a>

* **Definition:** Modularity is the practice of dividing a software system into distinct, independent modules that encapsulate specific functionality.
* **Why:** Enhances code reusability, maintainability, and testability by isolating changes to specific modules.
* **How:** In Python and C++, use classes, functions, and separate files or namespaces to organize code into logical modules.
* **Example:** Separate user authentication, data processing, and UI logic into different modules or classes.

***

### 5. Abstraction <a href="#id-5-abstraction" id="id-5-abstraction"></a>

* **Definition:** Abstraction involves hiding complex implementation details and exposing only the necessary features of a component.
* **Why:** Simplifies usage and reduces dependencies between components, making the system easier to understand and modify.
* **How:** Use abstract classes, interfaces, or function signatures to define what a component does, not how it does it.
* **Example:** Define a `Database` interface with methods like `connect()` and `query()`and implement the details in subclasses.

***

### 6. Encapsulation <a href="#id-6-encapsulation" id="id-6-encapsulation"></a>

* **Definition:** Encapsulation is the bundling of data and methods that operate on that data within a single unit, typically a class, and restricting direct access to some of the object's components.
* **Why:** Protects the internal state of an object and enforces a clear interface for interaction.
* **How:** Use private/protected attributes and public methods in classes (e.g., `__private_var` in Python, `private:` in C++).
* **Example:** A `User` Class exposes methods to update user data, but keeps the actual data fields private.

***

### 7. Cohesion <a href="#id-7-cohesion" id="id-7-cohesion"></a>

* **Definition:** Cohesion refers to how closely related the responsibilities of a single module or class are.
* **Why:** High cohesion means a module or class has a well-defined purpose, making it easier to maintain and understand.
* **How:** Group related functions and data together. Avoid mixing unrelated responsibilities in the same module or class.
* **Example:** A `Logger` class should only handle logging, not user authentication.

***

### 8. Coupling <a href="#id-8-coupling" id="id-8-coupling"></a>

* **Definition:** Coupling is the degree of interdependence between software modules.
* **Why:** Low coupling makes modules more independent, so changes in one module have minimal impact on others.
* **How:** Use interfaces, dependency injection, and clear APIs to minimize dependencies between modules.
* **Example:** A payment module should interact with an order module through a well-defined interface, not by accessing its internal data directly.

***

### Summary Table <a href="#summary-table" id="summary-table"></a>

| Principle     | Key Idea                                 | Why It Matters                     | Example (Python/C++)                 |
| ------------- | ---------------------------------------- | ---------------------------------- | ------------------------------------ |
| DRY           | Avoid code duplication                   | Easier maintenance, fewer bugs     | Use functions for repeated logic     |
| KISS          | Keep code simple                         | Easier to read, maintain, extend   | Small, focused functions             |
| YAGNI         | Don’t add unneeded features              | Prevents code bloat                | Only implement current requirements  |
| Modularity    | Divide code into independent modules     | Reusability, testability           | Separate classes for each feature    |
| Abstraction   | Hide details, expose essentials          | Simplifies usage, reduces coupling | Abstract classes/interfaces          |
| Encapsulation | Bundle data and methods, restrict access | Protects state, enforces interface | Private attributes in classes        |
| Cohesion      | Group related responsibilities           | Easier to maintain and understand  | Single-purpose classes               |
| Coupling      | Minimize inter-module dependencies       | Easier to change and test          | Use interfaces, dependency injection |

***

