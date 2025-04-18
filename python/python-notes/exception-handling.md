# Exception Handling

### Exception Handling

<table><thead><tr><th width="53.746246337890625">SI</th><th width="114.17816162109375">Section</th><th>Usage Scenarios</th><th>Pros</th><th valign="top">Cons</th></tr></thead><tbody><tr><td>1</td><td><strong>try-except-finally</strong></td><td>Handling errors in code where an exception might occur, but you want to continue execution.</td><td><ol><li>Handles exceptions gracefully, keeps the program running after catching error.</li><li>Ensures resources are freed, files are closed, and cleanup actions are taken.</li></ol></td><td valign="top"><ol><li>Can hide bugs if used excessively or incorrectly.</li><li>It can make code less readable if not used carefully.</li></ol></td></tr><tr><td>2</td><td><strong>Raising Exceptions (raise)</strong></td><td>Explicitly raise exceptions when a condition is not met, for better control over error handling.</td><td><ol><li>Customizable error handling, useful for catching specific exceptions in context.</li><li>Improves code clarity when handling specific conditions.</li></ol></td><td valign="top"><ol><li>Overuse may lead to excessive custom exceptions that clutter the code.</li><li>Can make code harder to follow if exceptions are raised frequently.</li></ol></td></tr><tr><td>3</td><td><strong>Custom Exceptions</strong></td><td>Create custom exceptions to better represent specific errors in your program.</td><td>Allows for more descriptive error messages and clearer error handling.</td><td valign="top">Can complicate code unnecessarily if simple exceptions suffice.</td></tr><tr><td></td><td></td><td>Useful for domain-specific errors that built-in exceptions don't cover.</td><td>Makes debugging and error handling more intuitive.</td><td valign="top">Overuse can lead to excessive custom exception classes.</td></tr><tr><td>4</td><td><strong>Assertions</strong></td><td>Verifying conditions during development to catch bugs early by ensuring a condition is true.</td><td>Helps catch bugs early, useful during development.</td><td valign="top">May cause performance issues if assertions are used excessively in production.</td></tr><tr><td></td><td></td><td>Ensures that certain conditions hold true, which can prevent logical errors.</td><td>Improves code quality by validating conditions before proceeding.</td><td valign="top">Can be disabled globally with the <code>-O</code> flag, rendering them useless in production.</td></tr><tr><td>5</td><td><strong>try-except with Multiple Exceptions</strong></td><td>Catch multiple different types of exceptions in a single block.</td><td>Can catch multiple exceptions in one place, providing cleaner error handling.</td><td valign="top">If not careful, may catch unrelated exceptions, leading to difficult debugging.</td></tr></tbody></table>

***

### Code examples

{% tabs fullWidth="true" %}
{% tab title="try-except-finally" %}
```python
try:
    x = 1 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
finally:
    print("Finally block executed")
```
{% endtab %}

{% tab title="Raising Exceptions (raise)" %}
```python
def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```
{% endtab %}

{% tab title="Custom Exceptions" %}
```python
class NegativeValueError(Exception):
    pass

def set_age(age):
    if age < 0:
        raise NegativeValueError("Age cannot be negative")
    return age
```
{% endtab %}

{% tab title="Assertions" %}
```python
assert x > 0, "x must be positive"
```
{% endtab %}

{% tab title="try-except with Multiple Exceptions" %}
```python
try:
    x = 1 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
except TypeError:
    print("Invalid data type")

```
{% endtab %}
{% endtabs %}

###
