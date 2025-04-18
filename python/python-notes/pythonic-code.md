# Pythonic Code

### Pythonic Code

<table><thead><tr><th width="48.618499755859375">SI</th><th width="146.07061767578125">Concept</th><th>Symbol / Function</th><th>Description</th><th>Usage Scenario</th></tr></thead><tbody><tr><td>1</td><td>One-liner</td><td><code>;</code> or compressed logic</td><td>Single line expressions or statements.</td><td>Quick scripts or clean outputs.</td></tr><tr><td>2</td><td>List Comprehension</td><td><code>[x for x in ...]</code></td><td>Create lists with filter/map logic.</td><td>Filtering, mapping.</td></tr><tr><td>3</td><td><code>any()</code> / <code>all()</code></td><td><code>any()</code>, <code>all()</code></td><td>Check boolean condition on iterable.</td><td>Validation checks.</td></tr><tr><td>4</td><td><code>sorted()</code> with <code>lambda</code></td><td><code>sorted(..., key=lambda)</code></td><td>Custom sorting logic.</td><td>Sorting complex data.</td></tr><tr><td>5</td><td>Ternary Operator</td><td><code>x if cond else y</code></td><td>Conditional assignment.</td><td>Short if-else logic.</td></tr><tr><td>6</td><td><code>zip()</code></td><td><code>zip(a, b)</code></td><td>Loop over multiple iterables.</td><td>Parallel iteration.</td></tr><tr><td>7</td><td><code>enumerate()</code></td><td><code>enumerate(list)</code></td><td>Index + value in loops.</td><td>Indexed looping.</td></tr><tr><td>8</td><td>Unpacking</td><td><code>a, b = [1, 2]</code></td><td>Assign multiple variables at once.</td><td>Working with tuples/lists.</td></tr><tr><td>9</td><td>Set &#x26; Dict Comprehension</td><td><code>{x for x in ...}</code> / <code>{k:v}</code></td><td>Similar to list comprehension but for sets/dicts.</td><td>Create unique elements or filtered dicts.</td></tr><tr><td>10</td><td><code>dict.get()</code> / <code>setdefault()</code></td><td><code>d.get(k)</code> / <code>d.setdefault()</code></td><td>Avoid <code>KeyError</code>, provide default values.</td><td>Cleaner dictionary access.</td></tr><tr><td>11</td><td>Context Managers with <code>with</code></td><td><code>with open(...) as ...:</code></td><td>Automatically handles file/resource cleanup.</td><td>File handling, locks, DB connections.</td></tr></tbody></table>

***

### Code Samples

{% tabs %}
{% tab title="One-liner" %}
```python
result = sum([i for i in range(10)])  # returns 45
```
{% endtab %}

{% tab title="List Comprehension" %}
```python
squares = [x**2 for x in range(10)]
```
{% endtab %}

{% tab title="any() / all()" %}
```python
nums = [0, 1, 2]
print(any(nums))  # True
print(all(nums))  # False
```
{% endtab %}

{% tab title="sorted() with lambda" %}
```python
data = [{'name': 'John', 'age': 25}, {'name': 'Jane', 'age': 20}]
print(sorted(data, key=lambda x: x['age']))
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="zip()" %}
```python
names = ['a', 'b']
marks = [90, 80]
for name, mark in zip(names, marks):
    print(f"{name} scored {mark}")
```
{% endtab %}

{% tab title="Second enumerate()" %}
```python
fruits = ['apple', 'banana']
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")
```
{% endtab %}

{% tab title="Set & Dict Comprehensions" %}
```python
unique = {x for x in [1, 1, 2, 3]}
squared = {x: x**2 for x in range(5)}
```
{% endtab %}

{% tab title="dict.get() / setdefault()" %}
```python
data = {"a": 1}
print(data.get("b", 0))  # 0
data.setdefault("c", 5)
print(data)  # {'a': 1, 'c': 5}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Context Manager (with)" %}
```python
with open("file.txt", "r") as f:
    for line in f:
        print(line.strip())

```
{% endtab %}

{% tab title="Ternary Operator" %}
```
score = 85
status = "Pass" if score >= 50 else "Fail"

```
{% endtab %}
{% endtabs %}
