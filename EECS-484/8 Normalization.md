[[2024-02-08]] #Database 

### Motivation 
Imagine the following table:

![[Pasted image 20240208152558.png|500]]

This is a terrible design for many reasons:
- `NULL` values in ID field 
- Addresses appear to be **multi-valued**
- Redundancy in (`Item`, `Desc`)

The process in which we reduce these unwanted features is called **normalization**.
- Avoid redundancy of data
- Capture the dependencies inherent in the data

```ad-important
We want tables where the attributes **depend on the primary key**, on the whole key, and nothing but the key.
```

There are two approaches to normalization.
1. Create an ER model and then map to tables - should result in good (normalized) tables (very manual)
2. State **dependencies** between attributes of tables and **map dependencies to tables** (automatic)

---
### Normalization 
TODO:

#### Redundancy
Having redundant rows causes the following problems:
- Space inefficient 
- Messy update process
	- **Update anomalies**: Changing a field requires changing multiple rows
	- **Insertion anomalies**: Inserting a field inserting `NULL` or other values in unrelated columns
	- **Deletion anomalies**: Deleting an item requires care could end up deleting a something unintended

We could surely use ER diagramming and translation to a relational model. However, this seems rather ad hoc. Alternatively, we can use functional dependencies.

#### Functional Dependencies
FD captures dependency between attributes. Given the notation
$$X \to Y$$
This reads as $X$ functionally determines $Y$, and that $Y$ depends on $X$ or for a given $X$, there is one $Y$.

More formally, a FD specifies a form of integrity constraint.
$$D:X\to Y \equiv \pi_{X}(t_{1})=\pi_{X}(t_{2})\implies \pi_{Y}(t_{1})=\pi_{Y}(t_{2})$$
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

We can then map functional dependencies to tables.

```ad-example
**Constraints to Entity Sets**

![[Pasted image 20240208173413.png|600]]
```

#### Implied FD
Implied functional dependencies, noted as $F+$ as the closure of $F$, is the set of **ALL valid** functional dependencies. To derive $F+$, we need to use **Armstrong's Inference Axioms**.

```ad-important
**Definition 8.1**: Armstrong's Inference Axioms

Given sets of attributes $X,Y,Z$, the following holds true:
- **Reflexivity**: if $Y \subseteq X$, then $X \to Y$ (trivial dependency)
	- e.g. (`supplier_id`, `item`) $\to$ `item`
- **Augmentation**: if $X \to Y$, then $XZ \to YZ$ $\forall Z$
- **Transitivity**: if $X \to Y$ and $Y \to Z$, then $X \to Z$

**Additional Rules**:
- **Union**: If $X \to Y$ and $X \to Z$, then $X \to YZ$
- **Decomposition**: If $X \to YZ$, then $X \to Y$ and $Y \to Z$
```

