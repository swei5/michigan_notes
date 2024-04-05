[[2024-04-04]] #DBMS #Database 

This lecture continues the discussion on cost models and their applications in query optimization.

### Statistics and Predicates
To know **HOW MANY tuples** a DBMS processes, we need to have statistics. The DBMS stores internal statistics about tables, attributes, and indexes in its internal catalog. Different systems update them at different times. They can be invoked manually:
- `ANALYZE` in Postgres and SQLite
- `ANALYZE TABLE` in Oracle

Specifically, for each relation $R$, the DBMS maintains the following information
- $N_{R}$: number of tuples in $R$
- $V(A,R)$ Number of **distinct values** for attribute $A$

The **selection cardinality** $SC(A, R)$ is the **average number of records** with a value for an attribute $A$:
$$\frac{N_{R}}{V(A,R)}$$
Note that this **formula assumes data uniformity** where **every value has the same frequency** as all other values.

Equality predicates on **unique keys** are easy to estimate: selection cardinality is guaranteed to be 1 or 0. In some other cases, such as inequality or range predicates, it could be pretty complex.

#### Complex Predicates 
The **selectivity** (`sel`) of a predicate $P$ is the **fraction of tuples that qualify**.
- Would be in range of $[0,1]$

Its formula depends on **types of predicate**.

##### Selections
- Equality predicate:  $$\text{sel}(A:=\text{Constant})=\frac{SC(P)}{N_{R}}$$
- Range predicate: $$\text{sel}(A\ge a)=\frac{A_{\text{max}}-a+1}{A_{\text{max}}-A_{\text{min}}+1}$$
- Negation: $$\text{sel}(\neg P)=1-\text{sel}(P)$$

Selection is somewhat like a **probability** of attributes that match with a given predicate. Hence, we can also apply probability rules on selectivity to account for even more complex predicates.
- Conjunction: $$\text{sel}(P_{1}\land P_{2})=\text{sel}(P_{1})\cdot \text{sel}(P_{2})$$
However, this assumes that the selectivities are **INDEPENDENT**.
- Disjunction: $$\text{sel}(P_{1}\lor P_{2})=\text{sel}(P_{1})+ \text{sel}(P_{2})-\text{sel}(P_{1})\cdot \text{sel}(P_{2})$$
---
### Result Size Estimation for Joins
It is important for the join operator needs to **estimate the return size** for the higher level operators to process their estimates for the query plan.
- In other words, we care about the question: in other words, for a given tuple of $R$, on average, **how many tuples** of $S$ will it match?

We assume each key in the **inner relation ($S$) will EXIST** in the outer table ($R$). In a general case $R_{\text{cols}}\cap S_{\text{cols}}=\{A\}$, where $A$ is not a primary key for either table,
- If we match each $R$ -tuple with $S$ -tuples, the estimated size is $$N_{R}\cdot SC(A,S)=N_{R}\cdot \frac{N_{S}}{V(A,S)}$$
- Symmetrically, for $S$: $$N_{S}\cdot SC(A,R)=N_{R}\cdot \frac{N_{S}}{V(A,R)}$$

To be on the more conservative end, overall, we define that the estimated size of a equijoin as $$N_{R}\cdot \frac{N_{S}}{\text{max}(V(A,R),V(A,S))}$$
