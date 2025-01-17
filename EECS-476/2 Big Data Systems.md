[[2025-01-17]] #DataMining #DistributedSystem #System 

### Storage Infrastructure
In a *classical*, *single-node* architecture, data mining takes place on the disk, on a single computer. For significantly larger datasets, we need to rely on **clusters**.

**Large-scale computing** for **data mining** happens on **commodity hardware**. Challenges, however, include
- How to distribute computation?
- How to make distributed/parallel programming easier?
- How to achieve fault tolerance?

A typical cluster architecture looks something like

![[Pasted image 20250117135435.png|450]]

Switches act as data transfer/communication and coordination agents in the system.

```ad-faq
**Problem**: Copying data over a network takes **time**.
**Answer**: **Spark / Hadoop**
- Bring computation close to the data
	-  **Programming model**
		- MapReduce
		- Spark
- Store files multiple times for reliability
	- **Storage Infrastructure – File system**:
		- Google: GFS
		- Hadoop: HDFS
```

```ad-faq
**Problem**: If nodes fail, how to store data persistently?
**Answer**: **Distributed File System**
- Provides global file namespace
- Google GFS; Hadoop HDFS;
```

A distributed file system has the following components:
- **Chunk servers**
	- File is split into contiguous chunks
	- Typically, each chunk is 16-64MB
	- Each chunk **replicated** (usually 2x or 3x)
	- Try to keep replicas in different racks
- **Master node** - a.k.a. **Name Node** in Hadoop’s HDFS
	- Stores metadata about where files are stored
	- More **robust** to hardware failures than chunk servers & run critical cluster services
- **Client library** for file access
	- Talks to master to find chunk servers
	- Connects directly to chunk servers to access data

![[Pasted image 20250117140708.png|400]]

Typical usage patterns of these systems include
- Huge files (100s of GB to TB)
- Data is **RARELY updated** in place
- Reads and appends are common

---
### Programming Model: MapReduce
MapReduce is a style of programming designed for:
- Easy parallel programming
- Invisible management of hardware and software failures
- Easy management of very-large-scale data

It has several implementations, including
- Hadoop
- The original Google implementation - "MapReduce"
- Other MapReduce-_style_ implementations
	- **Spark**
	- **Flink**

```ad-note
**MapReduce Steps**, Review
1. **Map**:
	- Apply a user-written Map function to each input element
		- Mapper applies the Map function to a single element
		- Many mappers grouped in a Map task (the unit of parallelism)
	- Output of Map function is a set of 0, 1, or more key-value pairs.
2. **Group by key**: Sort and shuffle
	- System sorts all the key-value pairs by key, and
	- outputs key-(list of values) pairs
3. **Reduce**:
	- User-written Reduce function is applied to each key-(list of values)
```

![[Pasted image 20250117141134.png|400]]

A classical example is word counting. 

![[Pasted image 20250117141528.png|400]]

Note that we only need **sequential reads** because of the setup of the model, which saves a lot of time compared to random reads.

In codes, this is

```python
def map(key, value):
	# key: document name; value: text of the document
	for each word w in value:
		emit(w, 1)

def reduce(key, values):
	# key: a word; value: an iterator over counts
	result = 0
	for each count v in values:
		result += v
	emit(key, result)
```

In general, it has the following structure:

![[Pasted image 20250117143224.png|450]]

Alternatively, in a parallel system: 

![[Pasted image 20250117143315.png|450]]

All phases are distributed with many tasks doing the work.

```ad-summary
MapReduce environment takes care of:
- **Partitioning** the input data
- **Scheduling** the program’s execution across a set of machines
- Performing the **group by key** step
- Handling machine **failures**
- Managing required inter-machine **communication**
```

#### Master Node 
The master node takes care of **coordination**.
- **Task status:** (idle, in-progress, completed)
- **Idle tasks** get scheduled as workers become available
- When a map task completes, it sends the master the **location** and **sizes** of its $R$ intermediate files, one for each reducer
- Master pushes this info to reducers
- Pings workers periodically to detect failures

#### Fault Tolerance
- **Map worker failure**
	- Map tasks completed or in-progress at worker are **reset to idle and rescheduled**
	- Reduce workers are **notified** when task is **rescheduled** on another worker
- **Reduce worker failure**
	- Only in-progress tasks are reset to idle
	- Reduce task is restarted
- **Master failure**
	- MapReduce task is **aborted** and **client is notified**

```ad-note
**Rule of Thumb**

Given a MapReduce system with $M$ map tasks and $R$ reduce tasks, 
- Make $M$ **much larger** than the number of nodes in the cluster
- One DFS chunk per map is common
- Improves **dynamic load balancing** and speeds up recovery from worker failures

Usually, $R$ is **smaller** than $M$. The number of reducers determines the number of outputs.
```

```ad-note
**Limitations of MapReduce**

MapReduce greatly simplifies “big data” analysis on large, unreliable clusters.
- Simple interface: map and reduce
- Hides the details of parallelism, data partition, fault- tolerance, load-balancing

**Problems**, however, include
- Cannot support complex applications **efficiently**
- Cannot support interactive applications **efficiently**
- **Difficulty of programming**: Many big data problems / algorithms aren’t easily described as MapReduce
- **Performance bottleneck**s, or batch not fitting the use cases (Saving to disk is typically much slower than in-memory work)
- **Root cause**: Inefficient data sharing

In MapReduce, the only way to share data across jobs is **stable storage** - slow!

![[Pasted image 20250117145418.png|450]]

MapReduce incurs substantial overheads due to data replication, disk I/O, and serialization.
- Outputs of mappers $M$ are saved on the disk, sorted, and then read again by reducers $R$ (HDFS read, HDFS write); however, this is necessary for **fault tolerance**
```

Overall, MapReduce doesn’t work well for large applications - often, one needs to chain multiple map-reduce steps.

---
### Data Flow Systems
MapReduce uses two *task ranks*:
- First rank: Map 
- Second rank: Reduce
- Data flows from the first rank to the second

**Data Flow Systems** generalize in two ways:
1. Allow any number of tasks/ranks
2. Allow functions other than Map & Reduce

If data flow is in one direction only (DAG, directed acyclic graph), we can have the blocking property and **allow recovery** of tasks rather than restarting whole jobs after a failure.

A typical analytical software stack looks something like 

![[Pasted image 20250117150623.png|450]]

---
### Spark
Spark is a popular data flow system - expressive computing system, not limited to the map-reduce model. It extends the MapReduce model:
- Fast data sharing
	- Avoids saving intermediate results to disk
	- Caches data for repetitive queries (e.g., for machine learning)
- General execution graphs (DAG, directed acyclic graph)
- Richer functions than just map and reduce

It is **compatible** with Hadoop. Spark offers high-level APIs:
- DataFrames & DataSets
- Introduced in more recent versions of Spark
- Different APIs for aggregate data, which allowed to introduce SQL support

#### Resilient Distributed Dataset (RDD)
- **Partitioned** collection of records
	- Generalizes (key-value) pairs
- Spread across the cluster, **read-only** (immutable)
- Caching dataset in memory
	- Fallback to disk is possible

RDDs can be created from Hadoop or by transforming other RDDs. They are best suited for applications that apply the **same operation to all elements** of a dataset.

There are two key types of operations for RDD.
- **Transformations**: build RDDs through deterministic operations on other RDDs
	- Define new dataset based on previous ones
	- e.g., `map`, `flatmap`, `filter`, `join`, `union`, `reducebykey`, etc.
	- **Lazy evaluation**: nothing computed until an action requires it
- **Actions**: start a job to execute on cluster (return value / export data)