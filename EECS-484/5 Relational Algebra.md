 [[2024-01-28]] #Database #SQL

### Formal Query Languages 
Formal query languages are the foundation for commercial query languages like SQL. There are two main types 
- Declarative: **Relational Calculus**
	- Describe what a user wants, rather than how to compute it
- Procedural: **Relational Algebra**
	- Operational, very useful for representing execution plans

Query Languages **ARE NOT** programming languages.
- Not expected to be Turing Complete 

```ad-note
Understanding Algebra & Calculus is key to understanding SQL query processing.
```

A query takes the input as relational instances and also outputs relational instances.
- Specified using the schemas

#### Relational Operators 
- **Selection** $\sigma$: Selects a subset of rows from relation
- **Projection** $\pi$: Deletes unwanted columns from relation
- **Cross-product** $\times$: Allows us to combine two relations
- **Set-difference** $-$: Returns tuples in relation 1, but not in relation 2
- **Union** $\cup$: Returns tuples in relation 1 and in relation 2

Selection and projection deals with one relation, and the others deal with two relations.

Additional operations (constructed from basic ops – not essential, but very useful) include
- Intersection 
- Joint 
- Division
- Renaming 

Because algebra is **closed**, we can compose operators.

---
### Selection: $\sigma$
Retrieve **rows** that satisfy a logical condition.
$$\sigma_{\text{condition}}(R)$$
Each term in the condition has one of the following forms:
- `<attr1> <op> <attr2>`, or
- `<attr> <op> <value>`

In the background, the program iterates through all the rows to locate all that have a matching condition.

```ad-example
The expression below is equivalent to `SELECT * FROM Athlete A WHERE A.sport = 'gymnastics' AND A.country = 'USA';`

![[Pasted image 20240128110446.png|500]]
```

---
### Projection: $\pi$
Does the following:
- Selects columns
- Delete attributes that are not in projection list

Projection operator in relational algebra **removes duplicates**; this is because classical relational model is a **set**.
- **DOES NOT** remove duplicates in SQL standards

```ad-example
The expression below is equivalent to `SELECT sport, country FROM Athlete;`

![[Pasted image 20240128111528.png|500]]
```

### Set Operations
Takes input of **two union-compatible relations**. The field names of the output use the names from the **first input**.
- Have to have the same number and type of attributes, in same order

#### Set Difference: $-$
Projection is often used before subtraction.

```ad-example
The expression below is equivalent to `SELECT * FROM name MINUS SELECT * FROM AthleteName;`

$$\text{name}-\text{AthleteName}$$

![[Pasted image 20240128111815.png|500]]
```

#### Cross-Product (Cartesian): $\times$
Result is each row of one relation **combined with each row** of the second relation. The result schema will have **ALL fields** from both relations, names inherited by default.

```ad-example
The expression below is equivalent to `SELECT * FROM Athlete, Olympics;`

$$\text{Athlete} \times \text{Olympics}$$

![[Pasted image 20240128112130.png|500]]
```

### Rename Operator: $\rho$
Use cases include
- Shorthand
- Self-joins
- If two relations have the same field

Follows the convention of
$$\rho_{\text{after}}(\text{before})$$ or $$ \rho(\text{after}, \text{before})$$
```ad-example
The expression below is equivalent to `SELECT * FROM Athlete A1, Athlete A2;`

$$\rho(A1, \text{Athlete}) \times \rho(A2, \text{Athlete})$$

![[Pasted image 20240128112520.png|500]]
```

Let's also look at an example in which we rename some columns (instead of relation).

```ad-example
The expression below is equivalent to `SELECT A1.aid, A1.name as name1, A1.sport, A1.country, A2.aid, A2.name as name2, A2.sport, A2.country  FROM Athlete A1, Athlete A2;`

$$\rho(C(A1.\text{name} \to \text{name1}, A2.\text{name} \to \text{name2}), \rho(A1, \text{Athlete}) \times \rho(A2, \text{Athlete}))$$

![[Pasted image 20240128114700.png|500]]
```

This is useful because we do not appreciate duplicate columns in a table. 
- Otherwise each DBMS has their own mechanism to rename, which is often non-SQL standard

---

### `JOIN`: $\bowtie$
The most common way of combining information from two tables. There are three types of `JOIN` s:
- **Conditional Join ($\theta$ -Join)**: $R\bowtie_{c}S=\sigma_{c}(R\times S)$, where $c$ is a condition
- **Equijoin**: $R\bowtie_{c}S$, where $c$ consists only of **equalities** of one or more columns
- **Natural Join**: $R\bowtie S$: Equijoin occurs on all columns with the **same name** (duplicate columns dropped)

```ad-important
Despite equivalence, `JOIN` is faster than [[#Cross-Product (Cartesian)|cross product]].
```

```ad-example
Given the following tables:

![[Pasted image 20240130152334.png|500]]

This is the solution to find names of sailors who have reserved a red boat
$$\pi_{\text{sname}}((\sigma_{\text{color}=\text{'red'}}\text{Boats}) \bowtie \text{Reserves} \bowtie \text{Sailors})$$
which is also equivalent to the following expression 
$$\pi_{\text{sname}}(\pi_\text{sid}((\pi_{\text{bid}}\sigma_{\text{color='red'}}\text{Boats})\bowtie \text{Reserves})\bowtie \text{Sailors})$$

Another example, with step-by-step approach:

![[Pasted image 20240130153545.png|500]]
```

The **query optimizer** chooses from the (equivalent) expressions and chooses one for **efficiency** of evaluation.

---
### Division: $/$
Let $A$ have 2 fields, $x$ and $y$ ; $B$ have only field $y$. Then,
$$A/B=\{x|\forall y\in B, (x,y)\in A\}$$

The result is all the $x$ tuples such that for every $y$ tuples in $B$, there is at an $(x,y)$ tuple in $A$.

The division operator can be equivalently expressed by basic operators:
$$A/B = \pi_{x}(A)-\pi_{x}((\pi_{x}(A)\times B)-A)$$
The underlying idea is to compute all $x$ values that are **NOT disqualified** by some $y$ value in $B$
- By subtracting $x$ that are **disqualified**

```ad-info
Division is useful for queries like "Find the sailors who have reserved all boats".
```

```ad-example
Suppose we want to find names of sailors who have reserved a red **AND** green boat.

**What works**:
- Identify (1) sailors who have reserved red boats, (2) sailors who have reserved green boats, then find the **intersection**

**What DOESNOT work**
- Identify boats that are both red and green, and join it with the sailors table
	- Logically impossible
```
