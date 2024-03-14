[[2024-03-12]] #Database 

### Table Indices 
A table index is a **replica** of a **subset of a table's attributes** that are **organized and/or sorted for efficient** access using those attributes.

The DBMS ensures that the contents of the table and the index are **logically synchronized**, and it is the DBMS's job to figure out the best indices to use to execute each query.

A tree is the default data structure of an index-based database.

```ad-note
There is a trade-off regarding the number of indexes to create per database and.
- Storage overhead, and 
- Maintenance overhead
```

### B+ Tree 
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
- The keys are derived from the **attribute (s) that the index is based on**
	- Inner nodes: separators
	- Leaf node: keys of the tuple
- The values will differ based on whether the node is classified as an **inner node** or a **leaf node**
	- Inner nodes: pointers to child nodes
	- Leaf nodes: the tuple

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