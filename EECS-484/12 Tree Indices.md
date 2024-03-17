[[2024-03-12]] #Database 

```ad-todo
Roadmap of DBMS:
- ~~Disk Manager~~
- **Access Methods** 
- Operator Execution 
- Query Planning 
- Transaction Manager 
- Log Manager
```
### Table Indices 
A table index is a **replica** of a **subset of a table's attributes** that are **organized and/or sorted for efficient** access using those attributes.

The DBMS ensures that the contents of the table and the index are **logically synchronized**, and it is the DBMS's job to figure out the best indices to use to execute each query.

A tree is the default data structure of an index-based database.

```ad-note
There is a trade-off regarding the number of indexes to create per database and.
- Storage overhead, and 
- Maintenance overhead
```

### B+Tree 
B-Tree is a family of **balanced tree data structures**. DBMS almost always use some kinds of variants of it. 

A **B+Tree** is an $M$ -way, **self-balancing** tree data structure that keeps data sorted and allows searches, insertions, and deletions in $O(\log n)$. 
- Generalization of a binary search tree, since a node can have more than two children
- Optimized for systems that read and write large blocks of data

It has the following properties:
- A node can have **AT MOST** $M$ children
- Perfectly balanced (every leaf node is at the same depth)
- Every node other than the root is at least half-full: $\frac{M}{2}-1 \le n (\text{keys}) \le M-1$
- Every inner node with $k$ keys has $k+1$ **non-null children**

![[Pasted image 20240314140244.png|500]]

Every B+Tree node is comprised of an **array of key/value pairs**.
- The **keys** are derived from the **attribute (s) that the index is based on**
	- Inner nodes: separators
	- Leaf node: keys of the tuple
- The **values** will differ based on whether the node is classified as an **inner node** or a **leaf node**
	- Inner nodes: pointers to child nodes
	- Leaf nodes: the tuple

Keys must be of the **same type**.

A node also has **sibling pointers** that point to the adjacent nodes on the same layer to accelerate and optimize for accessing. Each node can be viewed as a **page** if we use a disk structure.

The arrays are (usually) kept in **sorted** key order.

#### Leaf Node Organization 
There are two ways in which we can organize leaf nodes' structure. The first way is to store keys and values in an **alternating way**: `(k0,v0,k1,v1,...,vn)`. 

![[Pasted image 20240314141821.png|500]]

The second way is similar to the [[10 Database Storage#^3deea0|slotted arrays]] we previously mentioned in database storage. We would keep the **meta data** separately and keep an array of **sorted keys and corresponding values**.

![[Pasted image 20240314141952.png|500]]

#### Leaf Node Value
There are two approaches as well: 
1. **Record ID**
	- We store **a pointer to the location of the tuple** to which the index entry corresponds
2. **Tuple Data**
	- The leaf nodes (of the **primary key index**) store the actual contents of the tuple.
		- Index must be primary key to avoid duplication 
	- **Secondary indexes MUST store the Record ID** as their values

```ad-summary
**B-Tree VS. B+Tree**
- The original B-Tree from 1972 stored **keys and values in ALL nodes** in the tree.
	- More space-efficient, since each key only appears once in the tree
- A B+Tree only stores **values in leaf nodes**
	-  Inner nodes only guide the search process
```

#### Insertion 
As with most other data structures, equivalent to **finding** the correct leaf node $L$.
- If doesn't exist, we put data entry into $L$ in sorted order
	- If $L$ has enough space, OK
	- Else 
		- Redistribute a key to sibling node first - split $L$ 's keys into $L$ and a new node $L_{2}$, OR
			- Smallest key to the left and largest key to the right 
			- Then readjust separator
		- Redistribute entries evenly, copy and push up the **middle key** to parent's layer (separator)
			- Insert index entry pointing to $L_2$ from parent of $L$ (the new separator)
				- If parent is full again, repeat (split parent)
- Else, OK

#### Deletion
- Start at root, find leaf $L$ where entry belongs and remove
	- If $L$ is at least half full, OK
	- Else, re-distribute, borrowing from sibling
		- If re-distribution fails, **merge $L$ and sibling**, and delete entry and separator (pointing to $L$ or sibling) from parent of $L$

#### Duplicate Keys
Approach 1: **Append Record ID**
- Add the tuple's unique Record ID as part of the key to ensure that all keys are unique
- The DBMS can still use **partial keys** to find tuples

![[Pasted image 20240314151807.png|500]]
Approach 2: **Overflow Leaf Nodes**
- Allow leaf nodes to **spill into overflow nodes** that contain the duplicate keys
- This is more complex to maintain and modify

![[Pasted image 20240314152003.png|500]]

---
### Selection Conditions 
The DBMS can use a B+Tree index if the query provides **ANY of the attributes of the search key**.
- E.g. if we index on `<a,b>`
	- `(a=5 AND b=3)` is supported
	- `(b=3)` is supported

![[Pasted image 20240314151131.png|500]]

Not all DBMSs support this.

```ad-note
For a hash index, we must have **ALL attributes** in search key.
```

---
### Clustered Indices 

^b81d32

The table is stored in the **sorted order specified by the primary key.**  It **reorders** the way records in the table are **physically stored**.
- Can be either heap- or index-organized storage
- So that we can perform sequential scan

Some DBMSs always use a clustered index.
- If a table does not contain a primary key, the DBMS will automatically make a **hidden primary key**

With a B+Tree, we are able to achieve in-order traversal by traversing to the left-most leaf page to the right and then retrieve **tuples from all leaf pages**.
- This is **ALWAYS better** than external sorting

In Oracle, we can create an index by

```sql
CREATE INDEX <name> ON
<table> USING [BTREE|HASH] (<attr>);
```

B+Tree is the **default** index type of most DBMS. 

![[Pasted image 20240314152644.png|400]]

Conversely, retrieving tuples in the order they appear in a **non-clustered index can be very inefficient**, if data on the disk is **NOT SORTED** according to the index.
- Has to visit pages multiple times
- Taking lot of storage in buffer pool

![[Pasted image 20240314152917.png|400]]

The DBMS can first figure out **all the tuples that it needs** and then **sort** them based on their Page ID (or record ID). Then, we can remap them back to the original order if we want retrieved data to be sorted.
- This way, we are still prioritizing sequential read

```ad-note
**Sequential Scan and Index Scan**

Though index scan seems to be more efficient, at times we want to use a sequential scan when we are reading a larger portion of the database - in that case, we don't care as much about the ordering because we would have to read most of the data anyway.

In the case that we need only to read a small partition of data, index scan is more efficient as it's built to help find and read data quickly.
```

```ad-info
To get the tuple identifier in Oracle, we can select field `ctid`.
```

Clusters are **NOT incrementally updated** - that said, the cluster will not maintain its order after updates or deletes to the table.

---
### B+Tree Design Choices 
Four primary design choices are 
- Node Size
- Merge Threshold 
- Variable-Length keys
- Intra-Node Search 

#### Node Size 
The **slower the storage device**, the **larger the optimal node size** for a B+Tree.
- HDD: ~1 MB
- SSD: ~10 KB
- In-Memory: ~512B

Optimal sizes can vary **depending on the workload**.
- **Sequential scan** (lots of leaf-node scan): better to have larger node size
- **Small lookup** (lots of root-to-leaf traversal): better to have smaller node size

#### Merge Threshold 
Some DBMSs **do NOT always merge** nodes when they are half full.
- Delaying a merge operation may reduce the amount of reorganization
- It may also be better to just let smaller nodes exist and then periodically rebuild entire tree
	- With the expectation that more keys will be added

#### Variable-Length Keys
1. **Pointers** 
	- Store the keys as **pointers to the tuple’s attribute** 
		- Costly because of massive number of look ups
1. **Variable-Length Nodes** 
	- The size of each node in the index can vary
	- Requires careful memory management
2. **Padding** 
	- Always pad the key to be **max length of the key type**
3. **Key Map / Indirection**
	- Embed an array of pointers that map to the **key/value list** within the node
	- Used when the number of unique **keys** is small so that we can efficiently build a dictionary of it

#### Intra-Node Search 
1. **Linear**
	- Scan node keys from beginning to end
2. **Binary**
	- Jump to middle key, pivot left/right depending on comparison 
3. **Interpolation**
	- Approximate location of desired key based on **known distribution** of keys

![[Pasted image 20240316174420.png|500]]

### Optimization 
#### Prefix Compression 
**Sorted keys** in the same leaf node are likely to have the **same prefix**. Hence, instead of storing the entire key each time, extract common prefix and **store only UNIQUE SUFFIX** for each key.

![[Pasted image 20240316205241.png|400]]

There are many variations to this method.

#### Duplication 
Non-unique indexes can end up storing multiple copies of the same key in leaf nodes. The leaf node can **store the key once and then maintain a list of tuples with that key** (similar to [[11 Hash Tables#^c3fb2c|what we discussed]] for hash tables).

![[Pasted image 20240316205420.png|400]]

#### Bulk Insert 

^dc4c4c

Intuitively, the fastest way to build a new B+Tree for an existing table is to **first sort the keys** and then **build the index from the bottom up**.
