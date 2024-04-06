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

Arbitrary interleaving of operations can lead to temporary inconsistency (unavoidable) or permanent inconsistency (**BAD**). However, we want to maintain correctness and fairness throughout still. We would need formal correctness criteria to determine whether an interleaving is valid.

A transaction may carry out many operations on the data retrieved from the database. However, the DBMS is only concerned about **what data is read/written** from/to the database.

To a DBMSs' perspective, database is a **fixed set of named data objects**, e.g. $A,B$, and a transaction is a **sequence of read/write operations**, e.g. $R (A), W(B)$

