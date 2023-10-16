[[2023-10-15]] #DistributedSystem #OperatingSystem

### Motivation
Previously, we've covered the idea of **[[11 MapReduce#^664ec2|distributed system]]**, including MapReduce (distributed system for compute) and GFS (distributed system for storage). In this chapter, we will be discussing how distributed systems are **implemented** using threads and processes for parallelization.

#### Sequential Program
Sequential programs are **NOT** parallel. 
- E.g. Flask development server handles **ONE** request at **A TIME**

```python
@app.route("/")
def index():
	context = # slow database read
	return flask.render_template(
		"index.html", **context
	)
```

For example, in this program, the process follows a strict sequence that
1. Client sends `GET` request to the server
2. Server starts reading from the database
3. Server waits for the database's response
4. Server renders the template and sends the response back to client with code `200`

This seems a little undesirable if we have a moderately slow database if we have a lot of wait time. Moreover, imagine the case for **concurrent requests**. 

![[Pasted image 20231015151914.png|400]]

In the example above, client 2 will have to wait for unreasonably long amount of time because the server is busy dealing with the request sent from client 1!
- If rather we could take advantage of the wait time and do something else

Sequential programs could be problematic when
- Multiple things happening at once (concurrent requests)
- Slow resource (slow database read)

Essentially, such program needs to spend a lot of time **waiting**. Hence, the solution is, naturally, to work on both requests the **SAME** time.

---
### Processes
In simple language, a process is a running program.
- Much like what we would see within the task manager of computer

More formally, the Process is an **OS abstraction for execution**. A process is a program in execution. Programs are **static entities** with potential for execution; once the program is started, it becomes a process.

A process is consisted of
- A unique process ID (PID)
- An address space (memory)
- 1 or more threads (sequences of computation)
- Some other resources (file handles, open sockets, etc.)

Each process gets its own address space, and it can **ONLY** write to its own address space.

![[Pasted image 20231015153217.png|300]]

Our operating system manages process scheduling.
- Each CPU core can run one process at a time
- Every ~1-10 ms, the OS can switch

![[Pasted image 20231015152714.png|400]]

On a single-core machine, processes **take turn**. On a multi-core machine, processes run in **parallel**.
- Processes still take turns because there are usually many **more processes than cores**

```ad-summary
When are processes **useful**?
- Multiple things happening at once
- Different programs running on the same machine
- Same program running on different machines

When are processes **NOT useful**?
- Programs where multiple processes lead to **higher memory overhead** or have homogeneous code
```

---
### Threads
Threads are multiple **functions** in **one process** running **simultaneously**.
- Share heap, static data, code
- Lower overhead than processes

![[Pasted image 20231015153238.png|300]]

Let's demonstrate the concept using a simple example. Assume the following code,

```python
from threading import Thread

def hello():
	print("hello world")

t = Thread(target=hello)
t.start()
t.join()
```

![[Pasted image 20231015222527.png|400]]

`start` tells the program to start a new thread, and `join` tells it to return back to the **main thread**. 
- Everything coded before this lecture is **IN** the main thread

Similar to processes, each CPU core can run one thread at a time, and the OS switches in every ~1-10 ms.

![[Pasted image 20231015222050.png|400]]

On a single-core machine, threads **take turn**. On a multi-core machine, threads run in **parallel** (not in Python).
- Threads still take turns because there are usually many **more threads than cores**

Threads have a lot in common with processes in that they both provide an illusion of parallelism and both can indeed provide true parallelism on the right hardware.
- However, threads **share memory**, implying that
	- There is **LESS** overhead, but
	- **More** security risk, depending on the application

Let's go back to the example of concurrent requests. If we have multiple threads handling multiple requests, things would get a lot more easier.

![[Pasted image 20231015222622.png|400]]

No more unnecessary waiting!

```ad-summary
Threads are useful when
- Multiple things happening at once
	- Within one program
- May need to share data in memory
- Dealing with some usually slow resource
	- Network response
	- Disk I/O
	- Reading a sensor
	- Human input

**Benefits** of threads
- Simpler programming model
	- The illusion of a dedicated CPU per thread
	- State for each thread (local variables)
- OS takes care of CPU sharing
	- Other threads can progress while one thread waits for I/O
```

```ad-tldr
**Processes vs. threads**
- **Processes** have separate address spaces
	- Useful when there is not complete trust
	- Useful to limit the damage caused by buggy code
	- Needed for execution on **different computers**
- Threads share an address space
	- Useful for lower memory overhead
	- Shares data
```

---
### Synchronization
With parallel programs, we need to start thinking about synchronization of data.

Let's look at the following code snippet.

```python
def hello():
	print("hello")
	print("world")

t1 = Thread(target=hello)
t2 = Thread(target=hello)
t1.start()
t2.start()

t1.join()
t2.join()
```

The threads are sharing the **output stream** in a manner that looks like

![[Pasted image 20231015223242.png|400]]

It might occur that there are multiple ways to interleave the operations from `t1` and `t2`.
- Possible outputs include `hello, world, hello, world`, whereas it's impossible to see something like `world, hello, world, hello`

This is oftentimes unpredictable when the OS decides when threads take turns.
- Different timing means different thread order means different output

Thus, we might want to find a way to merge multiple threads together into global order, without producing incorrect results. This leads us to **locks**.

#### Atomic Operations
An atomic operation appears to the other threads as if it happened **instantaneously**.
- No code from other threads can occur in between

```ad-note
A common example is that `print()` behaved atomically. The `print()` output was line **buffered**, which means different lines of output cannot be interleaved.
```

In Python, the following operations are atomic, which means it's safe to perform these operations simultaneously from different threads:
- Update, insert, read, etc. Of dictionaries, lists
- Reads/writes of **built-in types**
- Read and assignment of instance variables

However, **combinations of atomic operations** are not atomic!
- `x=1` are atomic doesn't make `x=x+1` atomic

To let the programmers manually limit when threads take turn, Lock is introduced.
- Allows for mutual exclusion of operations (mutex)

```python
import time
import random
from threading import current_thread,
Thread, Lock

def hello(lock):
	name = current_thread().name
	lock.acquire()
	print(f"{name} hello")
	print(f"{name} world")
	lock.release()

lock = Lock()
t1 = Thread(target=hello, args=(lock,))
t2 = Thread(target=hello, args=(lock,))
t1.start()
t2.start()

t1.join()
t2.join()
```

The above code would produce the following:

![[Pasted image 20231015224337.png|400]]

```ad-tldr
**Threads vs Asynchronous programming**

Threads are multiple functions in one **process** running simultaneously
- OS controls when functions take turns

Asynchronous programming is functions interleaved with one another in a **single thread** of control
- Programmer controls when functions take turns

Both implement concurrency and are illusions of parallelism.
- Threads can provide true parallelism with the right hardware and software
```
