# Scripting Skills

### Features

<table><thead><tr><th width="189.92266845703125">Feature</th><th>Description</th><th>Use Case</th></tr></thead><tbody><tr><td><strong>CLI with <code>argparse</code></strong></td><td>Command-line argument parsing for building robust CLI tools.</td><td>Building user-facing scripts (e.g., <code>python greet.py John</code>)</td></tr><tr><td><strong><code>os</code> module</strong></td><td>Interacting with OS: path manipulation, directories, file operations.</td><td>Automating file/folder operations like creating backups or organizing directories.</td></tr><tr><td><strong><code>subprocess</code> module</strong></td><td>Executes shell commands inside Python.</td><td>Automating command-line tasks, shell scripting, running bash or system utilities.</td></tr><tr><td><strong>Environment Variables</strong></td><td>Accessing or modifying system environment variables.</td><td>Loading secrets/configs dynamically, adapting script behavior based on environment.</td></tr><tr><td><strong><code>shutil</code> for file ops</strong></td><td>High-level file operations like copy, move, archive.</td><td>Backup, restore, zip/unzip automation, temporary file cleanup.</td></tr><tr><td><strong>Logging (<code>logging</code>)</strong></td><td>Replaces print with log levels (info, debug, error).</td><td>Debugging and monitoring script behavior in production or automation pipelines.</td></tr><tr><td><strong>Shebang Line</strong></td><td><code>#!/usr/bin/env python3</code> lets a script be run as an executable on Unix-like systems.</td><td>Run scripts directly like <code>./myscript.py</code> from terminal.</td></tr><tr><td><strong>Exit Codes (<code>sys.exit</code>)</strong></td><td>Return specific exit codes for success or failure in automation pipelines.</td><td>Integrating with CI/CD or cronjobs where exit codes signal job status.</td></tr><tr><td><strong><code>pathlib</code></strong></td><td>Modern, OOP way of handling file paths (better than <code>os.path</code>).</td><td>Cleaner syntax for cross-platform file manipulation.</td></tr><tr><td><strong>Cross-platform Compatibility</strong></td><td>Consider differences in OS, paths, and encoding.</td><td>Make scripts usable on both Windows and Linux/macOS.</td></tr></tbody></table>

***

### Code sample

{% tabs %}
{% tab title="Argparse" %}
```python
import argparse

parser = argparse.ArgumentParser(description="Greet someone.")
parser.add_argument("name", help="Name of the person")
args = parser.parse_args()

print(f"Hello, {args.name}!")
```
{% endtab %}

{% tab title="OS" %}
```python
import os

print("Current directory:", os.getcwd())
os.mkdir("new_folder")
print("Files:", os.listdir())
```
{% endtab %}

{% tab title="subprocess" %}
```python
import subprocess

result = subprocess.run(["echo", "Hello from subprocess"], capture_output=True, text=True)
print(result.stdout)
```
{% endtab %}

{% tab title="Env Variable" %}
```python
import os

print("PATH:", os.environ.get("PATH"))
os.environ["MY_VAR"] = "123"
print("MY_VAR:", os.environ["MY_VAR"])
```
{% endtab %}

{% tab title="shutil" %}
```python
import shutil

shutil.copy("example.txt", "backup_example.txt")
shutil.move("backup_example.txt", "new_location/backup_example.txt")
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Logging" %}
```python
import logging

logging.basicConfig(level=logging.INFO)
logging.info("This is an info message.")
logging.error("This is an error message.")
```
{% endtab %}

{% tab title="Shebang" %}
```python
#!/usr/bin/env python3

print("This script can be run directly in Unix-like systems!")
```
{% endtab %}

{% tab title="exit codes" %}
```python
import sys

if some_condition_failed:
    sys.exit("Exiting due to error...")
else:
    print("All good!")
```
{% endtab %}

{% tab title="pathlib" %}
```python
from pathlib import Path

p = Path("my_folder") / "file.txt"
print(p.name)      # file.txt
print(p.exists())  # True/False
```
{% endtab %}

{% tab title="Cross-platform" %}
```python
import os
import platform

if platform.system() == "Windows":
    os.system("dir")
else:
    os.system("ls -la")
```
{% endtab %}
{% endtabs %}
