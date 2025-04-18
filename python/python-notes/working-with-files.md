# Working with Files

### File Handling Concepts Table

<table><thead><tr><th width="47.292449951171875">SI</th><th width="136.4755859375">Section</th><th>Usage Scenario</th><th>Pros</th><th>Cons</th></tr></thead><tbody><tr><td>1</td><td><code>with open()</code> for file handling</td><td>Reading/writing files safely, ensuring proper closure</td><td><ol><li>Automatically closes files</li><li>Cleaner code</li><li>Less prone to errors</li></ol></td><td><ol><li>Limited to scope of <code>with</code> block</li></ol></td></tr><tr><td>2</td><td>File Modes (<code>'r'</code>, <code>'w'</code>, <code>'a'</code>)</td><td>Choose based on whether you're reading, writing, or appending</td><td><ol><li>Flexible modes</li><li>Supports text and binary modes</li><li><code>'r+'</code> allows read/write</li></ol></td><td><ol><li><code>'w'</code> can overwrite data</li><li>Wrong mode may raise error</li></ol></td></tr><tr><td>3</td><td>Reading large files efficiently</td><td>Log files, datasets – when full file can’t be loaded into memory</td><td><ol><li>Memory efficient</li><li>Great for huge files</li></ol></td><td><ol><li>Slightly more logic needed</li></ol></td></tr><tr><td>4</td><td>File Reading Techniques</td><td>Different ways of reading based on need (whole, lines, line by line)</td><td><ol><li>Multiple options for use cases</li><li><code>readlines()</code> useful for processing all at once</li></ol></td><td><ol><li><code>read()</code> and <code>readlines()</code> can be memory-heavy for big files</li></ol></td></tr><tr><td>5</td><td>Context Managers</td><td>Safe file and resource handling using <code>with</code></td><td><ol><li>Auto resource management</li><li>Prevents file leaks</li><li>Readable and Pythonic</li></ol></td><td><ol><li>Can't use file object after exiting <code>with</code> block</li></ol></td></tr></tbody></table>

***

### Code Sample

{% tabs %}
{% tab title="with open()" %}
```python
with open('example.txt', 'r') as file:
    data = file.read()
    print(data)
```
{% endtab %}

{% tab title="Read or write" %}
```python
# Reading a file
with open('example.txt', 'r') as file:
    content = file.read()

# Writing to a file
with open('output.txt', 'w') as file:
    file.write("Hello, World!")
```
{% endtab %}

{% tab title="Reading Large Files Efficiently (Line by Line)" %}
```python
with open('large_file.txt', 'r') as file:
    for line in file:
        print(line.strip())
```
{% endtab %}

{% tab title=" File Reading Techniques" %}
```python
# Read the entire file
with open('example.txt', 'r') as file:
    content = file.read()

# Read all lines into a list
with open('example.txt', 'r') as file:
    lines = file.readlines()

# Read one line
with open('example.txt', 'r') as file:
    line = file.readline()
```
{% endtab %}

{% tab title="Context Manager" %}
```python
with open('example.txt', 'r') as file:
    content = file.read()
# File automatically closed after block
```
{% endtab %}
{% endtabs %}
