[[2024-04-15]] #Database #DBMS 

```ad-todo
Roadmap of DBMS:
- ~~Disk Manager~~
- ~~Access Methods~~
- ~~Operator Execution~~
- ~~Query Planning~~
- ~~Transaction Manager~~
- **Log Manager**
```

Motivation: in the case of database failure, how do we inform the client of **committed changes that were NOT COMMUNICATED** to the disk.

**Recovery algorithms** are techniques to **ensure database consistency, transaction atomicity, and durability** despite failures. It has two parts:
1. **Actions during normal transaction processing to ensure that the DBMS can recover from a failure** (Today)
2. Actions after a failure to **recover** the database to a state that ensures atomicity, consistency, and durability (Next section)

### Failure Classification
DBMS is divided into different components based on the underlying **storage device**.
- **Volatile Storage** 
	- Data **DOES NOT** persist after power loss or program exit
	- E.g. DRAM, SRAM
- **Non-volatile Storage**
	- Data **persists** after power loss and program exit
	- E.g. HDD, SDD
- **Stable Storage**
	- Only exists in theory
	- A non-existent form of **non-volatile storage** that **survives ALL possible failures** scenarios

We must also classify the **different types of failures** that the DBMS needs to handle.

#### Transaction Failures 
There are two broad categories.
- **Logical Failures**
	- Transaction cannot complete due to some **internal error condition**
	- E.g., integrity constraint violation
- **Internal State Errors**
	- DBMS **MUST terminate** an active transaction due to an error condition
	- E.g. deadlocks

#### System Failures 
There are two broad categories.
- **Software Failure**
	- Problem with the **OS or DBMS implementation**
	- E.g. uncaught divide-by-zero exception
- **Hardware Failure**
	- The **computer** hosting the DBMS **crashes** 
	- E.g. power plug gets pulled

```ad-note
Fail-stop Assumption: **Non-volatile storage** contents are assumed to **NOT** be corrupted by system crash (stored on hardware).
```

#### Storage Media Failures
Also known as **Non-Repairable Hardware Failure**:
- E.g. A head crash or similar disk failure destroys all or part of non-volatile storage
- Destruction is assumed to be **detectable** (e.g., disk controller use checksums to detect failures)

DBMS **CANNOT address** it by itself - needing external support to address it manually and to be restored from archived version.

```ad-note
The **primary storage location** of the database is on **non-volatile storage**, but this is much slower than volatile storage.

However, we still use volatile memory for faster access:
- First copy target record into memory
- Perform the writes in memory
- Write dirty records back to disk

Nevertheless, this is a naive approach to ensure recovery - we want to minimize the number of IOs - if not we prefer sequential IOs over random IOs.
```

Hence, we would like the DBMS to ensure the following guarantees:
- The changes for any transaction are **durable** once the DBMS has told somebody that it committed (durability)
- **NO partial changes** are durable if the transaction aborted (atomicity)

---
### Buffer Pool Policies
Now let's first discuss the definitions of **undos** and **redos**.
- **Undo**: The process of removing the effects of an incomplete or aborted transaction
	- In case DBMS aborts a transaction
- **Redo**: The process of re-instating the effects of a committed transaction for durability
	- In case DBMS fails after commits a transaction

How the DBMS supports this functionality depends on how it manages the buffer pool.

#### Steal Policy
Whether the DBMS allows an **uncommitted transaction** to **overwrite** the **most recent committed value** of an object in non-volatile storage (written to the disk).
- **STEAL**: allowed
- **NON-STEAL**: **NOT allowed**

```ad-example
**Example, Steal Policy**

![[Pasted image 20240415225215.png|500]]
```

#### Force Policy 
Whether the DBMS requires that **ALL updates** made by a transaction are reflected on non-volatile storage **before the transaction can commit**.
- **FORCE**: required
- **NON-FORCE**: **NOT** required

#### No-Steal and Force 
This approach is the easiest to implement:
- Never have to **undo** changes of an aborted transaction because the **changes were not written to disk**
- Never have to **redo** changes of a committed transaction because all the changes are **guaranteed to be written to disk at commit time** (assuming atomic hardware writes)

However, it cannot support write sets that **exceed the amount of physical memory** available.
- Expensive or even impossible

```ad-example
**Example, No-Steal and Force**

![[Pasted image 20240415225658.png|500]]

In case of a no-steal and force policy, we would need to create a second copy of the page that rollbacks the changes made to $A$ (because of no-steal policy) and flush that page to the disk immediately at commit time (force policy).

After that, aborting in $T_{1}$ should be trivial since rollback has been done in the disk to ensure durability.
```

---
### Shadow Paging
^521cf4
Maintain **two separate copies** of the database:
- **Master**: Contains only changes from committed transaction
- **Shadow**: Temporary database with changes made from uncommitted transaction

Both copies can be greater than the memory. Transactions only make updates in the shadow copy. When a transaction commits, **atomically switch the shadow to become the new master** (pointer).

This would be a **no-steal and force** policy.

To install the updates, **overwrite the root so it points to the shadow**, thereby **swapping** the master and shadow.
- Before overwriting the root, none of the transaction's updates are part of the disk-resident database
- After overwriting the root, all the transaction's updates are part of the disk-resident database

```ad-example
**Example, Shadow Paging**

Initially, the master page table and shadow page table should be pointing to the same pages in disk, since **NO changes** happened.

![[Pasted image 20240415232425.png|500]]

Then, when a modifying transaction happens, we would only need to update the shadow pages. In this case, we would write copies of the pages onto the disks and re-route the corresponding shadow page pointers to those pages.

![[Pasted image 20240415232614.png|500]]

After the transaction commits and update is finished, the DB root now can safely point to the shadow page table (which is afterwards the master page table).

![[Pasted image 20240415232717.png|500]]
```

Shadow paging supports easy recovery and rollbacks.
- **Undo**: Remove the shadow pages; leave the master and the DB root pointer alone
- **Redo**: **NOT NEEDED** at all

```ad-summary
**Shadow Paging, Disadvantages**

**Copying** the entire page table is expensive.
- To combat this, use a page table structured like a B+tree
	- No need to copy entire tree, only need to copy paths in the tree that lead to updated leaf nodes

**Commit overhead** is high.
- Flush every updated page, page table, and root
- Data gets fragmented (lots of random I/Os since shadow pages live in random locations)
- Need garbage collection
- **Only supports ONE writer transaction** at a time or txns in a batch
```

SQLite has used a similar system to shadow paging. When a transaction modifies a page, the DBMS copies the original page to a separate journal file before overwriting master version (reverse shadow paging).

Shadowing page requires the DBMS to perform writes to random non-contiguous pages on disk. We need a way for the DBMS **convert random writes into SEQUENTIAL writes CONCURRENTLY** .

---
### Write-Ahead Log 
Maintain a **log file separate from data files** that contains the changes that transactions make to database.
- Assume that the log is on **stable storage**
- Log contains **enough information** to perform the necessary **undo and redo** actions to restore the database
- Only need to **record changes of records**, instead of writing shadow pages
- Capitalizes on sequential I/Os since logs are adjacent to each other on disk

DBMS must write to disk the log file records that correspond to changes made to a database object **BEFORE** it can **flush that object to disk**.

This would be a **steal and no-force** policy. This is known as the **Wal Protocol**.

#### Wal Protocol 
The DBMS **stages all a transaction's log** records in **volatile storage** (usually backed by buffer pool). All log records pertaining to an updated page are written to non-volatile storage **before the page itself is over-written** in non-volatile storage.

A transaction is not considered committed until all its log records have been written to stable storage.