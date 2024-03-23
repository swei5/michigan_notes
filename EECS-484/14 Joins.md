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
- Nested Loop Joins 
- Sort-Merge Joins 
- Hash Joins 

#### Nested Loop Joins

#### Sort-Merge Joins 

#### Hash Joins 