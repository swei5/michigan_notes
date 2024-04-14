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

There are two phases:
1. Growing
	- Each transaction **requests the locks** that it needs from the DBMS’s lock manager
	- The lock manager 
		- Grants lock requests, or
		- Denies lock requests
1. Shrinking
	- The transaction is **allowed to only release** locks that it previously acquired. It cannot acquire new locks
	- **CANNOT acquire more locks** once in this phase

![[Pasted image 20240413225119.png|400]]



### Deadlock Detection + Prevention

### Isolation Levels
