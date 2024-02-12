[[2024-01-21]] #Database #SQL 

Recall the definition of a relational model: ![[1 DBMS#^092f47]] 
Most DMBS use the relational data model.

### Relational Model 
Every relation has a [[1 DBMS#^0ed388|schema]]. An **instance** is a table (an instance of the schema), with rows (aka tuples, records), and columns (aka fields, attributes) that match the schema.
- Number of rows: **cardinality**
- Number of columns: **degree** or **arity**

A relation is **defined by a schema and an instance**.

```ad-example
The schema to the table below is `(aid: integer, name: string, country: string, sport: string)`.

![[Pasted image 20240121140214.png|500]]
```

#### Classical Relational Model 
Requires that **EVERY row** is **UNIQUE** (set semantics). 
- In modern system SQL, a multi-set semantics is adapted which means **DUPLICATE rows** are allowed.

---
### Relational Query Languages
Supports simple, powerful querying of data. Queries are written **declaratively**, in contrast to procedural methods, which allows for mathematical optimization.

SQL is the standard database query language.

```ad-note
**DBMS** is responsible for **efficient** evaluation. The system can **optimize** for efficient query execution, and still ensure that the answer **DOES NOT** change.
```

---
### Integrity Constraints

```ad-important
**Definition 3.1**: Integrity Constraint

**Integrity constraints** must hold true for **every instance** of the database.
- ICs are specified based on application semantics when **schema is defined** 
- ICs are checked whenever relations are **modified**

```

Types of ICs include
- Domain constraints (type)
- Primary key constraints 
- Foreign key constraints 
- [[4 Mapping ER to Relational#^d1927a|General constraints]]

A legal instance of a relation satisfies **ALL specified ICs**. DBMS **must NOT** admit illegal instances.

```ad-example
Examples of ICs include:
- Specify certain attributes as keys of a table
- Reference keys of entities in other tables
- No dangling references are allowed in a database when updates occur to any table
```

#### `CREATE`, Domain Constraints
 `CREATE` follows the syntax below:

```sql
CREATE TABLE {tb} (
	{field1} {TYPE},
	{field2} {TYPE},
	... ... ...
);
```

**Domain constraint** (type) enforced when tuples **added** or **modified**.

```ad-warning
Each table **MUST HAVE** domain constraints.
```

#### Primary Key Constraint
Recall the definition of **keys**: ![[2 Entity-Relationship Model#^76b380]]
Equivalently, no two tuples in (any instance of) $R$ have the same attributes $A_{1}, \cdots, A_{n}$.

```ad-important
**Definition 3.2**: Primary Key Constraint

By SQL standard, a **primary key **attribute** CANNOT** be `NULL`. Primary key is often an **integer ID** for efficiency.
```

^eff39f

A **superkey** is a key that only satisfies the **uniqueness requirement**, but no minimal requirement.  ^5bf6f5

```ad-note
Every key is a superkey.
```

There are two ways to specify a primary key constraint:
```sql
CREATE TABLE {tb} (
	{id} INTEGER PRIMARY KEY,
	...
);
```

Or

```sql
CREATE TABLE {tb} (
	{id} INTEGER
	...,
	PRIMARY KEY ({id})
);
```

We can **disallow null** values for an attribute by appending the `NOT NULL` keyword. 

```sql
CREATE TABLE {tb} (
	...,
	{field1} CHAR(30) NOT NULL,
	...
);
```

`NULL` value implies the value is **unknown** or **inapplicable**. ^5faaba

```ad-note
The design consideration here is that primary keys should be long-term stable, unique, and, ideally, not sensitive. Avoid addresses, SSN, etc.
```

#### Candidate Keys
Candidate keys specified using `UNIQUE`. One of the candidate keys is chosen as the primary key.

```sql
CREATE TABLE a (
	...,
	{field1} CHAR(30),
	{field2} VARCHAR(200),
	UNIQUE ({field1}, {field2}),
	...
);
```

`UNIQUE` attributes **CAN** be `NULL`, unless `NOT NULL` is specified.
- **Only the NON-`NULL`** attribute values need to be `UNIQUE`.

#### Foreign Key Constraint and Referential Integrity
Enforcing foreign key constraints implies **referential integrity** (no dangling references) is achieved.

```sql
CREATE TABLE {tb} (
	...,
	FOREIGN KEY ({key}) REFERENCES {tb2},
	...
);
```

Whenever we modify the database, the DBMS **must check for violations** of ICs. In simple terms, the DBMS rejects offending `UPDATE` or `INSERT` command.
- Primary key and unique ICs are intuitive

If a complete tuple is inserted with **NO corresponding** foreign key in the referenced table, `INSERT` is also rejected.
- `NULL` is accepted

```ad-summary
In general, when it comes to enforcement of referential integrity in the case of deleting a tuple, SQL uses the following rules:
- No Action/Restrict: disallow action (default)
- `CASCADE`: deletes all **referencing tuple**
- `SET NULL`: sets foreign key value of referencing tuple to NULL or a default value
```

```ad-important
An IC is a statement about all possible instances. We can check a database instance to** see if an IC is violated**, but we can **NEVER** infer that an IC is **TRUE** by looking at an instance.
```

### Destroying & Altering Relations 
To destroy the relation, we use `DELETE TABLE {tb}`. Schema information and tuples are deleted.

To alter the schema by adding a new column, we use `ALTER TABLE {tb} ADD COLUMN {colname}: {TYPE}`.
- The new field are put as `NULL`
