[[2024-02-13]] #Database #SQL

### Overview
- *Traditional RDBMS* 
	- Most focus on this course
- **NoSQL**
	- Key-value stores (hash tables)
	- Document stores (MongoDB)
	- MapReduce (similar to `GROUP BY` in SQL)
- Modern RDBMS (performance improvement strategies, based on workload)
	- In-memory DBs
	- Columnar DBs (DuckDB)
	- Approximate DBs

### NoSQL 
Not every data problem is best solved by traditional relational databases: NoSQL stands for **not only SQL**.
- Still supports SQL languages 

```ad-summary
**NoSQL, Pros and Cons versus traditional RDBMS**

![[Pasted image 20240214140301.png|500]]

Scaling up traditional RDBMS could be really expensive - NoSQL provides a more flexible solution.
```

---
### Key-Value Stores 
The simplest way to store data is `(key,val)` pairs. API tools include `get(key)`, `put(key, value)`, and `delete(key)`, for instances.
- Redis, Amazon DynamoDB, Azure Cosmos DB, RocksDB

#### Wide Column Stores
Another way of key-value stores is **Wide Column Stores**, which support tables by storing `(key,colname,val)` triplets. Unlike a relational DB, the **names** and **format** of the columns can **vary from one row to another** within the same table.
- Cassandra, BigTable, HBase

**Not ALL** columns are required in all rows. It is **schema-free**. Typically, there is a **very large number of columns** (possibly millions).

![[Pasted image 20240216164041.png|600]]

---
### Document Stores 
Uses `(key, document)` pairs to store data. A document is usually in `JSON` format (or `XML`). In addition to key, they can also **fetch documents by their content**.