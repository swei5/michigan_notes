[[2024-03-26]] #Database #DBMS #OperatingSystem #System 

In this lecture, we will discuss how to execute queries using **multiple workers** (threads and cores) to increase performance.
- Improve throughput
- Reduce latency
- Increase responsiveness
- Lower total cost of ownership (TCO)
	- Fully utilizing cores of the machine saves cost

We will only focus on **parallel DBMS**s in this course. A parallel DBMS means that the database is spread out across **multiple resources** to improve different aspects of the DBMS **on a SINGLE machine**. This appears as a single logical database instance to the application, regardless of physical organization.

```ad-summary
**Parallel vs. Distributed DBMS**

- **Parallel DBMS**
	- Resources are physically close to each other
	- Resources communicate over high-speed interconnect
	- Communication is assumed to be **cheap and reliable**
- **Distributed DBMS**
	- Resources can be far from each other 
	- Resources communicate using slow(er) interconnect 
	- Communication cost and problems cannot be ignored ([[14 Networking|TCP]])
```

### Process Models
A DBMS’s **process model** defines **how the system is architected to support concurrent requests** from a multi-user application.

A **worker** is the DBMS component that is responsible for **executing tasks on behalf of the client** and returning the results.

There are three primary process models.

#### Process per DBMS Worker 
Each worker is a **separate OS process**. A DBMS would need to fork different processes of the main DBMS process. Examples include IBM, DB2, Postgres, Oracle. 
- Relies on OS scheduler
- Use **shared-memory for global data structures**
- A process crash **DOES NOT** take down entire system

![[Pasted image 20240331001222.png|400]]

The downsides include
- The process model **completely relies on the OS**, DBMS has **NO control**
- Needs to allocate shared memory for global data structure
#### Thread per DBMS Worker 
Single process with **multiple worker threads**. Examples include MSSQL, MySQL, and almost all DBMS created in the past 20 years. 
- DBMS (mostly) manages its own scheduling (with thread libraries)
- May or may not use a dispatcher thread
- Thread crash (may) kill the entire system
	- Can mitigate the problem through exception catching

![[Pasted image 20240331001930.png|400]]

##### Scheduling 
With a threading model, for each query plan, the **DBMS decides** where, when, and how to execute it. The DBMS **knows more than the OS** due to the context it has about the data and queries.
- How many tasks should it use?
- How many CPU cores should it use?
- What CPU core should the tasks execute on?
- Where should a task store its output?

**SQLOS** (Microsoft) is a user-level OS layer that runs **inside of the DBMS** and manages provisioned hardware resources. DBMS **DOES NOT** make any system calls.
- Determines which tasks are scheduled onto which threads
- Also manages I/O scheduling and higher-level concepts like logical database locks
- Can be easily adapted to other OS

It is a **non-preemptive thread scheduling** through instrumented DBMS code.
- Machine's OS not allowed to interrupt the thread

**SQLOS quantum** (unit of execution) is 4 ms but the scheduler **CANNOT** enforce that. DBMS developers **must add explicit yield calls** in various locations in the source code - OS is **NOT responsible**.

![[Pasted image 20240331002930.png|400]]

To control DBMS's own scheduling, we cannot simple emit a tuple. We need to keep track of time - if exceeding a certain limit, we need the thread to **yield control to other threads**.

#### Embedded DBMS
**NOT a formal** process model but a popular way to handle DBMS queries these days. In this model, DBMS runs **inside of the same address space** as the application. Application is (mostly) responsible for threads and scheduling.

There is **NO dedicated server** that runs the DBMS. Examples include SQLite, RocksDB, DuckDB. 
- E.g. A DBMS application can be embedded in a Java program that is responsible for execution and control

```ad-summary
**Process Models, Summary**
Advantages of a multi-threaded architecture, comparing to a multi-process model include 
- Less overhead per context switch
- **DO NOT** have to manage shared memory

However, the thread per worker model **DOES NOT** mean that the DBMS supports **intra-query parallelism**.
```

---
### Execution Parallelism
Let's cover some major definitions first.
- **Inter-Query Parallelism**: Execute multiple disparate queries simultaneously
	- Increases throughput & reduces latency
	- If queries are **read-only**, then this requires **almost no explicit coordination** between queries; buffer pool can handle most of the sharing
	- **HARD** if multiple queries are updating DBMS at the same time
- **Intra-Query Parallelism**: Execute the operations of a single query in parallel
	- Decreases latency for long-running queries, especially for OLAP queries
	- There are **parallel versions of every operator**
		- Can either have multiple threads access **centralized data structures** or use **partitioning to divide tuples up**

There are three ways to achieve intra-query parallelism.

#### Intra-Operator (Horizontal)
Most common approach. The DBMS **decomposes operators into independent fragments** that perform the **same function on different subsets of dat**a.

After **ALL** parallel execution is completed, the DBMS inserts an **exchange operator** (gather in Postgres) into the query plan to **coalesce/split results from multiple children/parent operators**.

![[Pasted image 20240331012949.png|500]]

```ad-example
**Example: Intra-Operator Parallelism**

![[Pasted image 20240331013708.png|500]]
```
##### Exchange Operator 
There are three general ways to implement the exchange operator.
1. **Gather**: **Combine the results** from multiple workers into a **single output stream**
2. **Distribute**: Split a single input stream into **multiple output streams**
	- Useful since next step can use the partitions if it needs parallel execution 
3. **Repartition**: Shuffle multiple input streams across multiple output streams 
	- Useful when parallelism in child operator is **different from** that in parent operator

#### Inter-Operator (Vertical)
Also called pipeline parallelism. Operations are **overlapped in order to pipeline data** from one stage to the next **without materialization**. Workers **execute operators from different segments** of a query plan at the same time. This is most common in streaming system.
- Useful when data continuously arrive (streaming)

#### Bushy
A combination of the first two approaches.