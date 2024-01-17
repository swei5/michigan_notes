[[2024-01-16]] #Database 

Database models determine how data can be **stored**, **organized** and **manipulated** in a database system. Today we will delve into the **Entity Relationship** (ER) Model, which is most useful for end-users and database designers.

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
	- *Acts-in* is a many-to-many relationship as an actor can act in multiple movies and a movie can have multiple actors
- One-to-many or many-to-one
	- *Directed-by* is a many-to-1 relationship because a movie has at most 1 director
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
A **key** is a **minimal set** of one or more attributes that has **unique value for each record**.
- Candidate keys are potential keys
	- E.g. students in a student database have multiple potential keys such as student ID, log name, SSN, etc.

A **primary key** is one of the candidate keys and is **underlined** in the ER Diagram. When you design a database, the primary key is **cross-referenced** in other tables to represent relationships.

Often each entity is assigned a **unique ID**, which serves as a **primary key**.

![[Pasted image 20240116164900.png|500]]

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
- Weak entity has a single owner (one-to-many relationship)
- Weak entity must have **total participation** in the **above identifying relationship set**.
```