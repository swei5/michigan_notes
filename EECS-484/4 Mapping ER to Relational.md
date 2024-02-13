[[2024-01-27]] #Database #SQL

### Relationship Sets to Tables 
The primary key (s) of a relationship set are specified by the **underlined attributes** in the participating entity sets, whereas foreign keys are referenced from participating entity sets.

Given the following ER Diagram:

![[Pasted image 20240127215819.png|500]]

The relational table should look like the following:

```sql
CREATE TABLE Votes (
	citizenid INTEGER,
	iid INTEGER, 
	when DATE,
	PRIMARY KEY (citizenid, iid), 
	FOREIGN KEY (citizenid),
		REFERENCES Citizens(cid), 
	FOREIGN KEY (iid)
		REFERENCES Initiative
);
```

Note that we specify `cid` in the `REFERENCES` clause because we rename `cid` to `citizenid` in this example.

#### Key Constraint
Now imagine the following scenario:

![[Pasted image 20240127220529.png|500]]

Note that we now have a [[2 Entity-Relationship Model#^221638|key constraint]] (citizens many to one initiative relation). To represent this entity-relationship set, we could use **Approach One**: three tables (citizens, votes, initiatives). Note that we may also need to edit the `votes` table:

```sql
CREATE TABLE Votes (
	cid INTEGER, -- cid CANNOT be NULL, implies by primary key constraint
	iid INTEGER NOT NULL, -- doesn't make sense to have NULL initiative
	when DATE,
	PRIMARY KEY (cid), 
	FOREIGN KEY (cid),
		REFERENCES Citizens, 
	FOREIGN KEY (iid)
		REFERENCES Initiative
);
```

The reason why we are using `cid` as the primary key of this relationship (instead of both primary keys in the previous example) is because of the key constraint - a citizen can be related to **AT MOST ONE** initiative - allowing for unique identification.

Even better, we can represent this with **Approach TWO**: just two tables (folded. Again, thanks to the key constraint, we can effectively meld `Citizens` and `Votes` down to one table:

```sql
CREATE TABLE Citizen_Votes (
	cid INTEGER, -- cid CANNOT be NULL
	name CHAR, 
	bday DATE,
	when DATE, -- CAN be NULL
	iid INTEGER, -- CAN be NULL (FK constraint DN apply)
	PRIMARY KEY (cid), 
	FOREIGN KEY (iid)
		REFERENCES Initiative
);
```

```ad-question
Can we just simply fold everything into ONE table?

**NO**! This is bad design. Specifically, for every citizen that votes for some initiative, we have to store `iname` and `idesc` information for *that initiative*. This is **REDUNDANT** because we really don't need as many columns.
```

Removing such redundancy is important. This process is called **normalization**, and ER diagram helps us a bunch in that.

```ad-summary
The tradeoff between using separate tables and folded tables lies in the tradeoff between storage efficiency and computational cost.
- In separate tables, storage is more efficient as we would simply store the objects under each entity
- In folded tables, some fields might be `NULL` due to relationship between the two entities, yet it contains more information regarding the relationship which saves computing costs
```
#### Participation Constraint 
Now imagine the following scenario (participation + key constraint):

![[Pasted image 20240127224651.png|500]]

Note that approach 1 (separate tables) simply doesn't work because there is no way to guarantee that every citizen in the `Citizens` table also appears in the `Votes` table.
- That is, after an insertion to the `Citizens` table, we would need a cross-table check to ensure the newly added `citizen` is also added to the `Votes` table
	- Difficult to hard to assert it in SQL and computationally expensive (using a trigger)

In this case, only approach 2 would work fine; however, this time we need to specify `iid` to be `NOT NULL` to suffice the participation constraint:

```sql
CREATE TABLE Citizen_Votes (
	cid INTEGER, -- cid CANNOT be NULL
	name CHAR, 
	bday DATE,
	when DATE, -- CAN be NULL
	iid INTEGER NOT NULL, -- each citizen must votes to one
	PRIMARY KEY (cid), 
	FOREIGN KEY (iid)
		REFERENCES Initiative
);
```

Let's again look at a more general picture:

![[Pasted image 20240127225533.png|500]]

To represent this **one-to-one** relationship, we would only need one table that looks like the following:

```sql
CREATE TABLE RAB (
	r1 Integer,  
	a1 Integer,  
	a2 Integer,  
	a3 Integer,
	b1 Integer NOT NULL,
	b2 Integer,
	b3 Integer,
	UNIQUE (b1), 
	PRIMARY KEY (a1)
);
```

```ad-warning
Note that in this case we **CANNOT use** both keys $(A, B)$ as the primary keys of the table as it *implies* that we could theoretically have $(a_1,b_1)$ and $(a_1,b_2)$ in the table, where $a \in A$, $b \in B$; however, this should not be allowed in the first place given the key constraint (table $A$ has a many-to-one relationship to table $B$).
```

Equivalently, we can specify `b1` as the primary key and thus mark `a1` as unique.

```ad-note
Generally, when there is a participation constraint, we do not want to keep the tables separate; rather, combine them based on that constraint.
```

##### Participation Constraint Only 
If we have a participation constraint only in one of the relationship, it could be tricky.

![[Pasted image 20240127230318.png|500]]

If we use approach 2 (folded table), we may need to have multiple `citizens` in the same table as there could be citizens that vote more than once, which **fails** the primary key constraint.

This leaves us with only approach 1. As mentioned previously, we would have to cross-check the two tables, `Citizens` and `Votes`, which is rather inefficient. The typical practical solution here would be to **IGNORE participation constraint** and simply don't enforce them.

#### Weak Entities 
In the case of weak entities, we want to use approach 2 (remember participation constraint).

![[Pasted image 20240127231050.png|500]]

```sql
CREATE TABLE Expert_Staff (
	ename CHAR(20),
	jobDesc CHAR(40),
	salary REAL,
	cid INTEGER,
	PRIMARY KEY (ename, cid), 
	FOREIGN KEY (cid) REFERENCES PR-Cands ON DELETE CASCADE
);
```

The `ON DELETE CASCADE` ensures that the DBMS delete all weak entities when an owner entity is deleted.

#### ISA Hierarchy, Aggregation 
Let's look at a specific ISA hierarchy example:

![[Pasted image 20240127234157.png|500]]

In this example, we want to have three tables, `Citizen`, `PR-Cand`, and `President`. `ssn` will be the primary key for all three tables and will be the **foreign key** for `PR-Cand` and `President` (foreign key constraint applies).

Often, we would need to *join* the `PR-Cand` table and `Citizen` table to look up information about `PR-Cand`, for instance. But more on this later.

In aggregation, we would simply treat a relationship as one single entity and apply the same thought process as above.

```ad-summary
**Relational Model Design**, by entity relationships
- **No constraints**
	- Approach 1
- **At most 1 **(key constraint but no participation constraints)
	- Approach 1
	- Approach 2
- **Exactly 1** (key constraint **AND** participation constraint)
	- Approach 2, `FOREIGN KEY NOT NULL`
- **1 or more** (participation constraint only)
	- Approach 1 (forgo participation constraint)
```

#### General Constraints 

^d1927a
Constraints that are more general than key constraints, and we can always use a query to express such constraints.

```sql
CREATE TABLE {tb} (
	...,
	age INTEGER,
	CHECK (age >= 18 AND age <= 80)
);
```

General constraints are checked each time a table is **updated**. 

```ad-note
A `CHECK` constraint is always true for **empty** relation.
```

SQL standard also allows **cross-table** `CHECK` constraints. But, they are not supported in most systems – expensive to enforce.

#### Active Databases, Triggers
A trigger is a procedure that **starts automatically** if specified changes occur to the DBMS. It is consisted of three parts:
- **Event** (activates the trigger)
- **Condition** (test that is run when the trigger is activated)
- **Action** (what happens if the trigger runs)

There are two types of trigger execution:
- **Row-level Triggers**: Once per modified row
	- Useful in auditing, creating a log of updates, security checks
- **Statement-level Triggers**: Once per SQL statement

Let's look at an example; this is a row-level trigger (triggered per row update). If new salary above 1000, log entry is added.

Below is an example of a row-level trigger.

```sql
CREATE OR REPLACE TRIGGER Log_salary_increase AFTER UPDATE ON Employee FOR EACH ROW WHEN (new.Sal > 1000)  
BEGIN
	INSERT INTO Emp_log (Emp_id, Log_date, New_salary, Action) 
		VALUES (:new.Empno, SYSDATE, :new.SAL, 'NEW SAL’);
END;
```

Remember `CASCADE` (foreign key constraints)? We can also use a trigger to achieve the same result:

```sql
CREATE TABLE Compete (aid INTEGER,
	oid INTEGER,  
	PRIMARY KEY (aid, oid),  
	FOREIGN KEY (aid) REFERENCES Athlete, 
	FOREIGN KEY (oid) REFERENCES Olympics
);

CREATE OR REPLACE TRIGGER cascade_on_delete 
AFTER DELETE ON Athlete  
FOR EACH ROW  
BEGIN
	DELETE FROM Compete
	  WHERE Compete.aid = :OLD.aid;
END;
```

```ad-summary
**Triggers**: Pitfalls and Pain 
- Triggers can be **recursive**
	- Chain of triggers can be hard to predict, which makes triggers difficult to understand and debug
- Errors with “mutating” table 
	- A mutating table is a table that is currently being modified by an `UPDATE`, `DELETE`, or `INSERT` statement, or a table that might be updated by the effects of a DELETE CASCADE constraint
	- The session that issued the triggering statement **CANNOT** **query** or **modify** a mutating table
```

Avoid using triggers unless absolutely necessary.