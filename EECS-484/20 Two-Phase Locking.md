[[2024-04-13]] #Database #DBMS 

We need a way to guarantee that all execution schedules are correct (i.e., serializable) without knowing the entire schedule **AHEAD of time**.
- Different from theoretical analysis done in previous lecture

The solution is to use **LOCKs** to protect different operations on DBMS accordingly that guarantees serializability.

A naive lock looks like below: the lock effective blocks any possible operations from other transactions the time when the lock is granted until it is released.

![[Pasted image 20240413224151.png|400]]

---
### Lock Types
There are two types:
- **S-LOCK**: Shared locks for reads
- **X-LOCK**: Exclusive locks for writes

![[Pasted image 20240413224353.png|300]]

**Transactions** **request** locks (or upgrades).
- S-Lock for reading (or can upgrade it to an X-Lock)
- X-Lock for writing

**Lock manager grants** or blocks requests. Finally, **transactions release locks**.

Lock manager updates its internal lock-table.
- It keeps track of what transactions hold what locks and what transactions are waiting to acquire any locks (metadata)

Locks **DO NOT guarantee** serializability, if we only grant and reducing them when we need to access or modify records in a naive way:

![[Pasted image 20240413224901.png|400]]

### Two-Phase Locking
**Two-phase locking** (2PL) is a concurrency control protocol that determines whether a transaction can access an object in the database **on the fly**.
- The protocol **DOES NOT** need to know all the queries that a transaction will execute ahead of time

There are two phases, for **EACH transaction**:
1. Growing
	- Each transaction **requests the locks** that it needs from the DBMS’s lock manager
	- The lock manager 
		- Grants lock requests, or
		- Denies lock requests
1. Shrinking
	- The transaction is **allowed to only release** locks that it previously acquired. It cannot acquire new locks
	- **CANNOT acquire more locks** once in this phase

![[Pasted image 20240413225119.png|400]]

2PL on its own is sufficient to **guarantee conflict serializability**.
- It generates schedules whose precedence graph is acyclic
	- As we don't have release/acquire scenarios again (?)
- However, subject to **cascading aborts**

![[Pasted image 20240413232310.png|200]]

In this example above, if $T_{1}$ aborts, $T_{2}$ should be aborted as well ASAP. If $T_{2}$ does not abort, it will be performing operations on wasted data. 
- Any information about $T_1$ cannot be leaked to the outside world
- However, this is permissible by 2 PL but does not make sense to applications

```ad-note
2PL is **one version of the many** protocols that allow for conflict serializability, but it **does NOT exhaustively** allow all serializable schedules.
- May still have "dirty reads" due to cascading aborts
	- Solution: **Strong Strict 2PL** (aka Rigorous 2PL)
- May lead to deadlocks
	- Solution: Detection or Prevention
```

#### Strong Strict Two-Phase Locking 
The transaction is **ONLY allowed to release locks after is has ended**, i.e., committed or aborted. Allows only conflict serializable schedules, but it is often conservative and stronger than needed for some apps.

![[Pasted image 20240413232925.png|400]]

A schedule is **strict** if a value written by a transaction is **NOT read or overwritten** by other transaction until that transaction **finishes**.
- Does not incur cascading aborts 
- Aborted transaction can be undone by just **restoring original values of modified tuples** (of the current transaction)

![[Pasted image 20240413233517.png|400]]

From this we further expanded our universe of schedules:

![[Pasted image 20240413235234.png|400]]

Note that there exists **some conflict serializable** schedules that are **NOT cascading aborts** and **NOT strict** - that said, for some transactions we don't need to employ strong strict 2PL to guarantee this property.

---
### Deadlock
A **deadlock** is a **cycle of transactions waiting for locks** to be released by each other.

```ad-example
**Example, Deadlock**

![[Pasted image 20240415104506.png|500]]

In this example above, we have a deadlock scenario because the $T_{2}$ will have to wait for $T_{1}$ to grant permission for `Lock(A)`, which could not take place if $T_{1}$ has not granted permission for `Lock(B)` which was locked granted to $T_{2}$.
```

There are two ways to deal with deadlocks:
1. Deadlock detection 
2. Deadlock prevention 

#### Deadlock Detection 
The DBMS creates a **waits-for graph** to keep track of what locks each transaction is waiting to acquire.
- Similar to a dependency graph 
- Nodes are transaction
- Directed

Edge from $T_i$ to $T_j$ if $T_i$ is **waiting** for $T_j$ to release a lock.

The system periodically **checks for cycles** in waits-for graph and then decides how to break it.

```ad-example
![[Pasted image 20240415105157.png|500]]
```

#### Deadlock Prevention 
When the DBMS detects a deadlock, it will **select a "victim" transaction to rollback** to break the cycle.
- **DOES NOT require** a waits-for graph
- Victim will either **restart or abort** (more common) depending on how it was invoked

There is a **trade-off** between the **frequency of checking** for deadlocks and **how long transactions have to wait** before deadlocks are broken.

Selecting the proper victim depends on a lot of different variables. We use various heuristics.
- By age (lowest timestamp)
	- Oldest transaction may have acquired the **most number of locks**
- By progress (least/most queries executed)
	- Least queries: to prevent wasting work
- By the number of items already locked
- By the number of transactions that we have to rollback with it

We also should consider the number of **times a transaction has been restarted** in the past to prevent starvation.

Based on the priority of transactions, there are two schemes:
- **Wait-Die** ("Old Waits for Young")
	- If requesting transaction (acquiring locks) has higher priority than holding transaction, then **requesting transaction** **waits for holding transaction**
	- Otherwise requesting transaction aborts
- **Wound-Wait** ("Young Waits for Old")
	- If requesting transaction has higher priority than holding transaction, then **holding transaction aborts and releases lock**
	- Otherwise requesting transaction waits

```ad-example
**Example, Wait-Die and Wound-Wait**

![[Pasted image 20240415132646.png|500]]
```

These schemes guarantee **NO deadlocks** because only **one "type" of direction is allowed** when waiting for a lock.
#### Rollback 
After selecting a victim transaction to abort, the DBMS can also decide on **how far to rollback** the transaction's changes.
- Completely
- Minimally
	- Theoretically possible but difficult to implement

---
### Isolation Levels
Enforcing serializability may allow **too little concurrency and limit performance**. We may want to use a weaker level of consistency to improve scalability.

An isolation level controls **the extent that a transaction is exposed to the actions of other concurrent transactions**. It provides for greater concurrency at the cost of exposing transactions to **uncommitted changes**:
- Dirty Reads (W-R conflict)
- Unrepeatable Reads (R-W conflict)
- Phantom Reads (Unprotected Inserts/Deletes)

The levels (from most strict to most loose) are:
1. **SERIALIZABLE**: No phantoms, all reads repeatable, no dirty reads
2. **REPEATABLE READS**: Phantoms may happen
3. **READ COMMITTED**: Phantoms and unrepeatable reads may happen
4. **READ UNCOMMITTED**: All of them may happen

```ad-summary
**Isolation Levels, Summary**

![[Pasted image 20240415134144.png|500]]
```

You set a transaction's isolation level before you execute any queries in that transaction in some SQL languages: `SET TRANSACTION ISOLATION LEVEL <isolation-level>;`.

**NOT ALL** DBMS support **all isolation levels** in all execution scenarios
- Default depends on implementations: only a few uses serializable schedule in fact

We can provide hints to the DBMS about whether a transaction will **modify** the database during its lifetime:  `SET TRANSACTION <access-mode>;` Only has two states:
- `READ WRITE` (Default)
- `READ ONLY`

Not all DBMSs will optimize execution if you set a transaction to in `READ ONLY` mode.
