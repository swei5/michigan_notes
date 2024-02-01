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

TODO:

