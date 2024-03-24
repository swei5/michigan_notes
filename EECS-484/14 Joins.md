[[2024-03-23]] #Database #DBMS 

```ad-todo
Roadmap of DBMS:
- ~~Disk Manager~~
- ~~Access Methods~~
- **Operator Execution** 
- Query Planning 
- Transaction Manager 
- Log Manager
```
### Join
We use **join operators** to reconstruct the original tuples without any information loss to allow for **[[8 Normalization|normalization]]**. 

In this lecture, we will focus on performing **binary joins** using **equijoin algorithms**.
- These algorithms can be tweaked to support other joins

```ad-note
We want the **smaller table** to always be the **left table** ("outer table") in the query plan. More on this later.
```

```ad-example
**Example: Query Plan**

We can visualize a query plan in form of a tree:

![[Pasted image 20240323163244.png|200]]

In this example, data flows from the leaves of the tree up towards the root, and the output of the root node is the result of the query.
```

In terms of join operators, there are two primary considerations:
1. **[[#Join Output|Output]]** 
	- **What data does the join operator emit** to its parent operator in the query plan tree?
2. **[[#Cost Analysis|Cost Analysis Criteria]]**

#### Join Output 
For tuple $r \in R$ and tuple $s \in S$ that match on join attributes, **concatenate $r$ and $s$ together into a new tuple**. The output contents can vary, depending on:
- Processing model
- Storage model 
- Data requirements in query

Like [[13 Sorting & Aggregation#^13b71e|sorting]], we have two ways of arranging outputs: ^7c7a46
1. **Early Materialization**
	- Copy the values for the attributes in outer and inner tuples into a **new output tuple**
	- Then, subsequent operators in the query plan **NEVER** need to go back to the **base tables** to get more data
		- However, redundant information may be read

![[Pasted image 20240323164011.png|300]]

2. **Late Materialization**
	- Only copy the **joins keys** along with the **Record IDs** of the matching tuples
	- Ideal for [[10 Database Storage#Decomposition Storage Model (DSM)|column store]] because the DBMS **DOES NOT copy** data that is not needed for the query, saving some I/O cost
		- However, not optimal for row store (as we have to access the entire page or tuples anyways , doing early materialization makes more sense)

![[Pasted image 20240323164421.png|300]]

#### Cost Analysis 
Assume that there are $M$ pages and $m$ tuples in $R$; $N$ pages and $n$ tuples in $S$. We define the **cost metric** as the **number of I/Os** to compute join.
- We will ignore output costs since that depends on the data and we cannot compute that yet
- CPU operations don't count, we **ONLY** care about I/Os from the disk

```ad-note
**Join v.s. Cross-Product**

$R \bowtie S$ is the most common operation and thus must be **carefully optimized**.

$R \times S$ followed by a selection is **inefficient** because the cross-product is large.

Join is strongly preferred over cross product.
```

There are many algorithms for reducing join cost, but no algorithm works well in all scenarios.

---
### Join Algorithms 
We will cover three main types:
- **[[#Nested Loop Joins|Nested Loop Joins]]** 
- **[[#Sort-Merge Joins|Sort-Merge Joins]]** 
- **[[#Hash Joins|Hash Joins]]** 

Like in sorting, we only care about cost if the database **DOES NOT** fit into memory; if it does, the cost of operation is really negligible. The algorithms below are all based on the assumption that we are handling **larger databases**.
#### Nested Loop Joins
##### Simple Join
The vanilla version follows the algorithm:

```javascript
// Cost of reading R: M
forEach r in R: // Outer m times
	for each s in S: // Inner Cost of reading S: N
		emit if r and s match
```

For join keys $r, s$.

###### Cost
This is stupid, because we need to scan $S$ once for every $r \in R$ . Total cost is $$M+(m \cdot N)$$ where $M$ is the cost for scanning $R$ and $N$ is the cost for scanning $S$.

Albeit the inefficiency, this shows us why we would want the **smaller table** to be on the LHS: $$N+(n\cdot M) < M+(m \cdot N) \text{ for } n<m, N<M$$
##### Block Join
We have come to realize there are more efficient ways to nested loop joins, if we read **data in blocks** instead of **by single tuples** in each iteration:

```javascript
// Cost of reading R: M
forEach block br in R: // M Times
	forEach block bs in S: // Cost of reading S: N
		forEach r in br:
			for each s in bs:
				emit if r and s match
```

###### Cost
This is saying for **every block** in $R$, it scans $S$ once. In our example, treat a block as a page. The total cost is therefore $$M+(M\cdot N)$$
Even better, let's account for the usage of **buffer pools**. Let's assume we have $B$ buffer pools, with $B-2$ buffer pools used for scanning the **outer table**, 1 used for the **inner table**, and 1 used for storing output. Then, the algorithm looks like:

```javascript
// Cost of reading R: M
forEach B-2 block br in R: // M/B-2 Times
	forEach block bs in S: // Cost of reading S: N
		forEach r in B-2 br:
			for each s in bs:
				emit if r and s match
```

With this we have a total cost of $$M+\left(\left \lceil \frac{M}{(B-2)}\right \rceil \cdot N\right)$$
If the outer relation completely fits in memory, i.e. $B>M+2$, intuitively, the total cost becomes $M+N$. In other words, since we are only using 1 block for the inner table, we would be able to do in-memory join as long as $B$ has two more buffer pools then $M$.

Basic nested loop joins, however, are pretty bad. For each tuple in the outer table, we must do a **sequential scan to check for a match** in the inner table. With an existing index, we can avoid sequential scans by using an index to find inner table matches.

##### Index Nested Loop Join
Follows the algorithm:

```javascript
// Cost of reading R: M
forEach r in R: // m Times
	forEach s in Index(r_i = s_j) // Cost of look up: C
		emit if r and s match
```

###### Cost
If we assume the cost of each index probe is some constant $C$ per tuple, then the total cost is equal to $$M+(m\cdot C)$$
The exact cost $C$ here is somewhat subtle as it depends on the type of index used in the table. It is more useful if we are dealing with smaller tables (small $m$).

![[Pasted image 20240323202720.png|500]]

```ad-summary
**Nested Loop Joins, Summary**
- Pick the smaller table as the outer table
- **Buffer as much** of the outer table in memory as possible
- Loop over the inner table (or use an index)
```

#### Sort-Merge Joins 
Contains two phases:
1. **Sort** : sort both tables **on the join key**(s) 
	- Can use [[13 Sorting & Aggregation#External Merge Sort|external merge sort]]
2. **Merge**: Step through the two sorted tables with cursors and emit matching tuples
	- May need to backtrack depending on the join type 

The algorithm looks similar to a two-pointer traversal:

```python
sort R, S on join keys
init cursor_r, cursor_s on sorted keys
while cursor_r and curosr_s:
	if cursor_r > cursor_s:
		increment cursor_s
	elif cursor_r < cursor_s:
		increment cursor_r
	else:
		emit
		increment cursor_s
```

![[Pasted image 20240323203958.png|500]]

The algorithm above may not work in all databases - for instance, it may be possible that when we have multiple join keys from $R$ that match with a join key from $S$, because of the design of the algorithm, we increment the cursor from $S$ after only matching one of the join keys in $R$ and thus neglect the rest. 
- In such cases, when we encounter the same join key from the previous one in $R$, we should always decrement the inner cursor by one

##### Cost
The sorting cost (Table $R$, for instance) is $$2M\cdot\left(1+ \left\lceil \log_{B-1} \lceil \frac{M}{B}\rceil \right\rceil\right)$$ and the merge cost is on average
$$M+N$$
The total cost of the join is the sum of the two, or $$2M\cdot\left(1+ \left\lceil \log_{B-1} \lceil \frac{M}{B}\rceil \right\rceil\right)+2N\cdot\left(1+ \left\lceil \log_{B-1} \lceil \frac{N}{B}\rceil \right\rceil\right)+M+N$$ The thought process is similar to [[13 Sorting & Aggregation#^b4cf7d|that in external merge sort]]

The worst case for the merging phase is when the **join attribute** of all the tuples in both relations contains the **same value**: in which case the cost is $(M\cdot N)+ \text{sort cost}$.
- This could be more costly than nested loop joins if data are **NOT sorted**

```ad-summary
**Sort-Merge Joins, Summary**

Sort-Merge Joins are useful when one or both tables are **already sorted on join key**, or that the output must be **sorted on join key**.
- If sorted, we are just doing merge really

The input relations may be sorted either by an explicit sort operator, or by scanning the relation using an index on the join key.
```

#### Hash Joins 
As mentioned previously in nested loop joins, we can take advantage of indices to enable faster lookups. While hash indices are not as common as tree indices, they have their own merit.

If tuple $r \in R$ and a tuple $s \in S$ satisfy the join condition, then they have the same value for the join attributes. If that value is hashed to some partition $i$, the $R$ tuple must be in $r_i$ and the $S$ tuple in $s_i$. Therefore, $R$ tuples in $r_i$ need only to be compared with $S$ tuples in $s_i$.

There are two phases:
1. **Build**: **scan the outer relation** and populate a hash table using the hash function $h_1$ on the join attributes
2. **Probe**: scan the inner relation and use $h_1$ on each tuple to **jump to a location in the hash table and find a matching tuple**
	- $O(1)$ lookup/comparison, very efficient

In pseudo code, this is 

```javascript
build hash table HT for R
forEach s in S:
	emit if h_1(s) in HT
```

![[Pasted image 20240323215941.png|500]]

The **keys** in `HT` are simply attribute(s) that the query is joining the tables on. 

For values, they may depend on what the **operators above the join in the query plan expect** as its input.
1. Full tuple 
	- Avoid having to retrieve the outer relation's tuple contents on a match
	- Takes up **more space** if tuples have **MANY attributes**
2. Tuple identifier
	- Could be to either the base tables or the intermediate output from child operators in the query plan
	- Ideal for **column stores** because the DBMS does not fetch data from disk that it does not need
	- Better if join **selectivity is LOW**

The approach above is naive in that it assumes the hash table fits perfectly into memory. What if we do not have enough space and do not want to let the buffer pool manager swap out the hash table pages at random?

##### Grace Hash Join
Works when hash tables do not fit in memory. It also contains two phases:
1. **Build**: **Hash BOTH tables** on the join attribute into partitions using the **SAME hash function**
	- We're building for **BOTH** as we want to make sure tuples from $R$ and from $S$ are in the **SAME partition**
1. **Probe**: Compares tuples in corresponding partitions for each table

![[Pasted image 20240323220739.png|500]]

Then, assume each partition fits in memory, for each bucket (partition), we can do a nested loop join:

```javascript
forEach r in buckets_R:
	foreach s in buckets_S:
		emit if match(r,s)
```

Usually a **nested loop join** would be sufficient, as tuples hashed into the same partition tend to be fairly similar; or else, we can always build a hash index and do **hash join**.

The maximum size of a table that can be used in the approach above is $B(B-1)$, where $B-1$ is the partitions in the build phase, and $B$ is the maximum number of blocks each partition can hold.
- A single partition needs to fit into memory of size $B$

In other words, a table of $N$ pages need $\sqrt(N)$ buffers.

The above assumes hash distributes records evenly; if not, we can use a fudge factor $f>1$ such that the buffer pages needed is $B\cdot \sqrt(f\cdot N)$.

However, if a table does **NOT fit** in memory, we can either 
1. Go back to more naive methods such as nested loop joins that are more expensive but doesn't have memory restriction
2. **Recursively partition** to splits partitions into smaller partitions that will fit
	- Build another hash table for **each bucket** $b$ for both tables using $h_{2}$ such that $h_{2}\ne h_{1}$
	- Then, probe it for each tuple

![[Pasted image 20240323235322.png|500]]

##### Cost 
In the partition phase, the cost is $$2(M+N)$$ as we need to **read and write** **both tables**. In the probe phase, the cost is 
$$M+N$$
since we need to **read both tables**. Bringing the total cost to $3(M+N)$.

Note that this does **NOT include computing cost and output I/O**.

```ad-note
If the DBMS **knows the size of the outer table**, then it can use a **[[11 Hash Tables#Static Hashing Schemes|static hash table]]**.
- Less computational overhead for build/probe operations

However, if we do not know the size, then we must use a **[[11 Hash Tables#Dynamic Hashing Schemes|dynamic hash table]]** or allow for overflow pages.

A hash index needs to be **dynamic** since it needs to handle insertion and deletion of a table.
```

```ad-summary
**Join Algorithms, Summary**

![[Pasted image 20240324000222.png|500]]
```

Note that hashing is almost always better than sorting for operator execution. However, it has some caveats:
- Sorting is better on non-uniform data
- Sorting is better when result needs to be sorted

When data size is small, all algorithms will perform robustly in memory. In sum, DBMS should handle choosing algorithms, rather than users.
