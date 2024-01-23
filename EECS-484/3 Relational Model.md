[[2024-01-21]] #Database #SQL 

Recall the definition of a relational model: ![[1 DBMS#^092f47]] 
Most DMBS use the relational data model.

### Relational Model 
Every relation has a [[1 DBMS#^0ed388|schema]]. An **instance** is a table (an instance of the schema), with rows (aka tuples, records), and columns (aka fields, attributes) that match the schema.
- Number of rows: **cardinality**
- Number of columns: **degree** or **arity**

```ad-example
The schema to the table below is `(aid: integer, name: string, country: string, sport: string)`.

![[Pasted image 20240121140214.png|500]]
```

### Relational Query Languages
Supports simple, powerful querying of data. Queries are written **declaratively**, in contrast to procedural methods, which allows for mathematical optimization.

SQL is the standard database query language.

```ad-note
**DBMS** is responsible for **efficient** evaluation. The system can **optimize** for efficient query execution, and still ensure that the answer **DOES NOT** change.
```

#### `CREATE`, Integrity Constraints
`CREATE` follows the syntax below:

```sql
CREATE TABLE table_name (
	field1 TYPE,
	field2 TYPE,
	... ... ...
);
```

**Integrity constraints** must hold true for **every instance** of the database.
- ICs are specified when **schema is defined** 
- ICs are checked whenever relations are modified

A legal instance of a relation satisfies **ALL specified ICs**. DBMS **must NOT** admit illegal instances.

```ad-example
Examples of ICs include:
- Specify certain attributes as keys of a table
- Reference keys of entities in other tables
- No dangling references are allowed in a database when updates occur to any table
```

##### Primary and Candidate Keys
Recall the definition of **keys** ![[2 Entity-Relationship Model#^76b380]]
Equivalently, no two tuples in (any instance of) $R$ have the same key attributes $A_{1}, \cdots, A_{n}$.

A **superkey** is a key that only satisfies the uniqueness requirement, but no minimal requirement. 

```ad-note
Every key is a superkey.
```

