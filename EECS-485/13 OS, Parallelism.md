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

This seems a little undesirable if we have a moderately slow database. Moreover, imagine the case for **concurrent requests**. 

![[Pasted image 20231015151914.png|400]]

In the example above, client 2 will have to wait for unreasonably long amount of time because the server is busy dealing with the request sent from client 1!

Sequential programs could be problematic when
- Multiple things happening at once (concurrent requests)
- Slow resource (slow database read)

Essentially, such program needs to spend a lot of time **waiting**. Hence, the solution is, naturally, to work on both requests the **SAME** time.

---
### Processes
In simple language, a process is a running program.
- Much like what we would see within the task manager of computer

More formally, the Process is an **OS abstraction for execution**. A process is a program in execution. Programs are **static entities** with potential for execution. A process is consisted of
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