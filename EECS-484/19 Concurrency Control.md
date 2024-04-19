[[2024-04-05]] #Database #DBMS 

```ad-todo
Roadmap of DBMS:
- ~~Disk Manager~~
- ~~Access Methods~~
- ~~Operator Execution~~
- ~~Query Planning~~
- **Transaction Manager**
- Log Manager
```

We have learned about how queries are executed and planned **bottom up**. The last two components, transaction manager and log manager are **NOT in sequential order** but works with all parts of the system.

A DBMS's concurrency control and recovery components permeate throughout the design of its **entire architecture**.

The two main properties we want to maintain are **concurrency control and recovery**.
- Concurrency control (Lost updates)
	- *How to avoid race condition?*
- Recovery (Durability)
	- *What is the correct database state?*

These are fundamental properties of a DBMS, known as ACID. This sets a formal DBMS apart from a basic spreadsheet like `csv`. Before delving deeper, we need to understand the concept of transaction.

### Transactions
A transaction is the **execution of a sequence of one or more operations** (e.g., SQL queries) on a database to perform some higher-level function. 
- By default, every query will be its own transaction

It is the **basic unit** of change in a DBMS. Partial transactions are **NOT allowed**.

#### Strawman System
Execute each transaction **one-by-one** (i.e., serial order) as they arrive at the DBMS.
- **One and only one** transaction can be running at the same time in the DBMS

Before a transaction starts, copy the entire database to a **new file** and **make all changes** to that file.
- If transaction completes successfully, **overwrite** the original file with the new one
- Else, **remove** the dirty copy

A (potentially) better approach to it is to **allow concurrent execution** of independent transactions.
- Better utilization/throughput 
- Increased response times to users

---
### Motivation
**Arbitrary interleaving** of operations can lead to temporary inconsistency (unavoidable) or permanent inconsistency (**BAD**). However, we want to maintain **correctness** and **fairness** throughout still. We would need formal correctness criteria to determine whether an interleaving is valid.

A transaction may carry out many operations on the data retrieved from the database. However, the DBMS is only concerned about **what data is read/written** from/to the database.

To a DBMSs' perspective, database is a **fixed set of named data objects**, e.g. $A,B$, and a transaction is a **sequence of read/write operations**, e.g. $R (A), W(B)$
- Does not concern the granularities of the objects
- Fixed set means that we do not take into account `INSERT` or `DELETE` commands 

In SQL, a new transaction starts with the `BEGIN` command and stops with either `COMMIT` or `ABORT` command. Semantics may differ among different systems.
- If `COMMIT`, the DBMS **saves all the transaction's** changes; 
- If `ABORT`, all changes are undone so that it's like as if the **transaction never executed** at all
	- Can be either self-inflicted or caused by the DBMS

---
### ACID
The correctness criteria are comprised of the four:
- **Atomicity**: All actions in the transaction happen, or none happen
	- “all or nothing”
- **Consistency**: If each transaction is consistent and the DB starts consistent, then it ends up consistent
	- “it looks correct to me”
- **Isolation**: Execution of one transaction is isolated from that of other transactions.
	- “as if alone”
- **Durability**: If a transaction commits, its effects persist.
	- “Survive failures”

These properties are supported by the transaction manager and log manager in a DBMS.
#### Atomicity 
There are two possible outcomes of executing a transaction:
1. Commit after completing all its actions 
2. Abort (or be aborted by the DBMS) after executing some actions

From a user's point of view: transaction always either executes all its actions or executes no actions at all.

There are two general approaches to ensure atomicity in a DBMS:
1. **Logging**
	- DBMS **logs all actions** so that it can **undo the actions** of aborted transactions
	- Maintain undo records both in memory and on disk
	- Logging is used by almost every DBMS
2. **[[21 Logging#Shadow Paging|Shadow Paging]]**
	- DBMS makes copies of pages and transactions make changes to those copies; only when the **transaction commits is the page made visible** to others
	- Only couchDB and LMDB use this (less efficient because of copying and random I/Os)

#### Consistency 
Consistency means that the "world" represented by the database is logically correct. All questions asked about the data are given logically correct answers.
- **Database Consistency**
	- The database **accurately models** the real world and follows **integrity constraints**
	- Transactions in the future **see the effects of transactions committed in the past inside** of the database
- **Transaction Consistency**
	- If the database is consistent before the transaction starts (running alone), it will also be consistent after
	- Application's responsibility - DBMS **DOES NOT control this**

#### Isolation, Concurrency Control
When users submit transactions, each transaction executes as if it was running by itself.
- However, DBMS achieves concurrency by interleaving the actions of transactions
	- We need a way to interleave transactions but still make it appear as if they ran **one at a time**

This is known as **concurrency control**. It describes how the DBMS decides the **proper interleaving of operations** from multiple transactions.

There are two categories:
- **Pessimistic**: Assume conflicts happen all the time, don't let problems arise in the first place
- **Optimistic**: Assume conflicts are rare, deal with them after they happen

```ad-example
**Transaction Conflict, Example**

![[Pasted image 20240406221919.png|500]]

Given the order in which we execute `T1` and `T2`, there could be many posibilities. However, there is this one constraint that `A+B=2000*1.06=2120` after all transactions take place.

In other words, the net effect must be equivalent to these two transactions running serially in some order.

![[Pasted image 20240406222138.png|500]]
```

Why do we care about interleaving at all? We interleave transaction to **maximize concurrency**.
- Slow disk/network I/O
- Multi-core CPUs

When one transaction stalls because of a resource (e.g., page fault), another transaction can continue executing and make forward progress. See [[13 OS, Parallelism#Synchronization|notes on synchronization on operating system]].

Ideally, the correctly interleaved result should be equivalent to a serially produced result. Both are valid.

![[Pasted image 20240406222748.png|500]]

However, some particular scheduling may produce invalid results. Conceptually, this is:

![[Pasted image 20240406222953.png|500]]

##### Correctness 
The schedule of some transaction is correct if it is **equivalent to some serial execution**.

A **serial schedule** is a schedule that **DOES NOT interleave** the actions of different transactions. 

An **equivalent schedule** is a schedule such that for any database state, the effect of executing the first schedule is identical to the effect of executing the second schedule, regardless of what the arithmetic operations are.

A **serializable schedule** is a schedule that is **equivalent to some serial execution** of the transactions.
- This provides the DBMS with **additional flexibility** in scheduling operations with parallelism, compared to a direct timestamp approach

If each transaction preserves consistency, **every serializable schedule preserves consistency**.

We need a formal notion of equivalence that can be implemented efficiently based on the notion of **"conflicting" operations**.

##### Conflicts 
Two operations **conflict** if
- They are by **different transactions**, and
- They are on the **same object** and **AT LEAST ONE** of them is a **write**

Naturally, this boils down to three general anomalies:
- Read-Write Conflicts (R-W)
	- Unrepeatable reads

![[Pasted image 20240406224856.png|400]]

- Write-Read Conflicts (W-R)
	- Reading Uncommitted Data ("Dirty Reads")

![[Pasted image 20240406224928.png|400]]

- Write-Write Conflicts (W-W)
	- Overwriting Uncommitted Data 

![[Pasted image 20240406225202.png|400]]

Given these conflicts, we now can understand what it means for a schedule to be **serializable**.
- This is to **check whether schedules are correct**
- This is **NOT** how to generate a correct schedule

There are different levels of serializability:
- **Conflict Serializability**
	- Most DBMSs support this
- **View Serializability**
	- Very difficult to achieve in practice

##### Conflict Serializable Schedules 
Two schedules are **conflict equivalent** iff
- They involve the **same actions of the same transactions**, and
- Every pair of **conflicting actions is ordered the same way**

Schedule $S$ is conflict serializable if $S$ is **conflict equivalent** to some **serial schedule**. In other words, schedule $S$ is **conflict serializable** if you can **transform S into a serial schedule** by **swapping consecutive non-conflicting operations** of different transactions.

```ad-example
**Conflict Serializable Transaction, Example**

Imagine a schedule like this:

![[Pasted image 20240406231159.png|200]]

We are able to swap `R(B)` and `W(A)` and `R(A)` and `W(B)` as they are non-conflicting operations of different transactions (as done to different DB objects). Eventually, we could potentially end up with a serializable schedule:

![[Pasted image 20240406231307.png|400]]

```

Swapping operations is easy when there are only two transactions in the schedule. It's cumbersome when there are many transactions.

##### Dependency Graphs
A quicker way to determine if two schedules are conflict serializable. This is also known as a **precedence graph**.

We will construct a **node per transaction**, and draw an edge from $T_{i}$ to $T_{j}$ if
- An operation $O_{i}$ of $T_{i}$ conflicts with an operation $O_{j}$ of $T_{j}$, and
- $O_{i}$ is earlier than $O_{j}$ in schedule

Using this representation, a schedule is **conflict serializable** iff its dependency graph is **acyclic**.

```ad-example
**Dependency Graph, Example 1**

![[Pasted image 20240413192844.png|500]]

The first step to build the graph is **finding the conflicts** between the transactions and determining who arrives first. If there are multiple conflicts between two transactions, we would only need to show an edge.

An edge from $T_{1}$ to $T_{2}$ means that the result of $T_{2}$ depends on $T_{1}$, and vice versa. A cycle implies that both transactions **depend on EACH OTHER** - no way to serialize it.
```

```ad-example
**Dependency Graph, Example 2**

![[Pasted image 20240413220919.png|500]]

This would be a serializable schedule - in the order of $T_{2}, T_{1}, T_{3}$ (follow the ordered graph), though in reality the transactions happen in a different order.
```

Although some schedule **may NOT be serializable**, it is still **possible** for the transactions to produce the correct result if the application **does NOT directly interact** with the conflicting output.
- However, DBMS never knows what application does

##### View Serializability 
An alternative and weaker notion of serializability. 
- Allows slightly **more schedules** than conflict serializability 
- Hard to implement and difficult to enforce
- Conflict serializability is more popular

In short, it allows for **ALL conflict serializable schedules** and **blind writes**.

![[Pasted image 20240413222108.png|500]]

The original schedule is **NOT conflict serializable** (work out a dependency graph) but really is equivalent to a **serializable** schedule (RHS). This is because the conflicts are caused by **blind writes** (does not impact the final result or any `READ`).

**Neither definition** allows all schedules that you would consider "serializable".
- This is because they don't understand the meanings of the operations or the data

![[Pasted image 20240413222506.png|400]]
#### Durability 
All the changes of committed transactions should be **persistent**.
- No torn updates 
- No changes from failed transactions 

The DBMS can use either **logging or shadow paging** to ensure that all changes are durable.

We will cover [[21 Logging#^521cf4|this]] later on.

```ad-summary
**Concurrency, Summary**
Concurrency control and recovery are among the most important functions provided by a DBMS.
- **Automatic**: system automatically inserts lock/unlock requests and schedules actions of different transactions
- It ensures that resulting execution is equivalent to executing the transactions one after the other in some order
```

