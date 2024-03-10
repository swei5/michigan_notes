[[2024-03-07]] #Database 

Today, we are now going to talk about how DBMS's execution engine supports **read/write** data from pages. There are two types of data structures used within the internal storage of DBMS:
- Hash tables
- Trees

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

#### Static Hash Table 
To create a static hash table, we allocate a giant array that has **one slot for EVERY element** you need to store. This requires the following:
1. We know the number of elements ahead of time 
2. Each key is **unique**
3. We have a hash function $h$ such that $i \ne j \to h (i) \ne h(j)$

There are two sets of design decision we need to account for:
1. Hash Function 
	- How to **map a large key space into a smaller domain**
	- Trade-off between being fast vs. collision rate
1. Hashing Scheme
	- How to **handle key collisions** after hashing
	- Trade-off between allocating a large hash table vs. Additional instructions to find/insert keys 

#### Hash Functions 
We **do NOT** want to use a **cryptographic hash function** for DBMS hash tables (e.g., SHA-2), as we want something that is **fast** and has a **low collision rate**.

#### Static Hashing Schemes 
##### Linear Probe Hashing 
Resolve collisions by linearly searching for the **next free slot** in the table.
- Python `dict`
- Single giant table of slots

To determine whether an **element is present**, **hash to a location in the index** and scan for it, and we **must store the key in the index** to know when **to stop scanning**.
- Insertions and deletions are **generalizations of lookups**

```ad-example
**Linear Probe Hashing, Insertion**

![[Pasted image 20240309200327.png|500]]
```

When it comes to deletion, there are two approaches:
1. Tombstone
	- Mark the node as `DEAD` after deleting a value from it; then 
	- When hashed into a `DEAD` node, look past it until found

![[Pasted image 20240309201453.png|300]]

2. Movement 

##### Robinhood Hashing 
This is a variant of linear probe hashing that steals slots from "rich" keys and give them to "poor" keys. Each key tracks the **number of positions they are from where its optimal position** in the table, and on insert, a key takes the slot of another key **if the first key is farther away from its optimal position than the second key**.

#### Non-unique Keys 
In case we have multiple values that associate with the same key (duplicate keys) in the same hash tables, we have two choices:
1. Separate linked list
	- Store values in separate storage area for each key

![[Pasted image 20240309202003.png|300]]

1. Redundant Keys
	- Store duplicate keys entries together in the hash table
	- Easier to implement and what most systems do