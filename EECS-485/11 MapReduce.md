[[2023-10-04]] #MapReduce 

### Distributed System
Distributed system is when **multiple computers cooperating** on a task. 
- Stored in data centers
	- Lots of computers means lots of failures
	- Old strategy: buy a few expensive, reliable servers
	- New strategy: buy lots of cheap, unreliable servers, use software to make them reliable
- Implemented by
	- Networking for **communication**
	- Threads and processes for **parallelization**

We will cover these topics later on in the year.

Distributed systems are important because they are the solution for dealing with the **scale** of the web.

Some examples of distributed systems include:
- **MapReduce**: Distributed system for compute
	- Run a program that would be too slow on one computer
- **Google File System**: Distributed system for storage
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
A shared data structure could be implemented using a **message passing system** but it would be **slow**. The network is a bottle neck.
```

#### Group, Reduce

^3749b4

Instead of having a shared data structure, let's just have two **computers**, two **data structures** running on two inputs to get two **outputs**.
- We will need to put together the output

This can be done through grouping and later reducing. First, we group the output by **key**, which guarantee that every line with the **same key** is in the **same group**.

![[Pasted image 20231004124022.png|500]]

Then, we can "put together the output", which is called **reducing**.
- Runs in parallel, just like map

![[Pasted image 20231004124123.png|600]]

In this example, the second program is very similar to the first. Note that this is only possible when same keys are in the **same group**.

However, at this point, we would still have a bunch of problems to tackle.
- Don't know how many machines until run time
- What happens if one machine dies during a long running computation?

Today we will focus on **MapReduce**.

---
### MapReduce Programming Model
In this section, we will continue using the **Hadoop MapReduce streaming interface** as an example.

```ad-tldr
Map is a program.
- Each line of input is from an input file
- Each line of output is `<key>\t<value>`

Group is provided by MapReduce framework.

Reduce is a program.
- Each line of input is `<key>\t<value>`
- Each line of output is up to the programmer
```

Mappers **DON'T count** words, it just **splits** them up. The code would look something like:

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

---
### MapReduce Execution Model
