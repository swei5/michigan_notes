[[2024-03-23]] #Database #DBMS 

```ad-todo
Roadmap of DBMS:
- ~~Disk Manager~~
- ~~Access Methods~~
- ~~Operator Execution~~
- **Query Planning** 
- Transaction Manager 
- Log Manager
```

In this lecture, we will finally assemble the parts learned on operators (joins and sorts) into the bigger picture of a query plan.

### Processing Model
A DBMS's processing model defines **how the system executes a query plan**. 
1. **Iterator Model** 
2. **Materialization Model**
3. **Vectorized / Batch Model**

```ad-question
The utmost important question in query processing is the **granularity of data**: how do we move data around?

There are different trade-offs for different workloads.
```

#### Iterator Model 
Most common in early DBMS, also known as Volcano or Pipeline model. Each query plan operator implements a `Next()` function.
- On **each invocation**, the operator returns either a **single tuple** or a `null` marker if there are no more tuples

The operator implements a **loop** that calls `Next()` on its children to retrieve their tuples and then process them.

```ad-note
Each operator does **NOT need to wait** for its children to finish running before it can start processing data.
```

```ad-example
**Example: Iterator Model**

![[Pasted image 20240324002007.png|500]]

We would need to finish running the entirety of step (3) before going into step (4). This is because we need to have a full hash table before starting the probe phase.

![[Pasted image 20240324002020.png|500]]

When data is emitted from step (5), it gets fed into step (4), then (2), and finally (1) in output. The model **DOES NOT** read all tuples first in the memory at once then perform operations; it does so **iteratively** in a pipeline.
- Results can be returned to clients immediately after one single tuple is returned 
```

This is used in almost every DBMS and it allows for **tuple pipelining**. Some operators **must block** until their children emit all their tuples.
- Joins, subqueries, order by

Output control works easily with this approach.

```ad-summary
**Cons, Iterator Model**
- Maybe accessing data more than needed
- Coordination is difficult with multitudes of function calls: synchronization overheads
- Excessive movements of data between operators
```

#### Materialization Model 
Each operator **processes its input all at once** and then **emits its output all at once**. The operator "materializes" its output as a **single result** to a buffer and pass it to parent operator.
- Like in [[14 Joins#^7c7a46|value selections]] seen earlier, we have the options of using early materialization (sending tuples) or late materialization (record or tuple IDs)

The DBMS can push down hints (e.g., `LIMIT`) to avoid scanning too many tuples. The output can be either **whole tuples** ([[10 Database Storage#$N$ -ary Storage Model (NSM)|NSM]]) or **subsets of columns** ([[10 Database Storage#Decomposition Storage Model (DSM)|DSM]]).

![[Pasted image 20240324003335.png|500]]

```ad-summary
**Materialization Model, Pros and Cons**

- **Pros**
	- Better for OLTP workloads because queries only access a **small number of tuples at a time**
		- Lower execution / coordination overhead
		- Fewer function calls 
- **Cons**
	- Not good for OLAP queries with **large intermediate results**
	- Large buffer/storage requirement
```

#### Vectorization Model 
Like the Iterator Model where each operator implements a `Next()` function, but emits **a batch of tuples** instead of a single tuple. A mixture between iterator model and materialization model.
- The operator's **internal loop processes multiple tuples** at a time
- The size of the batch can **vary based on hardware or query properties** (CPU Cache)

![[Pasted image 20240324005313.png|500]]

The cost of coordination amortizes across the many tuples in one batch, which in the end is negligible.

It is **ideal for OLAP queries** because it greatly reduces the number of invocations per operator. It also allows for operators to more easily use **vectorized (SIMD) instructions** to process batches of tuples.

Most analytical systems use or support the vectorization model.

---
### Plan Processing Direction 
When it comes to the direction of process, we can either do
1. **Top-to-Bottom** (What we have looked at)
	- **Start with the root** and "pull" data up from its children
	- Tuples are always **passed with function calls**
2. **Bottom-to-Top**
	- **Start with leaf nodes** and push data to their parents
	- Allows for tighter control of caches/registers in pipelines

---
### Access Methods 
An access method is the way that the DBMS accesses the data stored in a table.
1. **Sequential Scan** 
2. **Index Scan** 
3. **Multi-index/Bitmap Scan** 

This is the basis of all operations, but is **NOT** defined in [[5 Relational Algebra|relational algebra]]. 

#### Sequential Scan 
For **each page** in the table, we
1. Retrieve it from the buffer pool
2. Iterate over each tuple and evaluate predicate

The DBMS maintains an **internal cursor** that tracks the **last page/slot it examined**.
- So that when parent operator calls `next()` we know where to continue

This is almost always the **WORST** thing that the DBMS can do to execute a query. We will focus on two primary optimization methods.
##### Zone Maps
Zone maps are **pre-computed aggregates for the attribute** values in a page. DBMS **checks the zone map** first to decide whether it wants to access the page. All zone maps of a group of pages are stored in the page directory.

![[Pasted image 20240324010148.png|500]]

Observing a zone map for this given data block informs the DBMS to not scan it at all, for instance.

##### Late Materialization 
DSM DBMSs can **delay stitching together tuples** until the upper parts of the query plan.

![[Pasted image 20240324010349.png|500]]

In the selection stage, we only read in all `a` tuples and return their offsets to the parent operator; in the join operation, we only returned results that pertained to offsets of `b`, etc. 

#### Index Scan 
The DBMS picks an index to find the tuples that the query needs. The usage of index has a number of considerations (more on [[18 Query Planning II|this]] in later chapters):
- What attributes the index contains
- What attributes the query references
- The attribute's value domains
- Predicate composition
- Whether the index has unique or non-unique keys

#### Multi-index Scan 
If there are **multiple indices** that the DBMS can use for a query, we 
1. Compute **sets of Record IDs** using each matching index
2. **Combine these sets** based on the query's predicates (union/intersect)
3. Retrieve the records and apply any remaining predicates

Note that the first two phases **NEED NOT** to scan the pages per se but the **tuples' offsets** (Record IDs).

![[Pasted image 20240324010930.png|500]]

---
### Modification Queries
Operators that modify the database (`INSERT`, `UPDATE`, `DELETE`) are responsible for **checking constraints** and **updating indices**.

For `UPDATE` and `DELETE`, child operators **pass Record IDs for target tuples** and they **MUST keep** track of **previously seen tuples**.
- To avoid repetitive update

For `INSERT`, we have two design choices:
1. `INSERT` **materializes** tuples inside of the operator 
2. `INSERT` **inserts** tuples constructed by some child operators

#### `UPDATE`
Let's look at an example of how `UPDATE` works under the hood.

![[Pasted image 20240324125817.png|500]]

Once we retrieve the record to be updated from the index, we first **remove it from the index**, then **update the tuple**, and finally **insert** the new record into the index.

The issue is that, if we **DID NOT** keep track of previously seen tuples (e.g. `999, Atul`), after we updated such tuple and retrieve it again ( `1099, Atul`) in the index, we would have accidentally updated it twice.

![[Pasted image 20240324130039.png|500]]

This is known as the **Halloween Problem**, which describes the anomaly where an `UPDATE` operation changes the physical location of a tuple, which causes a **scan operator to visit the tuple multiple times**.
- Can occur on clustered tables or index scans

The solution is to track **modified Record IDs per query**.

---
### Expression Evaluation 
The DBMS represents a `WHERE` clause as an **expression tree**. The nodes in the tree represent different expression types:
- Comparisons
- Conjunctions (`AND`), disjunction (`OR`)
- Arithmetic Operators 
- Constant Values 
- Tuple Attribute References 

![[Pasted image 20240326134821.png|500]]

In this example above, we first start at the root (predicate of the query), then traverse from left to right, from up to bottom, until we obtain all necessary values to evaluate the predicate `S.val = $1 + 9`.

However, evaluating predicates in this manner is **slow**, though flexible.
- The DBMS **traverses the tree and for each node** to figure out what the operator needs to do

A better approach is to just evaluate the expression directly.
- At runtime, generate function calls and use it (JIT compilation)

![[Pasted image 20240326135230.png|300]]

