[[2023-10-04]] #MapReduce #DistributedSystem

### Distributed System
Distributed system is when **multiple computers cooperating** on a task.  ^56108f
- Stored in data centers
	- Lots of computers means lots of failures
	- Old strategy: buy a few expensive, reliable servers
	- New strategy: buy lots of cheap, unreliable servers, use software to make them reliable
- Implemented by
	- Networking for **communication**
	- Threads and processes for **parallelization**

We will cover these topics later on in the year.

Distributed systems are important because they are the solution for dealing with the **scale** of the web. ^664ec2

Some examples of distributed systems include:
- **MapReduce**: Distributed system for **compute**
	- Run a program that would be too slow on one computer
- **Google File System**: Distributed system for **storage**
	- Store more data than fits on one computer

```ad-question
What kinds of programs are too big to run on one computer?
- Count the number of requests to each web page
	- Given several TB of log files
- Build a search engine inverted index ("look up table")
	- Over several PB of HTML files
- Count the frequency of words
	- In several PB of text files
```

Let's use an example to illustrate the motivation behind MapReduce. Assume we have the following Python program.

```python
import sys
import collections

word_count = collections.defaultdict(int)
for line in sys.stdin:
	words = line.split()
	for word in words:
		word_count[word] += 1

for word, count in word_count.items():
	print(word + "\t" + count)
```

This seems pretty straight-forward if the input is just a few line of words or even a book. What if we run this program on the entire web?
- Time complexity: $O(n)$ - where $n$ is the number of web pages (10 billion)
	- Assuming web page length is constant
- Space complexity: $O(m)$ - where $m$ is the number of unique words

It wouldn't help if we run this program on 1,000 computers, because our program is **NOT parallel**.

#### Pure Functions, Parallelization
We can sure split input into two files then run same program on two computers, but these computers **CANNOT** share a data structure.
- Programs that do not share data structures are easier to run in parallel
- In this case, the function can't modify any shared data but only read input and produce output - known as **stateless**, AKA **pure functions**

```ad-note
A shared data structure could still be implemented using a **message passing system** but it would be **really SLOW**. The network is a bottle neck.
```

#### Group, Reduce
^3749b4

Instead of having a shared data structure, let's just have two **computers**, two **data structures** running on two inputs to get two **outputs**.
- We will need to put together the output

This can be done through grouping and later reducing. First, we group the output by **key**, which guarantee that every line with the **same key** is in the **same group**. (Assume we somehow came up with this grouping program)

![[Pasted image 20231004124022.png|500]]

Then, we can "put together the output", which is called **reducing**.
- Runs in parallel, just like map

![[Pasted image 20231004124123.png|600]]

In this example, the second program is very similar to the first. Note that this is only possible when **same keys** are in the **same group**.

However, at this point, we would still have a bunch of problems to tackle if we scale up the problem from a word-counting utility to something along the lines of web statistics tracker. 
- Don't know how many machines until run time
- What happens if one machine dies during a long running computation?
- Have to solve for the same parallelization problems

Today we will focus on **MapReduce**.

A **MapReduce** framework executes MapReduce program.
- Parallelization over many machines
- Segment and distribute input
- Fault tolerance

It also provides a clean abstraction for programers.
- Programmers will have to write two programs to implement this, surprisingly:
	- `map()`
	- `reduce()`

---
### MapReduce Programming Model
In this section, we  will be using the **Hadoop MapReduce streaming interface**.

```ad-tldr
Map is a program.
- Each line of input is from an input file
- Each line of output is `<key>\t<value>`

Group is provided by MapReduce framework.

Reduce is a program.
- Each line of input is `<key>\t<value>`
- Each line of output is up to the programmer
```

Let's revisit the word count example. Mappers **DON'T count** words, it just **splits** them up. The code would look something like:

```python
import sys

for line in sys.stdin:
	line = line.strip()
	words = line.split()
	for word in words:
		print(word + "\t" + "1")
```

After the program finishes executing, we would anticipate the following:

![[Pasted image 20231004124837.png|500]]

This is known as the **intermediate key value pairs**. Next up is grouping, which is provided by the MapReduce framework. We group the pairs by keys.

![[Pasted image 20231004125046.png|500]]

And finally, we reduce the inputs into word count.

```python
counts = collections.defaultdict(int)

for line in sys.stdin:
	line = line.strip()
	word, count = line.split("\t")
	counts[word] += int(count)
	
for word, count in counts.items():
	print(word + "\t" + str(count))
```

However, there're still some lingering issues here. The intermediate data size could be **LARGE**. 

We have two ways to solve this.
1. A "local reducer" called a **Combiner** at each map
	- Compresses data between map and reduce
2. A different map implementation
	- This is exactly our [[#^3749b4|previous implementation]]
		- Add up word occurrences in the `map()` code

![[Pasted image 20231004203457.png|500]]

---
### MapReduce Execution Model
We can break it down into four processes.
1. Segment input
2. Map stage
3. Group stage
4. Reduce stage

Below is a simplified execution model that describes such process.

First, client will submit word count job, indicating **map code**, **reduce code** and input files.

```json
{
  "mapper": "wc_map.sh",
  "reducer": "wc_reduce.sh"
}
```

Then, main breaks input file into $k$ chunks, (in this case 6). Assigns work to workers.

![[Pasted image 20231004194643.png|600]] ^d9ff86

After `map()`, workers **exchange** map-output to produce **groups**. 

![[Pasted image 20231004194735.png|300]]

Main breaks `reduce()` keyspace into $m$ chunks (in this case 6). Assigns work. And finally, `reduce()` output may go to shared file system.

MapReduce framework provides grouping functionality.

![[Pasted image 20231004194940.png|500]]

```ad-important
Important takeaway here is that
1. All values for the **same key** must be in the **same group**
2. A group have more than 1 key
	- Shown in the example above
```

---
### Fault tolerance
A few common questions we might want to ask ourselves in complicated scenarios (large-scale projects):
- How do we know if a machine goes down?
	- Workers send **periodic heartbeat messages** to Main
	- Main keeps track of which workers are **UP**
		- Similar technique to Google File System
- What happens when a machine dies?
	- Without MapReduce, we have to restart the program once a machine dies.
	- However, with MapReduce, we would just have to
		- Restart that task on a different box, if **map worker** dies
		- Restart the reducer, using output from source mappers, if **reduce worker** dies
- What about slow, not dead, yet slow machines?
	- Run the same map job on two machines, and kill the second-place finisher

---
### Summary
Scalability and fault-tolerance achieved by **optimizing** the execution engine once.
- This is MapReduce
- Then we can use it many times by writing **different map** and **reduce** functions for different applications
	- This calls for stateless mapper and stateless reducer

Map and reduce functions are inspired by functions of the same name in **Lisp** programming language.
- Functional programming
	- Computation as the evaluation of mathematical function
	- Functions have no side effects, AKA pure, stateless functions
	- Easy to **parallelize**!