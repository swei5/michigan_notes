[[2024-03-26]] #Database #DBMS 

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

**SQLOS** (Microsoft) is a user-level OS layer that runs **inside of the DBMS** and manages provisioned hardware resources. DBMS **DOES NOT** make any s
- Determines which tasks are scheduled onto which threads
- Also manages I/O scheduling and higher-level concepts like logical database locks

It is a **non-preemptive thread scheduling** through instrumented DBMS code.
- Machine's OS not allowed to interrupt the thread

#### Embedded DBMS
**NOT a formal** process model but a popular way to handle DBMS queries these days.