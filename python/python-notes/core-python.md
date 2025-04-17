# Core Python

### 🔢 Data Types

<table><thead><tr><th width="93.14959716796875">Data Type</th><th width="113.8016357421875">Description</th><th width="111.86553955078125">Pros</th><th width="119.610107421875">Example (Pro)</th><th width="110.57476806640625">Cons</th><th>Example (Con)</th></tr></thead><tbody><tr><td><code>int</code></td><td>Integer numbers (e.g., <code>1</code>, <code>42</code>, <code>-5</code>)</td><td>Fast operations</td><td>Arithmetic like <code>1000 * 1000</code> runs quickly</td><td>Limited by memory</td><td>Very large ints use more memory: <code>10**1000</code></td></tr><tr><td><code>float</code></td><td>Decimal numbers (e.g., <code>3.14</code>, <code>-0.01</code>)</td><td>Precise for many cases</td><td><code>0.1 + 0.2</code> appears fine in simple math</td><td>Rounding/precision issues</td><td><code>0.1 + 0.2 == 0.3</code> → <code>False</code> due to binary float representation</td></tr><tr><td><code>str</code></td><td>Text data (<code>"hello"</code>, <code>'world'</code>)</td><td>Rich methods, Unicode support</td><td><code>"abc".upper()</code> → <code>'ABC'</code></td><td>Immutable</td><td><code>"abc"[0] = 'x'</code> → Error</td></tr><tr><td><code>bool</code></td><td>Logical values (<code>True</code>, <code>False</code>)</td><td>Clear for control flow</td><td>Used in <code>if</code>, <code>while</code> for readability</td><td>Only two values</td><td>Can't represent uncertainty beyond <code>True</code>/<code>False</code></td></tr><tr><td><code>list</code></td><td>Ordered, mutable sequence (<code>[1, 2, 3]</code>)</td><td>Flexible, powerful methods</td><td><code>append</code>, <code>pop</code>, slicing</td><td>Slower lookup than dict/set</td><td><code>5 in large_list</code> is O(n)</td></tr><tr><td><code>tuple</code></td><td>Ordered, immutable sequence (<code>(1, 2, 3)</code>)</td><td>Safer (can't be modified)</td><td>Useful as dict keys: <code>{(1,2): 'a'}</code></td><td>Immutable</td><td>Can't <code>append</code> or <code>remove</code></td></tr><tr><td><code>set</code></td><td>Unordered unique items (<code>{1, 2, 3}</code>)</td><td>Fast membership testing</td><td><code>x in my_set</code> is O(1)</td><td>No indexing/order</td><td><code>my_set[0]</code> → Error</td></tr><tr><td><code>dict</code></td><td>Key-value store (<code>{'a': 1}</code>)</td><td>Fast key-based lookup</td><td><code>my_dict['name']</code> is fast</td><td>Keys must be unique</td><td>Duplicate keys overwrite: <code>{'a': 1, 'a': 2}</code> → <code>{'a': 2}</code></td></tr></tbody></table>

***

### 🔄 Type Casting (Type Conversion)

<table><thead><tr><th width="98.9578857421875">Type Cast</th><th width="109.929443359375">Description</th><th width="110.57476806640625">Pros</th><th width="128.6453857421875">Example (Pro)</th><th width="112.51092529296875">Cons</th><th>Example (Con)</th></tr></thead><tbody><tr><td><code>int()</code></td><td>Converts to integer</td><td>Removes decimal points</td><td><code>int(3.9)</code> → <code>3</code></td><td>Loses precision</td><td><code>int(9.99)</code> → <code>9</code></td></tr><tr><td><code>float()</code></td><td>Converts to float</td><td>Adds precision</td><td><code>float(5)</code> → <code>5.0</code></td><td>Can lead to rounding issues</td><td><code>float("3.14") + 1e-16</code> may not be <code>3.1400000000000001</code></td></tr><tr><td><code>str()</code></td><td>Converts to string</td><td>Useful for printing/logs</td><td><code>str(123)</code> → <code>'123'</code></td><td>Not usable in math ops</td><td><code>'10' + 5</code> → TypeError</td></tr><tr><td><code>bool()</code></td><td>Converts to Boolean</td><td>Helps in logical filtering</td><td><code>bool([])</code> → <code>False</code></td><td>Can be confusing</td><td><code>bool("False")</code> → <code>True</code></td></tr><tr><td><code>list()</code></td><td>Converts to list</td><td>Good for iterables</td><td><code>list("abc")</code> → <code>['a', 'b', 'c']</code></td><td>Can create large lists</td><td><code>list(range(1_000_000))</code> → High memory usage</td></tr><tr><td><code>set()</code></td><td>Converts to set</td><td>Removes duplicates</td><td><code>set([1,2,2,3])</code> → <code>{1,2,3}</code></td><td>Unordered result</td><td>No guarantee of element order</td></tr><tr><td><code>tuple()</code></td><td>Converts to tuple</td><td>Immutable, hashable</td><td><code>tuple([1,2,3])</code> → <code>(1,2,3)</code></td><td>Can't modify</td><td><code>my_tuple[0] = 9</code> → Error</td></tr></tbody></table>

***

### ➕ Operators

#### 🧮 Arithmetic Operators

<table><thead><tr><th width="104.52606201171875">Operator</th><th width="157.50421142578125">Description</th><th width="150.33447265625">Example</th><th>Notes</th></tr></thead><tbody><tr><td><code>+</code></td><td>Addition</td><td><code>2 + 3</code> → <code>5</code></td><td>Works on numbers and strings (<code>'a' + 'b'</code>)</td></tr><tr><td><code>-</code></td><td>Subtraction</td><td><code>5 - 2</code> → <code>3</code></td><td></td></tr><tr><td><code>*</code></td><td>Multiplication</td><td><code>3 * 4</code> → <code>12</code></td><td>Also works for string repetition: <code>'a' * 3</code> → <code>'aaa'</code></td></tr><tr><td><code>/</code></td><td>Division</td><td><code>7 / 2</code> → <code>3.5</code></td><td>Always returns float</td></tr><tr><td><code>//</code></td><td>Floor Division</td><td><code>7 // 2</code> → <code>3</code></td><td>Rounds down</td></tr><tr><td><code>%</code></td><td>Modulus</td><td><code>7 % 2</code> → <code>1</code></td><td>Remainder</td></tr><tr><td><code>**</code></td><td>Exponentiation</td><td><code>2 ** 3</code> → <code>8</code></td><td>Power of a number</td></tr></tbody></table>

***

#### 🔍 Comparison

<table><thead><tr><th width="124.739501953125">Operator</th><th width="259.9932861328125">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>==</code></td><td>Equal</td><td><code>5 == 5</code> → <code>True</code></td></tr><tr><td><code>!=</code></td><td>Not Equal</td><td><code>5 != 3</code> → <code>True</code></td></tr><tr><td><code>></code></td><td>Greater than</td><td><code>5 > 3</code> → <code>True</code></td></tr><tr><td><code>&#x3C;</code></td><td>Less than</td><td><code>3 &#x3C; 5</code> → <code>True</code></td></tr><tr><td><code>>=</code></td><td>Greater or equal</td><td><code>5 >= 5</code> → <code>True</code></td></tr><tr><td><code>&#x3C;=</code></td><td>Less or equal</td><td><code>4 &#x3C;= 5</code> → <code>True</code></td></tr></tbody></table>

***

#### 🔗 Logical

| Operator | Description                  | Example                  |
| -------- | ---------------------------- | ------------------------ |
| `and`    | True if both are true        | `True and True` → `True` |
| `or`     | True if at least one is true | `True or False` → `True` |
| `not`    | Negation                     | `not True` → `False`     |

***

#### 📦 Assignment

| Operator | Description          | Example      | Notes                                            |
| -------- | -------------------- | ------------ | ------------------------------------------------ |
| `is`     | Same object identity | `a is b`     | Compares memory addresses                        |
| `is not` | Not same identity    | `a is not b` | Use with caution for numbers/strings (interning) |

***

#### 🧪 Identity & Membership

| Operator | Description                          | Example                     |
| -------- | ------------------------------------ | --------------------------- |
| `in`     | Checks if element is in sequence     | `'a' in 'cat'` → `True`     |
| `not in` | Checks if element is NOT in sequence | `5 not in [1,2,3]` → `True` |
