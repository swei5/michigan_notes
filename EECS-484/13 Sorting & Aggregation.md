[[2024-03-16]] #Database #DBMS #System 

In this chapter, we will be covering how to **execute queries** using the DBMS components we have discussed so far.

```ad-todo
Roadmap of DBMS:
- ~~Disk Manager~~
- ~~Access Methods~~
- **Operator Execution** 
- Query Planning 
- Transaction Manager 
- Log Manager
```

Conceptually, we have come to understand how operations are performed using the framework of [[5 Relational Algebra|relational algebra]].

In reality, in a disk-oriented DBMS, just like it cannot assume that a table fits entirely in memory, a **disk-oriented DBMS CANNOT assume that query results fit in memory**.
- We care a lot about computation and I/O cost

We are going to rely on the **buffer pool** to implement algorithms that need to spill to disk, and as always, preferring **algorithms that maximize the amount of sequential I/O**.

### External Merge Sort
Relational model/SQL is **UNSORTED**. However, at times, queries may request that tuples are sorted in a specific way (`ORDER BY`). We may also want the results to be sorted for the following reasons 
- Trivial to support duplicate elimination (`DISTINCT`)
- **[[12 Tree Indices#^dc4c4c|Bulk loading]]** sorted tuples into a B+Tree index is faster
- Aggregations (`GROUP BY`)

If data does not fit in memory (usually in DBMS), then we need to use a technique that is aware of the cost of reading and writing disk pages.
- If data fits in memory, can go with any efficient sorting algorithm

**External Merge Sort** is a divide-and-conquer algorithm that **splits data into separate runs**, **sorts them individually**, and then **combines them into longer sorted runs**.
- External means "interacting with the disk"
- Similar to the standard merge sort algorithm 

A run is a **list of key/value pairs**.

#### Sorting
The first phase: sort chunks of data that fit in memory and then write back the sorted chunks to a file on disk.

In sorting, a **key** is simply the **attribute(s)** to compare to compute the sort order. However, we have two choices with **values**: ^13b71e
1. Tuple (early materialization)
	- Storing the actual tuple
	- More read/write during sorting but more sequential I/Os when accessing after sort
1. Record ID (late materialization)
	- Storing a pointer to the tuple
	- Much less read/write during sorting but more random I/Os when accessing after sort

![[Pasted image 20240316213115.png|400]]

#### 2-Way External Merge Sorts 

```ad-example
**Example: 2-Way External Merge Sort**

Let's look at the big picture. We can break down this algorithm into 2 separate passes:

**Pass 0**: Read pages of the table into memory, one at a time, and **sort** the page into a run and **write it back** to disk.
- The reason we don't overwrite the original pages is the sorted runs are **intermediate steps**, not something we attempt to permanently store

![[Pasted image 20240316213943.png|400]]

**Pass 1-N**: Recursively **merge pairs of runs** into runs twice as long. We would need **three buffer pages** (2 for inputs and 1 for output).

![[Pasted image 20240316214054.png|400]]

A more concrete visualization (each element is a page):

![[Pasted image 20240316214512.png|400]]

```

The total number of passes is $1+ \lceil \log N \rceil$ for a database with $N$ pages.  Since in each pass, we read and write every page in the file, the total I/O cost is $2N \cdot n(\text{Pass})$.

We observe that this algorithm only requires three buffer pool pages to perform the sorting, which is not taking advantage of our **memory**.

What we can do to improve is called **double buffering optimization**: **prefetch the next run in the background** and store it in a second buffer while the system is processing the current run.
- **Reduces the wait time for I/O** requests at each step by continuously utilizing the disk
- We effectively save one unit of **input cost**

![[Pasted image 20240316215428.png|400]]

We can generalize this into a more efficient external merge sort. Assume the DBMS has a finite number of $B$ **buffer pool pages** to hold input and output data. 

In **Pass 0**, we read $B$ pages of the table into memory at a time and sort them and write back to disk.
- Produce $\lceil\frac{N}{B}\rceil$ sorted runs of size $B$

In **Pass 1 and onwards**, we merge $B-1$ runs

Hence, the total number of passes is $1+ \lceil \log_{B-1} \lceil \frac{N}{B}\rceil \rceil$ for a database with $N$ pages, and the total I/O cost is still $2N \cdot n(\text{Pass})$. ^b4cf7d

---
### B+Tree for Sorting 
If the table that must be sorted **already has a B+Tree index on the sort attribute**(s), then we can use that to accelerate sorting. As discussed [[12 Tree Indices#^b81d32|previously]], retrieving tuples in desired sort order by **simply traversing the leaf pages** of the tree:
- Clustered B+Tree 
	- Better than external sorting because there is **NO computational cost**
	- All disk access is sequential
- Unclustered B+Tree 
	- Constantly chase each pointer to the page that contains the data
	- Almost always a bad idea: computational cost is $N$ random I/Os, might as well do external sort

---
### Aggregations
Aggregation means to collapse values for a single attribute from **multiple tuples into a single scalar value**. The DBMS needs a way to quickly find tuples with the specific properties used to aggregate the table.

There are two major design choices:
1. Sorting 
2. Hashing 

In many cases, hashing will be the cheaper method ($O(1)$ look up). We don't need the data to be sorted this way (without `ORDER BY`).
- `GROUP BY`
- `DISTINCT`

```ad-example
**Example: Sorting Aggregation**

![[Pasted image 20240316222538.png|500]]
```

#### Hashing Aggregation 
To populate an ephemeral hash table as the DBMS scans the table. For each record, check **whether there is already an entry** in the hash table.
- `DISTINCT`: discard duplicate 
- `GROUP BY`: perform aggregate computation 

When data does **NOT fit in memory**, the DBMS has to spill data to disk and use something called **external hashing aggregate**.

This algorithm contains 2 phases (similar structure to external merge sort):
1. **Partition**: we use hash function $h_{1}$ to split tuples into partitions on disk
	- Divide tuples into partitions based on the **same hash values**
	- Write them out to disk when they get full 
	- $B-1$ buffers to hold partitions and $1$ buffer for the input data
1. **Re-hash**: for each partition on disk, read it into memory and build an in-memory hash using second hash function $h_{2}$
	- Build in-memory hash table for each partition and compute the aggregation
	- This assumes the partition fits in memory

```ad-warning
In phase 1, we **DO NOT** build hash tables.

In phase 2, we **DO NOT** need to store hash tables built for each partition in disk. We simply push the entries into the final result set.
- Besides the hash key, we would also need to store a r**unning value** for the aggregate functions in the final output
		- E.g. `AVG()` needs `(COUNT, SUM)` to perform
```

```ad-example
**Example: Hashing Aggregation**

![[Pasted image 20240316224152.png|500]]

![[Pasted image 20240316224213.png|500]]
```
