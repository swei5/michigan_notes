[[2024-04-04]] #DBMS #Database 

This lecture continues the discussion on cost models and their applications in query optimization.

### Statistics and Cost Estimation
To know **HOW MANY tuples** a DBMS processes, we need to have statistics. The DBMS stores internal statistics about tables, attributes, and indices in its internal catalog. Different systems update them at different times. They can be invoked manually:
- `ANALYZE` in Postgres and SQLite
- `ANALYZE TABLE` in Oracle

Specifically, for each relation $R$, the DBMS maintains the following information
- $N_{R}$: number of tuples in $R$
- $V(A,R)$ Number of **distinct values** for attribute $A$

The **selection cardinality** $SC(A, R)$ is the **average number of records** with a value for an attribute $A$:
$$\frac{N_{R}}{V(A,R)}$$
Note that this **formula assumes data uniformity** where **every value has the same frequency** as all other values.

Equality predicates on **unique keys** are easy to estimate: selection cardinality is guaranteed to be 1 or 0. In some other cases, such as inequality or range predicates, it could be pretty complex.

#### Complex Predicates in Selections
The **selectivity** (`sel`) of a predicate $P$ is the **fraction of tuples that qualify**.
- Would be in range of $[0,1]$

Its formula depends on **types of predicate**.
- Equality predicate:  $$\text{sel}(A:=\text{Constant})=\frac{SC(P)}{N_{R}}$$
- Range predicate: $$\text{sel}(A\ge a)=\frac{A_{\text{max}}-a+1}{A_{\text{max}}-A_{\text{min}}+1}$$
- Negation: $$\text{sel}(\neg P)=1-\text{sel}(P)$$

Selection is somewhat like a **probability** of attributes that match with a given predicate. Hence, we can also apply probability rules on selectivity to account for even more complex predicates.
- Conjunction: $$\text{sel}(P_{1}\land P_{2})=\text{sel}(P_{1})\cdot \text{sel}(P_{2})$$
However, this assumes that the selectivities are **INDEPENDENT**.
- Disjunction: $$\text{sel}(P_{1}\lor P_{2})=\text{sel}(P_{1})+ \text{sel}(P_{2})-\text{sel}(P_{1})\cdot \text{sel}(P_{2})$$
#### Result Size Estimation for Joins
It is important for the join operator needs to **estimate the return size** for the higher level operators to process their estimates for the query plan.
- In other words, we care about the question: in other words, for a given tuple of $R$, on average, **how many tuples** of $S$ will it match?

We assume each key in the **inner relation ($S$) will EXIST** in the outer table ($R$). In a general case $R_{\text{cols}}\cap S_{\text{cols}}=\{A\}$, where $A$ is not a primary key for either table,
- If we match each $R$ -tuple with $S$ -tuples, the estimated size is $$N_{R}\cdot SC(A,S)=N_{R}\cdot \frac{N_{S}}{V(A,S)}$$
- Symmetrically, for $S$: $$N_{S}\cdot SC(A,R)=N_{S}\cdot \frac{N_{R}}{V(A,R)}$$

To be on the more conservative end, overall, we define that the estimated size of a equijoin as $$N_{R}\cdot \frac{N_{S}}{\text{max}(V(A,R),V(A,S))}$$

```ad-summary
**Selectivity Assumptions, Summary**
1. **Uniform Data**: The distribution of values (except for the heavy hitters) is the same
2. **Independent Predicates**: The predicates on attributes are independent 
3. **Inclusion Principle**: The domain of join keys overlap such that each key in the inner relation will also exist in the outer table
```

The segment below describes different ways of estimation in a more realistic manner.
#### Correlated Attributes 
The independent predicates assumption may fall prey to such issue; to mitigate this issue, users may manually inform the DBMS about the correlated attributes and treat each as a single tuple.

#### Cost Estimations
What if data are **NOT uniformly distributed**? A naive way to tackle this would be to save the **number of occurrences** for each distinct value.
- Not space efficient, however

Better, we can maintain a number of **equi-width buckets**, i.e. storing buckets that contain the same number of distinct values.

![[Pasted image 20240405001956.png|500]]

This is subject to outliers - where one value's count significantly deviates from the others in the same bucket.

Another way to improve on this is to have **equi-depth buckets** such that **the total number of occurrences for each bucket** is roughly the same.

![[Pasted image 20240405002157.png|500]]

#### Sampling
Modern DBMSs also collect samples from tables to estimate selectivities. 
- Sample is kept somewhere in metadata
- Run predicates on samples to get a rough estimate

We update samples when **the underlying tables changes significantly**.

This method is **NOT subject to any assumptions**. However, it comes at the cost of reading the tables, selecting the samples and storing them somewhere, as contrast to having some simple equations.

#### Sketches
Maintain a **probabilistic data structures** that generate approximate statistics about a data set. **Cost-model** can replace histograms with sketches to improve its selectivity estimate accuracy.

---
### Query Optimization 
After finishing estimating the selectivity of predicates, and subsequently the cost of query plans, we can apply these information to look for the optimal query plan.

After performing [[17 Query Planning I#^f47e10|rule-based rewriting]], the DBMS will enumerate different plans for the query and estimate their costs.
- Single relation (heuristics already work pretty well)
- Multiple relations
	- More in-depth cost-based search
- Nested sub-queries
	- [[17 Query Planning I#^c1b618|Flatten or extract]]

It chooses the **best plan it has seen for the query** after exhausting all plans or some **timeout**.
- Usually around 1 ms

#### Single Relation
Despite application of heuristics, there may still be some work for us to do such as: 
1. Pick the best access method.
	- Sequential Scan
	- Binary Search (clustered indices)
	- Index Scan
2. Predicate evaluation ordering

We can use the cost estimation methods mentioned above to make the choice.

Query planning for **OLTP** queries is easy because they are **sargable** (Search Argument Able).
- It is usually just picking the best indices 
- Joins are almost **always on foreign key relationships with a small cardinality**
- Can be implemented with simple heuristics

#### Multi-relation Query 
As **number of joins increases**, number of **alternative plans grows rapidly**. Hence, we need to **restrict search space** to limit the number of combinations.

The first principle is that we should **ONLY consider left-deep join trees**. A left-deep join tree is one such that the join operator can only be either the **root** of the tree, or the **left child** of its parent operator.
- Many modern DBMSs still make this assumption - this is a fairly efficient method to find optimal query plan

![[Pasted image 20240405010555.png|500]]

In a multi-relation query plan, we would consider the following:
1. Enumerate the **orderings**
	- E.g. Left-deep tree #1 , Left-deep tree #2 …
2. Enumerate the **plans for each operator**
	- E.g. Hash, Sort-Merge, Nested Loop…
3. Enumerate the **access paths** for each table
	- Index #1 , Index #2 , Sequential Scan…

This is a complex optimization problems with multiple stages. We may use dynamic programming to reduce the number of cost estimations.

However, **NO** real DBMS does it this way - usually more messy and intertwined. 

#### Postgres Optimizer 
Examines **ALL types** of join trees.
- Left-deep, Right-deep, bushy

It also uses two optimizer implementations:
- Traditional Dynamic Programming Approach (as in above)
- Genetic Query Optimizer (GEQO)

Postgres uses the traditional algorithm when # of **tables in query is less than 12** and switches to GEQO when there are 12 or more.
- With more than 12 tables the search space could be huge

GEQO uses a randomized algorithm to generate query plans initially, discard the worst plans among the generated plans, and **combine the elements in the selected plan** to construct the **next generations of plans**. 

![[Pasted image 20240405112842.png|500]]

