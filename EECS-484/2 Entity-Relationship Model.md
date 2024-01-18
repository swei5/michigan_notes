[[2024-01-16]] #Database 

Database models determine how data can be **stored**, **organized** and **manipulated** in a database system. Today we will delve into the **[[1 DBMS#^5cd323|Entity Relationship (ER) Model]]**, which is most useful for end-users and database designers.

### Creating ER Diagram 
#### Basics
Key legends as followed:
- Square ▭ : entities
- Diamond ♢ : relationships
- Oval ⃝ : attributes

An **Entity set** is a collection of entity instances.
- Set of Actors or set of Movies
A **relationship set** is a set containing all related entities for a relationship.
- Acts_in: $\{(a1,m1), (a1,m2), \cdots\, (a2, m3)\}$

```ad-example
![[Pasted image 20240116122525.png|500]]
```

#### Adding Constraints
An **arrow** indicates a **key constraint** on a relationship and an entity.
- Many-to-many
	- E.g. *Acts-in* is a many-to-many relationship as an actor can act in multiple movies and a movie can have multiple actors
- One-to-many or many-to-one
	- E.g. *Directed-by* is a many-to-1 relationship because a movie has at most 1 director
- One-to-one

Lines indicate **participation constraints** between two entities. 
- A **heavy line** indicates **EVERY** entity-1 entity **must-participate** in a relationship with an entity-2 entity 
- A **light line** indicates an entity-2 entity can be related to 0 or more entity-1 entities

To sum these up, we now look at another example.

```ad-example

![[Pasted image 20240116161347.png|500]]

The updated diagram specifies that
1. Key contraint: a movie can only be directed by at most one director (many-to-1)
2. Participation constraint: every movie must have at least one actor
```

#### Attributes 
A **key** is a **MINIMAL set** of one or more attributes that has **unique value for each record**.
- Candidate keys are potential keys
	- E.g. students in a student database have multiple potential keys such as student ID, log name, SSN, etc.

```ad-warning
A unique set of attributes doesn't make it the key of the entity unless it's also the **MINIMAL** set.
```

```ad-important
**Definition 2.1**: Primary Key

A **primary key** is one of the candidate keys and is **underlined** in the ER Diagram. When you design a database, the primary key is **cross-referenced** in other tables to represent relationships.
```

Often each entity is assigned a **unique ID**, which serves as a **primary key**. 

![[Pasted image 20240116164900.png|500]]

```ad-note
Keys of the entities define the relationship in which they participate in. In other words, the records with these keys in the relationship should be unique. 

Adding records with duplicate keys with additional attributes requires a redesign of the diagram. 
```

```ad-summary
**Steps to ER Modeling**
1. Identify **entities** (objects), **relationships** between entities 
2. Attach **attributes**  
3. Keys: something that uniquely identifies an entity
```

```ad-note
An entity can be **related to itself**.
```

#### Weak Entities 
A **weak entity** can be identified **uniquely** only by considering **some of its attributes in conjunction with the primary key** of the another entity (identifying owner).

```ad-summary
**Weak Entities Rules**
- Weak entity has a single owner (**one-to-many** relationship)
- Weak entity must have **total participation** in the **above identifying relationship set**.
```

```ad-example
In this example, the ER Diagram illustrates that candidates have experts on their staff, identified by their name (partial key).
- Expert names are not globally unique. To identify an expert, we need candidate’s ID + expert’s name

![[Pasted image 20240117130824.png|500]]
```

#### ISA Hierarchies
Attributes are **inherited**. If A is-a B, **every** A entity is also a B entity.
- Specialize superclass (top-down design)
- Generalize subclasses (bottom-up design)

![[Pasted image 20240117131128.png|500]]

There are two sets of constraints in such hierarchy:
1. **Overlap**
	- Can **more than one** subclasses contain the same entity?
	- Overlapping v.s. disjoint
1. **Covering**
	- Do the entities in the subclasses **include ALL the entities** in the superclass?
	- Total v.s. partial

#### Relationships with Relationships 
Treating a relationship as **an entity for another relationship** is called **aggregation**.

```ad-example
In this example, the sponsoring relationship is viewed as an entity. Specifically, each Project must be sponsored by at least one department, and each sponsoring relationship must be monitored by exactly one manager.

![[Pasted image 20240117153622.png|500]]
```

---
### Designing ER Model 
There are three main questions we want to ask ourselves in laying out the design:
1. Model a concept as an **entity** or an **attribute**?
2. Model a concept as an **entity** or a **relationship**?
3. **Binary** or **ternary** relationship? **Aggregation**?

#### Entity v.s. Attribute 
Go with entity if you want to:
- Store several characteristics per concept, or
- Encode the structure of a concept 
	- E.g. city, street of a location