[[2024-02-08]] #Database 

Up to this point, we have learned how to transform database designs (represented by an ER diagram) into relational schema with **integrity constraints covered**. This process is known as **normalization**.

![[Pasted image 20240212171904.png|500]]

In this section, we will formally introduce the process in a precise and mathematical manner.
### Motivation 
Imagine the following table:

![[Pasted image 20240208152558.png|500]]

This is a terrible design for many reasons:
- `NULL` values in *keys*
- Addresses appear to be **multi-valued**
- Redundancy in (`Item`, `Desc`)

The process in which we reduce these unwanted features is called **normalization**.
- Avoid redundancy of data
- Capture the dependencies inherent in the data

```ad-important
We want tables where the attributes **depend on the primary key**, on the whole key, and nothing but the key.
```

There are two approaches to normalization.
1. [[2 Entity-Relationship Model|Create an ER model]] and then [[4 Mapping ER to Relational|map to tables]] - should result in good (normalized) tables (very manual)
2. State **dependencies** between attributes of tables and **map dependencies to tables** (automatic)

---
### Normal Forms
A normal forms guarantees that certain problems won’t occur and obeys certain rules:
1. 1 NF: **NO set-valued attributes**
	- Rows can be ordered and **ALL rows** are independent 
	- Redundancy might exist
1. 2 NF: **Historical**
2. [[#3NF]]
3. [[#Boyce-Codd Normal Form (BCNF)|BCNF]]
4. Use lossless decompositions for multi-valued dependencies

The higher the form, the more restrictive the model is.

---
### Normalization
#### Redundancy
Having redundant rows causes the following problems:
- Space inefficient 
- Messy update process for **transactional databases**
	- **Update anomalies**: Changing a field requires changing multiple rows
	- **Insertion anomalies**: Inserting a field inserting `NULL` or other values in unrelated columns
	- **Deletion anomalies**: Deleting an item requires care and could end up deleting a something unintended

Read-only databases don't care as much about redundancy, however.

We could surely use ER diagramming and translation to a relational model. However, this seems rather ad hoc. Alternatively, we can use functional dependencies.

#### Functional Dependencies
FD captures dependency between attributes. Given the notation
$$X \to Y$$
This reads as $X$ functionally determines $Y$, and that $Y$ depends on $X$ or for a given $X$, there is one $Y$.

More formally, a FD specifies a form of integrity constraint.
$$F:X\to Y \equiv \pi_{X}(t_{1})=\pi_{X}(t_{2})\implies \pi_{Y}(t_{1})=\pi_{Y}(t_{2})$$
For some subsets of relation $R$ 's attributes $X$, $Y$.

```ad-example
In this table shown below, the FDs are as followed:
- Supplier ID $\to$ Supplier Name
- Item $\to$ Desc
- (Supplier ID, Item) $\to$ Price

![[Pasted image 20240208162254.png|500]]
```

```ad-note
An FD is a statement about **ALL** allowable relations.
- Based only on application semantics, not a table instance

**[[3 Relational Model#^eff39f|Primary key constraint]]** is a special case of **functional dependencies** such that primary key attributes $\to$ all other attributes.
```

We can then map functional dependencies to tables. However, note the example below is subjective to ER Diagram design - there could be a lot of ways to represent these dependencies.

```ad-example
**Constraints to Entity Sets**

![[Pasted image 20240208173413.png|600]]
```

#### Implied FDs 
Implied functional dependencies, noted as $F+$ as the closure of $F$, is the set of **ALL valid** functional dependencies that can be derived from $F$. To derive $F+$, we need to use **Armstrong's Inference Axioms**.

```ad-important
**Definition 8.1**: Armstrong's Inference Axioms

Given sets of attributes $X,Y,Z$, the following holds true:
- **Reflexivity**: if $Y \subseteq X$, then $X \to Y$ (trivial dependency)
	- e.g. (`supplier_id`, `item`) $\to$ `item`
- **Augmentation**: if $X \to Y$, then $XZ \to YZ$ $\forall Z$
- **Transitivity**: if $X \to Y$ and $Y \to Z$, then $X \to Z$

**Additional Rules**:
- **Union**: If $X \to Y$ and $X \to Z$, then $X \to YZ$
	- Intuition: $y,z$ must be on the same row if having equal $x$
- **Decomposition**: If $X \to YZ$, then $X \to Y$ and $Y \to Z$
```

```ad-note
Given the FD $X \to A$, where $A$ includes **ALL attributes** of $R$, we can deduce that $X$ must be a superkey.
- $X$ may not be a candidate key since its possible that $X=A$
```

##### Sound and Complete
A few more definitions to look at:
- $F^\star$ are all **FD**s that are implied by $F$
- $F{+}$ are all **FD**s that can be generated from $F$ using Armstrong's Axioms

**Soundness** describes that $F+$ is a **subset** of $F^{\star}$, and **completeness** describes that $F^\star$ is a **subset** of $F+$.

Armstrong’s Axioms can be shown to be **both sound and complete**.
- The axioms **CANNOT** generate illegal properties, and the axioms can generate **ALL implications**

Next, we will discuss how to actually applies FD in reducing redundancy of a relation: decomposition.
#### Decomposition 
Two key goals of decomposition are 
- **Lossless Join**: *Can we reconstruct the original relation from instances of the decomposed relations?*
	- **REQUIRED**
- **Dependency Preservation**: Avoid having to join decomposed relations to **check dependencies**
	- Single-table constraints are better than multi-table constraints
	- Good to have, but **NOT required**

One major downside of decomposition is expensive querying led by more join operations.

##### Lossless Join Decompositions
Given some relation $R$ that decomposes to $X,Y$; the decomposition of $R$ into $X,Y$ is **lossless** if
$$\pi_{X}(r) \bowtie \pi_{Y}(r)=r$$
For **every instance** $r$ of $R$.

Alternatively, **lossless-join** with respect to $F$ exists if and only if $F+$ contains
$$X \cap Y\to X$$ or $$X \cap Y \to Y$$
In other words, attributes common to $X$ and $Y$ contain a **key** for either $X$ or $Y$.

```ad-example
The following decomposition is **NOT** lossless join.

![[Pasted image 20240212153959.png|500]]
```

##### Dependency Preserving Decomposition 
A given relation $R$ has a dependency-preserving decomposition to $X,Y$ if
$$F+=(F_{X} \cup F_{Y})+$$

```ad-note
Note that it is not necessarily true that $F=(F_{X}\cup F_{Y})$ if the above holds true.
```

To test this, simply verify the functional dependencies **using the decomposed tables**.

#### Boyce-Codd Normal Form (BCNF)
Relation $R$ with FD $F$ is in **BCNF** if for a subset of attributes $X$ and some single attribute $A$ such that for all $X \to A \in F+$:
- $A \subseteq X$ (trivial FD), or
- $X$ is a [[3 Relational Model#^5bf6f5|superkey]] of $R$

```ad-important
In a BCNF relation, all non-trivial FDs over $R$ are due to keys.
```

BCNF guarantees **NO redundancy** in $R$ and is the most desirable form. Decomposition can end here.

![[Pasted image 20240212164441.png|400]]

#### 3NF
Relation $R$ with FD $F$ is in **3NF** if for some single attribute $A$ such that for all $X \to A \in F+$:
- $A \subseteq X$ (trivial FD), or
- $X$ is a superkey, or
- $A$ is part of some (minimal) **key** for $R$ (prime attribute)

```ad-note
BCNF **implies** 3NF, but 3NF **DOES NOT imply** BCNF. In other words, 3NF is a decomposition form where BCNF could not have been achieved (slightly more relaxed version).
```

```ad-example
Imagine the following table $T$(Sailor, Boat, Date, Card)
- Given FD: $SBD \to SBDC$, $S\to C$: $T$ is **NOT 3NF**
	- $SBD$ could be a super key of $T$, however
	- $S$ it not a key of the $T$, **AND**
	- $C$ is not a part of the key(s) of $T$
- If, additionally we have $C \to S$, then $CBD \to SBDC$ (which makes $CBD$ a key), now $C$ **IS** part of the keys of $T$ and $T$ is in 3NF
```

**Lossless-join**, **dependency-preserving** decomposition of $R$ into a collection of 3NF relations is **ALWAYS** possible.

```ad-tip
To verify whether if any relation is in 3NF, the best way to start would be to enumerate through all the given FDs to find out **ALL possible keys** (minimal) of the table.

Then, we attempt to **list the given FDs that violate** BCNF/3NF.
```

```ad-example
Suppose you are given the following relation $R$: $(ABCDEF)$ and the following FDs 
- $BC \to D$
- $CD \to B$
- $D \to E$
- $ACD \to F$

**Solution**
1. All possible keys: $ACD, ABC$
2. FDs that violate BCNF: $BC \to D$, $CD \to B$, $D\to E$
	- None are keys of $R$
3. FDs that violate 3NF: $D \to E$
	- $E$ not part of any keys of $R$
```

#### Decomposition into BCNF
Given an input relation $R$ with FDs $F$, we repeat the following steps until **NO** $X \to Y$ violates BCNF for all $(X\to Y)\in F$.
1. Identify if any FDs violate BCNF
	- If $X \to Y$ violates BCNF, decompose $R$ into $R-Y$ and $XY$

```ad-note
This algorithm provides a **lossless join decomposition**. This is because it's guaranteed that $X$ is a key for the relation $XY$.
- Because $X\to Y$
```

Several dependencies may cause violation of BCNF. The order in which we deal with them could lead to very **different sets of relations**.

```ad-example
Given $R=(A,B,C,D,E)$, FDs: $A\to BC, CD \to E, B \to D, E\to A$, we then 
1. Find the keys: $A, BC, CD, E$
2. Find the FD(s) that violate BCNF: $B\to D$
3. Decompose $R$ to $R-D$: $R_{1}=(A,B,C,E)$ and $R_{2}=(B,D)$

**Remarks**
The decomposition is lossless join - $R_{1}\cap R_{2}=B$ and $B \to R_{2}$. However, it is **NOT dependency preserving**: $CD\to E \notin (F_{1}\cup F_{2})+$
```

---
### Schema Refinement 
Suppose you are given the following schema of $R$. You are asked to normalize it: `IS: (~item~, name, desc, loc, price)` and `S: (~name~, addr)` with the additional FD that `name` $\to$ `loc`.

Let's first **summarize all existing FDs**:
- `item` $\to$ `IS`
- `name` $\to$ `S`
- `name` $\to$ `loc`

We then notice that `IS` is **NOT in** BCNF since `name` $\to$ `loc` and `loc` being a non-key of `IS`. Hence, we decompose `IS` into
- `IS(~item~, name, desc, price)`
- `LOC(~name~, loc)
- `S: (~name~, addr)` remains **unchanged**

Here, we spot an opportunity because `LOC` and `S` share the same key - we merge them to arrive at the final solution
- `IS(~item~, name, desc, price)`
- `S: (~name~, addr, loc)`
