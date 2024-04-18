[[2024-04-16]] #Database #DBMS 

**Recovery algorithms** are techniques to **ensure database consistency, transaction atomicity, and durability** despite failures. It has two parts:
1. Actions during normal transaction processing to ensure that the DBMS can recover from a failure (Lecture 21)
2. **Actions after a failure to recover the database to a state that ensures atomicity, consistency, and durability** (Today)

### ARIES
**ARIES** stands for Algorithms for Recovery and Isolation Exploiting Semantics. Not all systems implement ARIES exactly as defined in this paper but they're close enough.

The main ideas of ARIES are as followed:
- **Write-Ahead Logging** ([[21 Logging#Write-Ahead Log (WAL)|WAL]])
	- Any change is recorded in log on stable storage before the database change is written to disk
	- Must use **STEAL + NO-FORCE** buffer pool policies
- **Repeating History During Redo**
	- On restart, retrace actions and **restore database to exact state before crash**
- **Logging Changes During Undo**
	- Record undo actions to log to ensure action is not repeated in the event of repeated failures
		- Undo uncommitted changes

---
### Log Sequence Numbers
We need to extend our log record format from last class to include additional info. Every log record now includes a **globally unique log sequence number** (LSN).

Various components in the system keep track of LSNs that pertain to them.
- Each component (disk, buffer pools, checkpoints) each has to maintain a watermark of the LSN

![[Pasted image 20240418001644.png|500]]

Each data page contains a `pageLSN`.
- The LSN of the **most recent update** to that page

The system keeps track of `flushedLSN` in memory.
- The **max LSN flushed** so far

Before page `x` can be written to disk, **we must FLUSH log** at least to the point where:
- `pageLSNx` $\le$ `flushedLSN`
- Similar idea [[21 Logging#^2fa2e2|here]]

![[Pasted image 20240418002656.png|500]]

---
### Commit and Abort Operations
#### Writing Log Records
**ALL** log records have an LSN. We **update the `pageLSN`** every time a transaction **modifies a record in the page**. **We update the `flushedLSN`** in memory every time the DBMS **writes out the WAL buffer to disk**. 

For the purpose of this lecture, we assume that
- Each transaction invokes a sequence of reads and writes, followed by `COMMIT` or `ABORT`
- All log records fit within a single page
- Disk writes are **atomic**
- **Strong Strict 2PL** for concurrency control
- **STEAL + NO-FORCE** buffer management with WAL

#### Transaction Commit
When a transaction commits, we write `COMMIT` record to log. All log records up to a transaction's `COMMIT` record are **flushed to disk**.
- Log flushes are sequential, synchronous writes to disk
- We need to keep the log records in memory for a little bit longer for book-keeping reasons

When the commit succeeds, write a special `TXN-END` record to log.
- Now safe to truncate log records in memory
	- This **DOES NOT** need to be flushed immediately

#### Transaction Abort
Aborting a transaction is a special case of the ARIES undo operation applied to only one transaction.

We need to add another field to our log records:
- `prevLSN`: **The previous LSN** for the transaction
	- This maintains a linked-list for each transaction that makes it easy to walk through its records backwards

![[Pasted image 20240418004340.png|500]]

This is slightly different from a committing transaction since (1) we don't have to flush any of the aborting transaction's logs onto the disk since they are aborted anyways, and (2) we **DO NEED TO RECORD** what steps we took to undo the transaction but that information also needs not to be flushed.

Note that we are guaranteed that no dirty writes are written onto the disk since log records were not written.

A **Compensation Log Record** (CLR) describes the actions taken to **undo the actions of a previous update record**. It has all the fields of an update log record plus the `undoNext` pointer (the next-to-be-undone LSN, same as `prevLSN`).
- NOT for correctness for mainly for the purpose of efficiency

CLRs are added to log records but the DBMS **DOES NOT wait for them to be flushed** before notifying the application that the transaction aborted. 

![[Pasted image 20240418005002.png|500]]

```ad-summary
**Summary, Abort Algorithm**
1. Write an `ABORT` record to log for the transaction 
2. Play back the transaction's updates in reverse order; for each updated record:
	- **Write a CLR** entry to the log
	- **Restore** old value
3. Write a `TXN-END` log record

Notice: CLRs **NEVER need to be undone** (even if the system crashes).
```

---
### Fuzzy Checkpointing
Recall that the DBMS **halts everything** when it takes a [[21 Logging#Checkpoints|checkpoint]] to ensure a consistent snapshot:
- Halt the start of any new transactions
- Wait until all active transactions finish executing
- Flushes dirty pages on disk (random I/Os)

This is bad for runtime performance but makes recovery easy. However, it ensures safe recovery process as every records updated before the checkpoint is guaranteed to be correct (we can even get rid of all log records before the checkpoint).

A slightly better design will be to pause modifying transaction while the DBMS takes the checkpoint.
- Prevent queries from acquiring write locks on data
- **Don't have to WAIT** until all transactions finish before taking the checkpoint
	- Causing inconsistency and atomicity problems as we get "partial pages"

Hence, we must record additional information - internal state as of the beginning of the checkpoint. 
- **Active Transaction Table** (ATT)
- **Dirty Page Table** (DPT)

#### Active Transaction Table 
One entry **per currently active transaction**.
- `txnId`: Unique transaction identifier
- `status`: The current "mode" of the transaction 
	- `R`: Running 
	- `C`: Committing 
	- `U`: Candidate for Undo
		- Default state during crash recovery, or
		- System/user ordered abort
- `lastLSN`: Most recent LSN created by transaction

Entry is removed after the `TXN-END` message.

#### Dirty Page Table 
Keep track of which pages in the buffer pool contain changes from transactions that **have NOT been flushed to disk** (committed or aborted).

One entry **per dirty page in the buffer pool**:
- `recLSN`: The `LSN` of the log record that first caused the page to be dirty

```ad-example
**Example, Checkpoints**

![[Pasted image 20240418010555.png|500]]
```

However, this still is not ideal because the DBMS must stall transactions during checkpoints.

#### Fuzzy Checkpoints 
A **fuzzy checkpoint** is where the DBMS allows active transactions to **continue** the run while the system writes the log records for checkpoint.
- **NO attempt to force** dirty pages to disk
- New log records to track checkpoint boundaries:
	- `CHECKPOINT-BEGIN`: Indicates start of checkpoint
	- `CHECKPOINT-END`: Contains ATT and DPT

The LSN of the `CHECKPOINT-BEGIN` record is written to the **database's MasterRecord entry on disk** when the checkpoint **successfully completes**.

Any transaction that **starts after the checkpoint** is excluded from the ATT in the `CHECKPOINT-END` record.

---
### Recovery 
1. Analysis
	- **Read WAL from last MasterRecord** to identify **dirty pages** in the buffer pool and **active transactions** at the time of the crash
		- Through ATT and DPT at the `CHECKPOINT-BEGIN`
		- This is **really important** as it narrows the scope for us, i.e. what transactions we would need to look at - potential redo and undo candidates
1. Redo
	- Repeat all actions starting from an appropriate point in the log (even transactions that will abort)
2. Undo
	- Reverse the actions of transactions that **DID NOT commit** before the crash

![[Pasted image 20240418012016.png|400]]

#### Analysis Phase
Scan log **forward from** last **successful checkpoint** (`CHECKPOINT-BEGIN)`.
- If you find a `TXN-END` record, **remove** its corresponding transaction from ATT
- For all other records:
	- Add transaction to ATT with status `UNDO`
	- On commit, change transaction status to `COMMIT`

For `UPDATE` records:
- If page $P$ not in DPT, add $P$ to DPT, set its `recLSN` = `LSN`

At end of the Analysis Phase:
- ATT identifies which transactions were **active** at time of crash
- DPT identifies which **dirty pages** might not have made it to disk

```ad-example
**Example, After Analysis Phase**

![[Pasted image 20240418013051.png|500]]
```

#### Redo Phase 
The goal is to repeat history to reconstruct state **at the moment of the crash**.
- **Reapply all updates** (even aborted transaction!) and redo CLRs

There are techniques that allow the DBMS to avoid unnecessary reads/writes, but we will ignore that in this lecture.

We scan forward from the log record containing **smallest** `recLSN` in DPT. For each update log record or CLR with a given `LSN`, redo the action unless:
- Affected page is not in DPT, or
	- Page has been flushed
- Affected page is in DPT but that record's `LSN` is less than the page's `recLSN`
	- Page has been partially flushed, at least to the point in time of that record
#### Undo Phase
Undo all transactions that were **active at the time of crash** and therefore will **never commit**.
- These are all the transactions with `U` status in the ATT after the Analysis Phase

Process them in **reverse `LSN`** order using the `lastLSN` to speed up traversal. Write a CLR for every modification.