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
A **relationship set** is a **set** containing all related entities for a relationship.
- Acts_in: $\{(a1,m1), (a1,m2), \cdots\, (a2, m3)\}$

```ad-important
A relationship **IS A SET** - this implies we cannot have multiple, duplicate elements from entities in it.
```

```ad-example
![[Pasted image 20240116122525.png|500]]
```

---
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

```ad-warning
Arrows can only come **INTO** a relationship, not **OUT OF IT**.
```

---
#### Attributes, Keys
A **key** is a **MINIMAL set** of one or more attributes $A_{1}, \cdots, A_{n}$ that has **unique value for each record**. ^76b380
- Candidate keys are potential keys
	- E.g. students in a student database have multiple potential keys such as student ID, log name, SSN, etc.

**Attributes** are unique to a single object of an entity.
- E.g. Student s (ID: 1) cannot have more than 1 name

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
Keys of the entities **define the relationship** in which they participate in. In other words, the records with these keys in the relationship should be unique. 

Adding records with duplicate keys with additional attributes requires a redesign of the diagram. 
```

```ad-note
An entity can be **related to itself**.
```

---
#### Weak Entities 
A **weak entity** can be identified **uniquely** only by considering **some of its attributes (partial key) in conjunction with the primary key** of the another entity (identifying owner).
- If the object in the other entity gets deleted, then its corresponding weak entity also disappears
- Partial keys **MUST BE UNIQUE**

```ad-summary
**Weak Entities Rules**
- Weak entity has a **single owner** (**one-to-many** relationship)
- Weak entity must have **total participation** in the **above identifying relationship set**.
```

```ad-example
In this example, the ER Diagram illustrates that candidates have experts on their staff, identified by their name (partial key).
- Expert names are not globally unique. To identify an expert, we need candidate’s ID + expert’s name

![[Pasted image 20240117130824.png|500]]
```

```ad-warning
In exam, make sure we **bold out** the weak entity.
```

---
#### ISA Hierarchies
Attributes are **inherited**. If A is-a B, **every** A entity is also a B entity.
- Specialize superclass (top-down design)
	- Subclasses have the attributes of their superclasses
- Generalize subclasses (bottom-up design)

![[Pasted image 20240117131128.png|500]]

There are two sets of constraints in such hierarchy:
1. **Overlap**
	- Ask: *Can **more than one** subclasses contain the same entity?*
	- Overlapping v.s. disjoint: two subclasses are **overlapping** if they can contain the same entity, otherwise they are **disjoint**
		- E.g. president and president candidate are overlapping 
		- E.g. undergrad and graduate students are disjoint
1. **Covering**
	- Ask: *Is the union of all the subclasses the same as the super class?*
	- Total v.s. partial
		- E.g. All students are either undergrad or graduate students: total 
		- E.g. **NOT all** citizens are either presidential candidates or presidents

---
#### Aggregations
Treating a relationship as **an entity for another relationship** is called **aggregation**.
- An aggregated entity is **defined by the relationship**, which is defined by keys of the entities which partake in the relationship

```ad-example
In this example, the sponsoring relationship is viewed as an entity. Specifically, each project** must be sponsored by at least one department**, and each sponsoring relationship must be monitored by exactly one manager.

![[Pasted image 20240117153622.png|500]]
```

---
### Designing ER Model 
There are three main questions we want to ask ourselves in laying out the design:
1. Model a concept as an **entity** or an **attribute**?
2. Model a concept as an **entity** or a **relationship**?
3. **Binary** or **ternary** relationship? **Aggregation**?

#### Entity v.s. Attribute 
Go with an entity if you want to:
- Store several characteristics per concept, or
- Encode the structure of a concept 
	- E.g. city, street of a location
- Duplicate attributes needed

```ad-summary
**Steps to ER Modeling**
1. Identify **entities** (objects), **relationships** between entities
	- Handle ISA relationship
1. Attach **attributes**
	- Determine if they belong to entities or relationships 
	- Determine primary keys
1. Resolve constraints 
	- Handle key and participation constraints 
	- Determine what weak entities exist 
	- Check relationships for potential ternary relationships 
```

