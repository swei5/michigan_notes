[[2024-01-11]] #Database

### Database
A database is a large, structured collection of data.
- Models data of real-world enterprise, which can include entities and relationships

A **database management system **(DBMS) is a software package designed to store and manage databases. It is used to maintain and query **large datasets**. It provides **efficient**, **reliable**, **convenient**, and **safe multi-user storage** of and **access** to **massive** amounts of **persistent** data.

```ad-summary
DBMS has quite a few advantages:
- Data independence
	- App needs a view of the data, not info about internal representation
- Efficient storage and access
	- Assumption is that data is too **LARGE** to fit in memory
- Centralized data administration
- Data integrity and security
- Concurrent access, recovery from crashes
- Reduced application dev time
```

### Data Models
**Data model** is a **collection of concepts** for describing data. We will look into two main data models today.

```ad-important
**Definition 1.1**: Relational Model

A **relational model** is the most widely used model today.

A relational data model contains a collection of **relations**. A **relation** is a **set of records**, naturally represented as a table with rows and (named, typed) columns. Each table row is also called a **tuple** or **record**.
```

^092f47

```ad-important
**Definition 1.2**: Schema 

A schema is a **description** of data in terms of a data model.
- Every relation has a schema
- Specifies the **name of the relation**, the **name and type of the columns** (or fields or attributes)
```

^0ed388

![[Pasted image 20240111231446.png|500]]

#### Entity-Relationship (ER) Model
A higher-level, user-intuitive model. A ER diagram contains **entities** and **relationships**.

```ad-important
**Definition 1.3**: ER Models 

An Entity-Relationship (ER) model is a “semantic” data model, i.e., a higher-level more user-intuitive model.
- A (relational) DBMS understands only the relational model, so we will translate an ER schema to a relational schema
```

^5cd323

![[Pasted image 20240111231653.png|500]]

In the example above, entities are Student and Course, and relationships are Enrolled in.

##### Schema
Schema is an abstraction. In reality, there are three layers to it.

![[Pasted image 20240111232130.png|500]]

The **physical schema** is then **directly related** to the database itself (layout, indexing, storage, etc.) and does not affect the logical design of the database.

The structure of the database (including the data models) concerns only the conceptual schema.

The **external schema** (views) are subject to some set of **constraints** that are visible to certain group of end-users, and only expose a portion of the database.
- When querying, views are computed on the fly (re-computed every time)

```ad-note
Schemas are defined using **Data Definition Language** (DDL), e.g. `CREATE TABLE X (...)`.

Data is modified/queried using **Data Manipulation Language** (DML), e.g. `SELECT FROM X WHERE (...)`.
```

```ad-example
![[Pasted image 20240111232334.png|500]]
```

Views can be **computed from the relations in the conceptual schema**. Both conceptual and external schema can be made into **tables**.

#### Data Independence 
The idea of having applications **insulated** from data format and storage details.
- **Logical data independence**:  Protection from **changes in logical structure** of data
	- External / Conceptual schema interface
- **Physical data independence**: Protection from changes in physical structure of database
	- Conceptual / Physical schema interface