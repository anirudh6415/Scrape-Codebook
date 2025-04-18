# Decorators

### Decorators

| **Decorator Type**           | **Explanation**                                            | **Use Case**                                   | **Pros**                                                                          | **Cons**                                              |
| ---------------------------- | ---------------------------------------------------------- | ---------------------------------------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------- |
| `@function_decorator`        | Wraps and modifies a function’s behavior                   | Logging, validation, caching                   | <p>1. Cleaner logic reuse<br>2. Keeps functions modular</p>                       | 1. Harder to debug deeply nested decorators           |
| `@staticmethod`              | Method bound to the class, not to an instance              | Math utilities, class-level helper functions   | <p>1. No self required<br>2. Groups functionality logically</p>                   | 1. Can't access or modify class or instance variables |
| `@classmethod`               | Method bound to the class; receives `cls` as the first arg | Factory methods, alternative constructors      | <p>1. Access to class-level data<br>2. Used for class-wide state logic</p>        | 1. No access to instance (`self`)                     |
| `@property`                  | Getter method turned into an attribute                     | Accessing computed values like `area`, `price` | <p>1. Clean attribute-like access<br>2. Useful for read-only fields</p>           | 1. Can't accept arguments                             |
| `@functools.wraps`           | Preserves original metadata of decorated function          | Any custom decorator                           | <p>1. Retains docstrings &#x26; names<br>2. Essential for clean introspection</p> | 1. Requires `functools` import                        |
| `@contextlib.contextmanager` | Allows creating context managers using `yield`             | Managing resources (e.g., open files)          | <p>1. Cleaner resource handling<br>2. Less boilerplate</p>                        | 1. Needs `try/finally` for robustness                 |
| `@dataclass`                 | Auto-generates methods like `__init__`, `__repr__`         | Data container classes                         | <p>1. Saves time &#x26; code<br>2. Improves readability</p>                       | 1. Not suitable for logic-heavy classes               |
| `@lru_cache`                 | Caches return values of a function with given arguments    | Expensive or recursive functions               | <p>1. Major performance boost<br>2. Easy to use</p>                               | 1. Only works with hashable inputs                    |
| `@cached_property`           | Caches a property’s value after first call                 | Expensive one-time computations                | <p>1. Memory-efficient<br>2. Used like a property</p>                             | 1. Python 3.8+ only                                   |
| `@singledispatch`            | Function overloading based on argument type                | Type-specific operations                       | 1. Cleaner alternative to if-else type checks                                     | 1. Only dispatches on the first argument              |
| `@typechecked` (via library) | Validates input/output types at runtime                    | Type safety                                    | <p>1. Prevents silent type bugs<br>2. Useful in production</p>                    | 1. Extra dependency                                   |
| **Custom Decorator**         | User-defined decorator for reusable logic                  | Auth checks, time logging, input validation    | <p>1. Flexible &#x26; reusable<br>2. Encapsulates logic</p>                       | 1. Slightly more verbose                              |
| **Chained Decorators**       | Applying multiple decorators to one function               | Logging + caching + validation                 | <p>1. Clean modular layers<br>2. Easy to combine behaviors</p>                    | 1. Order-sensitive                                    |
| **Class Decorator**          | Affects class-level behavior (entire class wrapped)        | Registering models, singleton pattern          | <p>1. Dynamic class transformation<br>2. No metaclass needed</p>                  | 1. Can confuse code readers                           |

***

### Comparison: `@staticmethod` vs `@classmethod` vs Regular Method

| Feature          | `@staticmethod`   | `@classmethod`                         | Regular Method               |
| ---------------- | ----------------- | -------------------------------------- | ---------------------------- |
| Access to `self` | ❌                 | ❌                                      | ✅                            |
| Access to `cls`  | ❌                 | ✅                                      | ❌                            |
| Usage            | Utility functions | Class-wide operations, factory methods | Instance-specific logic      |
| Example Use      | Math utils        | From-config constructor                | Changing object’s attributes |

***

