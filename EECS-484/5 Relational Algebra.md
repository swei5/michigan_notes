 [[2024-01-28]] #Database 

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
### Selection 
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
### Projection 
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

#### Set Difference 
Projection is often used before subtraction.

```ad-example
The expression below is equivalent to `SELECT * FROM name MINUS SELECT * FROM AthleteName;`

$$\text{name}-\text{AthleteName}$$

![[Pasted image 20240128111815.png|500]]
```

#### Cross-Product (Cartesian)
Result is each row of one relation **combined with each row** of the second relation. The result schema will have **ALL fields** from both relations, names inherited by default.

```ad-example
The expression below is equivalent to `SELECT * FROM Athlete, Olympics;`

$$\text{Athlete} \times \text{Olympics}$$

![[Pasted image 20240128112130.png|500]]
```

### Rename Operator 
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