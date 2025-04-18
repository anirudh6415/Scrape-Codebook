# Data Structures

### Python Data Structures Overview

<table><thead><tr><th width="50.554595947265625">SI</th><th width="116.44708251953125">D</th><th width="114.44708251953125">Description</th><th>Usage Scenario</th><th>Pros</th><th width="116.93447875976562">Cons</th></tr></thead><tbody><tr><td>1</td><td><strong>List <code>[]</code></strong></td><td>Ordered, mutable, allows duplicates.</td><td>Storing and modifying a sequence of items.</td><td>1. Dynamic sizing<br>2. Built-in methods<br>3. Easy indexing</td><td>1. Slower for search operations</td></tr><tr><td>2</td><td><strong>Tuple <code>()</code></strong></td><td>Ordered, immutable, allows duplicates.</td><td>Returning multiple values from a function.</td><td>1. Memory-efficient<br>2. Safe from unintended changes</td><td>1. Cannot modify contents</td></tr><tr><td>3</td><td><strong>Set <code>{}</code> / <code>set()</code></strong></td><td>Unordered, mutable, no duplicates.</td><td>Membership testing, removing duplicates.</td><td>1. Fast lookup<br>2. Ensures uniqueness</td><td>1. No indexing</td></tr><tr><td>4</td><td><strong>Dictionary <code>{key: val}</code></strong></td><td>Key-value pairs, unordered in &#x3C;3.6, ordered from 3.7+.</td><td>Lookups by key, JSON-like data.</td><td>1. Fast key access<br>2. Flexible structure</td><td>1. Keys must be hashable</td></tr><tr><td>5</td><td><strong>Stack <code>[list]</code></strong></td><td>LIFO, often implemented using lists.</td><td>Undo functionality, parsing.</td><td>1. Simple to implement with lists<br>2. Built-in methods like <code>append</code>, <code>pop</code></td><td>1. Manual enforcement of LIFO</td></tr><tr><td>6</td><td><strong>Queue <code>deque([])</code></strong></td><td>FIFO, implemented using <code>collections.deque</code>.</td><td>Scheduling tasks, producer-consumer.</td><td>1. Fast appends and pops from both ends</td><td>1. Not built-in as list</td></tr><tr><td>7</td><td><strong>namedtuple <code>namedtuple()</code></strong></td><td>Immutable object with named fields.</td><td>Replacing simple classes.</td><td>1. Improves readability<br>2. Lightweight and immutable</td><td>1. Cannot modify fields</td></tr><tr><td>8</td><td><strong>Counter <code>Counter()</code></strong></td><td>Dict subclass for counting hashable items.</td><td>Word frequency, histogram data.</td><td>1. Fast frequency count<br>2. Built-in operations like <code>most_common()</code></td><td>1. Not suitable for unhashables</td></tr><tr><td>9</td><td><strong>defaultdict <code>defaultdict()</code></strong></td><td>Dict subclass with a default factory for missing keys.</td><td>Grouping values, building indexes.</td><td>1. Avoids <code>KeyError</code><br>2. Cleaner code</td><td>1. Slight overhead due to factory</td></tr></tbody></table>

***

### Code sample

{% tabs %}
{% tab title="List" %}
```python
my_list = [1, 2, 3]
my_list.append(4)
print(my_list)  # [1, 2, 3, 4]

```
{% endtab %}

{% tab title="Tuple" %}
```python
my_tuple = (1, 2, 3)
print(my_tuple[1])  # 2

# my_tuple[1] = 5  # ❌ Error: 'tuple' object does not support item assignment

```
{% endtab %}

{% tab title="Set" %}
```python
my_set = {1, 2, 3, 3}
my_set.add(4)
print(my_set)  # {1, 2, 3, 4}

```
{% endtab %}

{% tab title="Dictionary" %}
```python
my_dict = {"name": "Anirudh", "age": 25}
print(my_dict["name"])  # Anirudh

```
{% endtab %}

{% tab title="Stack " %}
```python
stack = []
stack.append(1)
stack.append(2)
print(stack.pop())  # 2

```
{% endtab %}

{% tab title="Queue" %}
```python
from collections import deque

queue = deque()
queue.append(1)
queue.append(2)
print(queue.popleft())  # 1

```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="namedtuple" %}
```python
from collections import namedtuple

Point = namedtuple("Point", "x y")
pt = Point(1, 2)
print(pt.x, pt.y)  # 1 2

```
{% endtab %}

{% tab title="Counter" %}
```python
from collections import Counter

c = Counter("anirudh")
print(c)  # Counter({'a': 1, 'n': 1, 'i': 1, 'r': 1, 'u': 1, 'd': 1, 'h': 1})

```
{% endtab %}

{% tab title="defaultdict" %}
```python
from collections import defaultdict

dd = defaultdict(int)
dd["apple"] += 1
print(dd["apple"])  # 1
print(dd["banana"])  # 0 (no error!)

```
{% endtab %}
{% endtabs %}
