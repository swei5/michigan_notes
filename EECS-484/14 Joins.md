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

Like [[13 Sorting & Aggregation#^13b71e|sorting]], we have two ways of arranging outputs:
1. **Early Materialization**
	- Copy the values for the attributes in outer and inner tuples into a **new output tuple**
	- Then, subsequent operators in the query plan **NEVER** need to go back to the **base tables** to get more data

![[Pasted image 20240323164011.png|300]]

2. **Late Materialization**
	- Only copy the **joins keys** along with the **Record IDs** of the matching tuples
	- Ideal for [[10 Database Storage#Decomposition Storage Model (DSM)|column store]] because the DBMS **DOES NOT copy** data that is not needed for the query

![[Pasted image 20240323164421.png|300]]

#### Cost Analysis 
Assume that there are $M$ pages and $m$ tuples in $R$; $N$ pages and $n$ tuples in $S$. We define the **cost metric** as the **number of I/Os** to compute join.
- We will ignore output costs since that depends on the data and we cannot compute that yet

```ad-note
**Join v.s. Cross-Product**

$R \bowtie S$ is the most common operation and thus must be **carefully optimized**.

$R \times S$ followed by a selection is **inefficient** because the cross-product is large.
```

There are many algorithms for reducing join cost, but no algorithm works well in all scenarios.

---
### Join Algorithms 
We will cover three main types:
- **[[#Nested Loop Joins|Nested Loop Joins]]** 
- **[[#Sort-Merge Joins|Sort-Merge Joins]]** 
- **[[#Hash Joins|Hash Joins]]** 

#### Nested Loop Joins
##### Simple, Block Joins
The vanilla version follows the algorithm:

```javascript
forEach r in R: // Outer
	for each s in S: // Inner
		emit if r and s match
```

For join keys $r, s$.

This is stupid, because we need to scan $S$ once for every $r \in R$ . Total cost is $$M+(m \cdot N)$$
Albeit the inefficiency, this shows us why we would want the **smaller table** to be on the LHS: $$N+(n\cdot M) < M+(m \cdot N) \text{ for } n<m, N<M$$
We have come to realize there are more efficient ways to nested loop joins, if we read **blocks of data** instead of the entirety of a table in each iteration:

```javascript
forEach block br in R: // M Times
	forEach block bs in S: // N Times
		forEach r in br:
			for each s in bs:
				emit if r and s match
```

This is saying for **every block** in $R$, it scans $S$ once. The total cost is therefore $$M+(M\cdot N)$$
Even better, let's account for the usage of **buffer pools**. Let's assume we have $B$ buffer pools, with $B-2$ buffer pools used for scanning the **outer table**, 1 used for the inner table, and 1 used for storing output. Then, the algorithm looks like:

```javascript
forEach B-2 block br in R: // M/B-2 Times
	forEach block bs in S: // N Times
		forEach r in B-2 br:
			for each s in bs:
				emit if r and s match
```

With this we have a total cost of $$M+\left(\left \lceil \frac{M}{(B-2)}\right \rceil \cdot N\right)$$
If the outer relation completely fits in memory, i.e. $B>M+2$, intuitively, the total cost becomes $M+N$.

Basic nested loop joins, however, are pretty bad. For each tuple in the outer table, we must do a **sequential scan to check for a match** in the inner table. With an existing index, we can avoid sequential scans by using an index to find inner table matches.

##### Index Nested Loop Join
Follows the algorithm:

```javascript
forEach r in R:
	forEach s in Index(r_i = s_j)
		emit if r and s match
```

If we assume the cost of each index probe is some constant $C$ per tuple, then the total cost is equal to $$M+(m\cdot C)$$

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

The sorting cost (Table $R$, for instance) is $$2M\cdot\left(1+ \left\lceil \log_{B-1} \lceil \frac{M}{B}\rceil \right\rceil\right)$$ and the merge cost is
$$M+N$$
The total cost of the join is the sum of the two.

#### Hash Joins 