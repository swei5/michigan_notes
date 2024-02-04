[[2024-01-30]] #Database #SQL 

### Structured Query Languages 
- **Data Definition Language** (DDl)
	- Create a table: `CREATE`
- **Data Manipulation Languages** 
	- Add new records: `INSERT`
	- Retrieve records: `SELECT`
	- Update records: `UPDATE`
	- Delete records: `DELETE`
	- Update a view: `UPDATE`
- **Views**: creating and dropping

---
### Basic SQL Query 

```sql
SELECT [DISTINCT] <attributes>
FROM <relations>
WHERE <qualification>
```

The conceptual evaluation here is
1. Take cross-product of relation-list
2. Select rows satisfying qualification 
3. Project columns in the list of attributes
	- Eliminate duplicates **ONLY IF** `DISTINCT`
		- By default, SQL allows duplicate rows in the result (multi-set semantics), unlike ER diagrams and Relational Algebra, for efficiency

Again, the optimizer chooses the efficient plan.

#### Range Variables 
The named aliases of relations - make things easier to read and is good style to use.

```sql
SELECT S.sname  
FROM Sailors S, Reserves R  
WHERE S.sid = R.sid AND R.bid = 103;
```

```ad-important
Range variables are always needed when same relation appears twice in `FROM` clause.
```

For example:
```sql
SELECT 
	S1.sname, 
	S2.sname
FROM Sailors S1, Sailors S2
WHERE S1.age > S2.age;
```

```ad-example
This query below finds pairs of sailors where the first one has half the rating of the second one.

```sql
SELECT S1.sname AS name1, S2.sname AS name2
FROM Sailors S1, Sailors S2
WHERE 2*S1.rating = S2.rating;
```

#### `INSERT INTO` with `SELECT`
The goal here is to populate a table from **existing data**. For instance,

```sql
INSERT INTO MySailors
SELECT S.sid, 
	S.sname, 
	S.rating
FROM Sailors S
WHERE S.sname IS NOT NULL;
```

#### `ORDER BY`
**Attribute**(s) in the `ORDER BY` clause **MUST BE** in `SELECT` list.

```ad-example
This query below finds the names, ages, and rankings of all sailors. Sort the result in increasing order of age. If there is a tie, sort those tuples in decreasing order of rating.
```sql
SELECT S.sname, S.age, S.rating
FROM Sailors S
ORDER BY S.age ASC, S.rating DESC
```

#### Set Operators 

##### `UNION`
Eliminates duplicates.

```ad-example
This query below finds the names of sailors who have reserved a red or a green boat.
```sql
SELECT S.sname
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid AND R.bid = B.bid AND B.color = 'red'
UNION
SELECT S.sname
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid and R.bid = B.bid AND B.color = 'green';
```

```ad-note
`UNION ALL`, however, **KEEPS** the duplicates.
```

##### `INTERSECT`
We can sometimes use an `INNER JOIN` to achieve the same outcomes. 

Suppose that we need to find names of sailors who have reserved a red and a green boat. It might be tempting to think:

```sql
SELECT S.sname
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid AND R.bid = B.bid AND B.color = 'red'
INTERSECT
SELECT S.sname
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid and R.bid = B.bid AND B.color = 'green';
```

However, note that we are intersecting on a **non-key attribute** `S.name`, instead of `S.sid`. The solution to this problem would be to (1) **intersect on keys**, then (2) **map** the `sid` values to names. Hence, we would in fact need:

```sql
CREATE VIEW RedGreenSailors AS
SELECT  S.sid
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid AND R.bid = B.bid AND B.color = 'red'
INTERSECT
SELECT S.sid
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid and R.bid = B.bid AND B.color = 'green';

SELECT S.sname
FROM Sailors S, RedGreenSailors R
WHERE S.sid = R.sid;
DROP VIEW RedGreenSailors;
```

Views are used for creating **external schemas or temporary schemas** on top of conceptual schema. 

Or just use a **subquery** with an inline name:

```sql
SELECT S.sname  
FROM Sailors S,  
	(
		SELECT S.sid  
		FROM Sailors S, Reserves R, Boats B  
		WHERE S.sid = R.sid AND R.bid = B.bid AND B.color = 'red' 
		INTERSECT
		SELECT S.sid  
		FROM Sailors S, Reserves R, Boats B  
		WHERE S.sid = R.sid and R.bid = B.bid AND B.color = 'green'
	) RedGreenSailors
WHERE S.sid = RedGreenSailors.sid;
```

```ad-important
It's never a bad practice to use set operators on the **KEYS** of the tables, as they guarantee unique and definite identification.
```
#### Set Comparison Operators 
Set comparisons can be made with 
- `<attr> IN <R>`: true if `R` contains `attr`
- `EXISTS <R>`: true if `R` is **NOT an empty relation**
- `UNIQUE <R>`: true if **NO duplicates** in `R`
- `NOT`: negation

Operations that are available for `ANY` or `ALL` include:
- `<attr> <op> ANY <R>`: some element of `R` satisfies the condition that `attr` `op` that element
- `<attr> <op> ALL <R>`: **ALL** element of `R` satisfies the condition that `attr` `op` that element

`<op>` can take value in `<, >, <>, <=, >=, =`. For these operators to work, the relation `R` needs to hold only the same type of attribute as `attr`.

```ad-note
If `ALL` returns an **EMPTY query**, then `<op> ALL` evaluates to `TRUE` for every tuple `attr`.
```

```ad-warning
Query optimizers **not as good** at optimizing queries across nesting boundaries. Hence, try hard first to write non-nested using joins.
```

For instance, instead of writing a nested query such as
```sql
SELECT S.sid
   FROM Sailors S
   WHERE S.rating > ANY (SELECT S2.rating
	   FROM Sailors S2
	   WHERE S2.name = ‘John’
	);
```

It's better to write the equivalent without a nested query:

```sql
SELECT S.sid
FROM Sailors S, Sailors S2
WHERE S.rating > S2.rating AND S2.name = ‘John’;
```

#### Aggregate Operators 
Includes functions like
- `COUNT(*)`
- `COUNT([DISTINCT] A)`
- `SUM([DISTINCT] A)`
- `AVG([DISTINCT] A)`
- `MAX(A)`
- `MIN(A)`

##### `GROUP BY`, `HAVING`
Partitions data into **groups** according to some criterion, and evaluate the **aggregate** **FOR EACH** group.

The general syntax is as followed:

```sql
SELECT [DISTINCT] <attr>
FROM <R>
WHERE <qualification> GROUP BY <grouping-list>
HAVING <grouping-qualification>
```

The conceptual evaluation, in steps, is:
1. Eliminate tuples that don’t satisfy qualification (`WHERE`)
2. Partition remaining data into groups `GROUP BY`
3. Eliminate groups according to group-qualification `HAVING`
	- Whatever that goes with `HAVING` must be a **group property** to make sense
1. Evaluate aggregate operation (s) for each group

```ad-note
The target list from this query contains:
1. Attribute names (a subset of the grouping-list), and 
2. Aggregate operations
```

#### `NULL` Values 
Recall the definition of `NULL` values: ![[3 Relational Model#^5faaba]]
The boolean operations that involve `NULL` are somewhat complicated; we can refer to this three-valued logic table below:

![[Pasted image 20240203180911.png|400]]

```ad-important
`WHERE` clause eliminates rows that **DON'T** evaluate to `TRUE`.
```

```ad-example
The following query will only return sailors' names whose age is **NOT equal** to 45, meaning that if one sailor has a `NULL` age they would be eliminated based on the table above: $U \lor U$ $\to$ $U$.

```sql
SELECT sname
FROM sailors
WHERE age > 45 OR age <= 45
```

`AVG` will discard `NULL` values but `COUNT` will include.

#### `OUTER JOIN`
Includes options like `LEFT`, `RIGHT`, `FULL`, and `NATURAL`.

`NATURAL JOIN` is a join operation that creates a join on the base of the **common columns** in the tables.

#### Views, `WITH`
We can define a view by using 

```sql
CREATE VIEW <name> AS
 ...
;

-- Query 

DROP <name>; -- Since it's a temp view
```

Or we can use the `WITH` keyword to generate a temporary table. Both of these are **BETTER** than subqueries as they don't require nested computation.