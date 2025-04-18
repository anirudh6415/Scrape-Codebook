---
description: >-
  Concurrency allows you to handle multiple tasks at once, which is crucial for
  improving efficiency and responsiveness in programs. Python provides various
  models for concurrency such as Threading, Mul
---

# Concurrency

### Concurrency Methods

<table><thead><tr><th width="147.3260498046875">Feature</th><th width="143.51763916015625">Description</th><th>Use Case</th><th>Pros</th><th>Cons</th></tr></thead><tbody><tr><td><strong>Threading</strong></td><td>Runs multiple threads in the same memory space.</td><td>I/O-bound tasks like network calls, file reading.</td><td>1. Lightweight<br>2. Simple syntax</td><td>1. Blocked by GIL<br>2. Not ideal for CPU-heavy tasks</td></tr><tr><td><strong>GIL (Global Interpreter Lock)</strong></td><td>A lock that prevents multiple threads from executing bytecodes simultaneously.</td><td>Restricts Python's multi-threading to one thread at a time.</td><td>1. Simplifies memory management</td><td>1. Limits CPU-bound multi-threading</td></tr><tr><td><strong>Multiprocessing</strong></td><td>Launches separate processes, each with its own memory space.</td><td>CPU-bound tasks such as data crunching, video rendering.</td><td>1. True parallelism<br>2. Bypasses GIL</td><td>1. Higher memory use<br>2. More overhead due to inter-process communication</td></tr><tr><td><strong>AsyncIO</strong></td><td>Cooperative multitasking using <code>async</code> and <code>await</code>.</td><td>Efficient for I/O-heavy operations like socket or web communication.</td><td>1. Scalable<br>2. Low memory footprint</td><td>1. Learning curve<br>2. Not for CPU-heavy tasks</td></tr><tr><td><strong>ThreadPoolExecutor</strong></td><td>High-level interface for threading via <code>concurrent.futures</code>.</td><td>Parallelize simple I/O tasks.</td><td>1. Easy to use<br>2. Manages threads efficiently</td><td>1. GIL still applies</td></tr><tr><td><strong>ProcessPoolExecutor</strong></td><td>High-level interface for multiprocessing.</td><td>Parallelize CPU-heavy computations.</td><td>1. Simple API<br>2. Avoids GIL</td><td>1. Slower startup<br>2. High memory usage</td></tr><tr><td><strong>Thread-safe Queue</strong></td><td>Queue class from <code>queue</code> module, useful for thread/process communication.</td><td>Sharing data between workers.</td><td>1. Safe and reliable data exchange</td><td>1. Slight overhead</td></tr><tr><td><strong>aiohttp</strong></td><td>Async HTTP client for asyncio-based applications.</td><td>High-performance async web scraping and APIs.</td><td>1. Built-in connection pooling<br>2. Easy coroutine integration</td><td>1. Async-only, requires different architecture</td></tr></tbody></table>

***

### Comparison: AsyncIO vs Threading vs Multiprocessing

| Feature       | AsyncIO                      | Threading                           | Multiprocessing             |
| ------------- | ---------------------------- | ----------------------------------- | --------------------------- |
| Best For      | I/O-bound & high concurrency | I/O-bound tasks                     | CPU-bound heavy computation |
| Memory Usage  | Low                          | Medium                              | High                        |
| GIL Affected? | No                           | Yes                                 | No                          |
| Parallelism   | Cooperative (non-blocking)   | Pseudo-parallelism (blocked by GIL) | True parallelism            |
| Complexity    | Medium to High               | Low                                 | Medium                      |

***

### Code Samples&#x20;

{% tabs %}
{% tab title="Threading" %}
```python
import threading
import time

def download_file(file_id):
    print(f"Downloading file {file_id}...")
    time.sleep(2)
    print(f"Finished downloading file {file_id}")

threads = []
for i in range(3):
    t = threading.Thread(target=download_file, args=(i,))
    threads.append(t)
    t.start()

for t in threads:
    t.join()

# ===== Output ==========
"""
Downloading file 0...
Downloading file 1...
Downloading file 2...
Finished downloading file 0
Finished downloading file 1
Finished downloading file 2
"""
```
{% endtab %}

{% tab title="Multiprocessing" %}
{% code overflow="wrap" %}
```python
from multiprocessing import Process
import time

def compute_square(n):
    print(f"Computing square of {n}")
    time.sleep(1)
    print(f"Square of {n} is {n * n}")

if __name__ == '__main__':
    processes = []
    for i in range(3):
        p = Process(target=compute_square, args=(i,))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()

# Output
"""        
Computing square of 0
Computing square of 1
Computing square of 2
Square of 0 is 0
Square of 1 is 1
Square of 2 is 4
"""
```
{% endcode %}
{% endtab %}

{% tab title="GIL" %}
```python
import threading
import time

def cpu_task():
    count = 0
    for _ in range(10**7):
        count += 1

start = time.time()
threads = [threading.Thread(target=cpu_task) for _ in range(4)]
for t in threads: t.start()
for t in threads: t.join()
end = time.time()

print(f"Threaded CPU task took {end - start:.2f} seconds")

# output
""" 
Threaded CPU task took 4.23 seconds
"""
```
{% endtab %}

{% tab title="AsyncIO" %}
```python
import asyncio

async def async_task(name):
    print(f"Start {name}")
    await asyncio.sleep(1)
    print(f"End {name}")

async def main():
    await asyncio.gather(
        async_task("Task1"),
        async_task("Task2"),
        async_task("Task3")
    )

asyncio.run(main())
# Output
""" 
Start Task1
Start Task2
Start Task3
End Task1
End Task2
End Task3
"""
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="ThreadPoolExecutor" %}
```python
from concurrent.futures import ThreadPoolExecutor

def greet(name):
    return f"Hello, {name}!"

names = ["Alice", "Bob", "Charlie"]

with ThreadPoolExecutor() as executor:
    results = executor.map(greet, names)
    for result in results:
        print(result)

# output
"""
Hello, Alice!
Hello, Bob!
Hello, Charlie!
"""
```
{% endtab %}

{% tab title="ProcessPoolExecutor" %}
```python
from concurrent.futures import ProcessPoolExecutor

def factorial(n):
    result = 1
    for i in range(2, n + 1):
        result *= i
    return f"{n}! = {result}"

numbers = [3, 4, 5]

with ProcessPoolExecutor() as executor:
    results = executor.map(factorial, numbers)
    for res in results:
        print(res)
        
# output
"""
3! = 6
4! = 24
5! = 120

"""
```
{% endtab %}

{% tab title="Thread-safe Queue" %}
```python
from queue import Queue
import threading

def producer(q):
    for i in range(5):
        q.put(i)
        print(f"Produced {i}")

def consumer(q):
    while not q.empty():
        item = q.get()
        print(f"Consumed {item}")

q = Queue()
producer_thread = threading.Thread(target=producer, args=(q,))
consumer_thread = threading.Thread(target=consumer, args=(q,))

producer_thread.start()
producer_thread.join()

consumer_thread.start()
consumer_thread.join()

# Ouput 
"""
Produced 0
Produced 1
Produced 2
Produced 3
Produced 4
Consumed 0
Consumed 1
Consumed 2
Consumed 3
Consumed 4

"""

```
{% endtab %}
{% endtabs %}
