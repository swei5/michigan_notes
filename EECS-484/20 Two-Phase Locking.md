[[2024-04-13]] #Database #DBMS 

We need a way to guarantee that all execution schedules are correct (i.e., serializable) without knowing the entire schedule **AHEAD of time**.
- Different from theoretical analysis done in previous lecture

The solution is to use **LOCKs** to protect different operations on DBMS accordingly that guarantees serializability.

A naive lock looks like below: the lock effective blocks any possible operations from other transactions the time when the lock is granted until it is released.

![[Pasted image 20240413224151.png|400]]

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
### Deadlock Detection + Prevention


### Isolation Levels
