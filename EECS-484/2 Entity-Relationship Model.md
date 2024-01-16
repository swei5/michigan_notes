[[2024-01-16]] #Database 

Database models determine how data can be **stored**, **organized** and **manipulated** in a database system. Today we will delve into the **Entity Relationship** (ER) Model, which is most useful for end-users and database designers.

### Creating ER Design 
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
An **arrow** indicates key constraint on directed-by relationship.
- Many-to-many
- One-to-many or many-to-one
- One-to-one