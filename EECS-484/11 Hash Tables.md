[[2024-03-07]] #Database #DBMS 

Today, we are now going to talk about how DBMS's execution engine supports **read/write** data from pages. There are two types of data structures used within the internal storage of DBMS:
- **Hash tables**
- Trees

A proper data structure should include the following. 
- **Internal meta-data**
- **Core data storage** 
- **Temporary data structures** 
- Table indexes

Hash tables are primarily utilized in the first three.

Likewise, the accessing methods should prioritize **sequential access** and should be adaptive to **DBMS-specific operations**.

```ad-todo
Roadmap of DBMS:
- ~~Disk Manager~~
- **Access Methods** 
- Operator Execution 
- Query Planning 
- Transaction Manager 
- Log Manager
```

### Hash Tables
A hash table implements an **unordered associative array** that maps keys to values. It uses a **hash function** to compute an offset into the array for a given key, from which the desired value can be found.
- Space complexity: $O(n)$
- Time complexity: $O(1)$

In DBMS, constants are also important. We would prefer $O(1n)$ a lot better than $O(1000n)$.

#### Static Hash Table 
To create a static hash table, we allocate a giant array that has **one slot for EVERY element** you need to store. 

There are two sets of design decision we need to account for:
1. Hash Function 
	- How to **map a large key space into a smaller domain**
	- Trade-off between being fast vs. collision rate
1. Hashing Scheme
	- How to **handle key collisions** after hashing
	- Trade-off between allocating a large hash table vs. Additional instructions to find/insert keys 

#### Hash Functions 
We **do NOT** want to use a **cryptographic hash function** for DBMS hash tables (e.g., SHA-2), as we want something that is **fast** and has a **low collision rate**. 

Hash functions are generally developed by large organizations and we would only need to pick the ones that suit our need.

#### Static Hashing Schemes 
The hash tables require the DBMS to know the **number of elements it wants to store**. Otherwise, it must rebuild the table if it needs to grow/shrink in size.
##### Linear Probe Hashing 
Resolve collisions by linearly searching for the **next free slot** in the table.
- Python `dict`
- Single giant table of slots

To determine whether an **element is present**, **hash to a location in the index** and scan for it, and we **must store the key in the index** to know when **to stop scanning**.
- If element existed, scan linearly
- Insertions and deletions are **generalizations of lookups**

```ad-example
**Linear Probe Hashing, Insertion**

![[Pasted image 20240309200327.png|500]]
```

When it comes to deletion, there are two approaches:
1. Tombstone
	- **Allocate a bit** to every slot to represent the tombstone status
	- Mark the node as `DEAD` after deleting a value from it; then 
	- When hashed into a `DEAD` node, look past it until found
	- Insert into a `DEAD` location then remove it 

![[Pasted image 20240309201453.png|300]]

2. Movement 
	- Re-insertion of all existing values 
	- Expensive as re-insertion is linear time

##### Robinhood Hashing 
This is a variant of linear probe hashing that steals slots from "rich" keys and give them to "poor" keys. Each key tracks the **number of positions they are from where its optimal position** in the table, and on insert, a key takes the slot of another key **if the first key is farther away from its optimal position than the second key**.

```ad-example
**Robinhood Hashing, Insertion**

![[Pasted image 20240309202909.png|500]]

In this example, `E` was hashed into where `A` is. However, both are 0 position away from its optimal position relative to the current position, hence `E` does not get the replace `A`. Same goes with `C`, which was hashed to `A` but then moved to where it is similarly.

However, when `E` moved one further down to where it is (where `D` was), it is now 2 positions away from its optimal position whereas `D` was just 1 position away (`D[1] < E[2]`), it replaced `D` in that position.
```

##### Cuckoo Hashing 
It uses multiple hash tables with different hash function seeds. On insert, check every table and pick anyone that has a **free slot.** If no table has a free slot, **evict the element from one of them and then re-hash** it find a new location.

Look-ups and deletions are **ALWAYS** $O(1)$ because **only one location per hash table** is checked.

```ad-example
**Cuckoo Hashing, Insertion**

![[Pasted image 20240309203531.png|500]]

In this example, after we successfully hashed `A` and `B` into the hash tables, hash results of `C` collided with that of both `A` and `B`. Hence, the algorithm lets us replace any element from the conflicted slots with `C`. Then, the next step would be to rehash `B`, which was replaced, to a different spot. 

If all other alternatives for `B` are also occupied, we continue the process with `B` until all keys are hashed without collision.
```

#### Non-unique Keys 

^c3fb2c

In case we have multiple values that associate with the same key (duplicate keys) in the same hash tables, we have two choices:
1. Separate linked list
	- Store values in separate storage area for each key

![[Pasted image 20240309202003.png|300]]

1. Redundant Keys
	- Store duplicate keys entries together in the hash table
		- If same keys found but with different values, continue linear search
	- Easier to implement and what most systems do

#### Dynamic Hashing Schemes 
Dynamic hash tables **resize themselves on demand**.

##### Chained Hashing
It maintains a linked list of buckets for each slot in the hash table and resolves collisions by **placing all elements** with the **same hash key into the same bucket**.
- Used in Java `HashMap` class

To determine whether an element is present, hash to its bucket and scan for it, and **Insertions and deletions are generalizations of lookups**.

```ad-example
**Chained Hashing, Insertion**

![[Pasted image 20240309205201.png|500]]
```

##### Extendible Hashing 
A chained-hashing approach where we **split buckets** instead of letting the linked list grow forever. Data movement is localized to just the split chain.
- Multiple slot locations can point to the same bucket chain
- **Reshuffle bucket entries on split** and increase the number of bits to examine

```ad-example
**Extendible Hashing, Insertion**

![[Pasted image 20240309211002.png|500]]

In the example above, numbers in the blue cells represent the number of bits a slot examines (differentiates by). 

Suppose we all keys are hashed into valid positions and we are attempting to hash `C`, which will be hashed into a bucket that is full. Now, to create a vacant spot for the new entry, we need to reshuffle our global bucket entries and increase the number of examined bits to 3.

![[Pasted image 20240309211321.png|500]]
```

##### Linear Hashing 
In linear hashing, the hash table maintains a **pointer that tracks the next bucket to split**. When **ANY** bucket overflows, **split** the bucket at the pointer location.

We use **multiple hashes** to find the **right bucket for a given key**. We can also use different overflow criterion:
- Space utilization
- Average Length of Overflow Chains

Splitting buckets based on the split pointer will eventually get to all overflowed buckets. When the **pointer reaches the last slot**, **delete the first hash function** and move back to beginning.

```ad-example
**Linear Hashing, Insertion**

![[Pasted image 20240309220902.png|500]]

Imagine the scenario above where we attempted to hash 17 - this caused the hashed bucket (1) to overflow. Then, we splitted bucket 0, where the split pointer then pointed to.

![[Pasted image 20240309221412.png|500]]

To split, we applied a different hashing function: `hash2` to all values in original bucket 0, which causes value `20` to pivot to now bucket 4. After that, the split pointer moved 1 position downward to bucket 1. 

From now on, for all buckets that are above the split pointer (i.e. bucket 0), we will be using the new hash function `hash2` to compute its hash value.

```
