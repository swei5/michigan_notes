[[2024-03-12]] #Database 

### Table Indices 
A table index is a replica of a **subset of a table's attributes** that are **organized and/or sorted for efficient** access using those attributes.

The DBMS ensures that the contents of the table and the index are logically synchronized, and it is the DBMS's job to figure out the best indices to use to execute each query.

```ad-note
There is a trade-off regarding the number of indexes to create per database and.
- Storage overhead, and 
- Maintenance overhead
```

### B+ Tree 
A **B+Tree** is an $M$ -way, **self-balancing** tree data structure that keeps data sorted and allows searches, insertions, and deletions in $O(\log n)$. 
- Generalization of a binary search tree, since a node can have more than two children
- Optimized for systems that read and write large blocks of data

It has the following properties:
- Perfectly balanced (every leaf node is at the same depth)
- Every node other than the root is at least half-full: $\frac{M}{2}-1 \le n (\text{keys}) \le M-1$
- Every inner node with $k$ keys has $k+1$ **non-null children**