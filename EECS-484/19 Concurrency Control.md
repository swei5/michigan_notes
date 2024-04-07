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
2. **Shadow Paging**
	- DBMS makes copies of pages and transactions make changes to those copies; only when the **transaction commits is the page made visible** to others
	- Only couchDB and LMDB use this (less efficient because of copying and random I/Os)

#### Consistency 
Consistency means that the "world" represented by the database is logically correct. All questions asked about the data are given logically correct answers.
- **Database Consistency**
	- The database **accurately models** the real world and follows integrity constraints
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