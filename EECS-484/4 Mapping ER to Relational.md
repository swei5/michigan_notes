[[2024-01-27]] #Database 

### Relationship Sets to Tables 
The primary key (s) of a relationship set are specified by the **underlined attribute**s in the participating entity sets, whereas foreign keys are referenced from participating entity sets.

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
