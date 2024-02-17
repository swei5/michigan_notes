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
- Allows for flexibility but is not really efficient due to sparseness

![[Pasted image 20240216164041.png|600]]

---
### Document Stores 
Uses `(key, document)` pairs to store data. A document is usually in `JSON` format (or `XML`). In addition to key, they can also **fetch documents by their content**. It allows multiple users by **sharding**.
- MongoDB, CouchDB, DynamoDB (as well), DataBricks
- Used for in **analytics** rather than transactional tables

```ad-note
We can think of the DBMS as providing a **distributed hash table** (DHT) – something that allows look up of a document given a key.
```

#### Documents, Collections 
A document corresponds to a **tuple** in a relation.

```json
{
	<key1>: <value1>,
	<key2>: <value2>,
	...
}
```

Documents are simply JSON objects in JavaScript syntax, consisting of field-value pairs. A value can also be an object.

A collection corresponds to a **table** in relational databases. It is a **set of documents**. **Common structure** among documents in the same collection is **NOT enforced**.

```json
{
	<key1>: <value1>,
	<key2>: <value2>,
	...
},
{
	<randomkey>: <randomval>,
	...
}
```

### MongoDB 
Key commands in Mongo include:

```javascript
<dbname>.<collectionname>.insert(<object>)
```

Which is equivalent to `SQL INSERT`. Andrewdeorio 

```javascript
<dbname>.<collectionname>.find(<predicate>)
```

Which is equivalent to `SQL SELECT <col> WHERE <cond>`. This returns a **cursor object** and prints 10 tuples at a time. 
- We can type `it` to see additional tuples 
- We can also save the results to JavaScript variables: `var x = db.users.find();`

We can also iterate over a cursor object:

```javascript
var mycursor = db.users.find(); 
while (mycursor.hasNext()) {
	var w = mycursor.next(); // next document
	print(w.user_id, w.DOB); // print fields
}
// mycursor now points to the end.
```

The predicate used in `find` is an object (part or all of a tuple in a document). For instance, if we want to find users born on 21st Nov. In state ”Rohan”, we can put 

```javascript
var mycursor = db.users.find({"DOB" : 21, "MOB" : 11, "hometown.state" : "Rohan"});
```

Notice the `hometown.state` is actually a nested field - we 

`FIND` can also include **projections**. If we want to find `first_name` and `last_name` of users born in `state` Rohan on Nov. 21st:

```javascript
var mycursor = db.users.find({"DOB" : 21, 
							  "MOB" : 11, 
							  "hometown.state" : "Rohan"}, 
							{first_name : 1, last_name : 1}); // 1 = include
```

It will return something like

```json
{ "_id" : ObjectId("5664e69d270b10887550707d"), "first_name": "Isabel", "last_name" : "THOMAS" }
```

`_id` is a special value, which serves as a key. Mongo **automatically creates** an `_id` value for inserted values in a collection. Dropping it in a projection **requires an explicit projection** to `0` for `_id`:

```javascript
var mycursor = db.users.find({"DOB" : 21, 
							  "MOB" : 11, 
							  "hometown.state" : "Rohan"}, 
							{first_name : 1, last_name : 1, _id : 0 }); 
```

We apply `count()` to count the number of results returned from `find()`.

```javascript
> mycursor.count()
4 
```

#### Aggregation 
- `$group`: similar to `GROUP BY`
- `$sort` : for **sorting** data
- `$unwind <arrayfield>`: **flattens** arrays
- `$out <out_collection>`: to put the result into a **output collection**

Aggregate also returns a **cursor**.

```ad-example
Aggregations Pipeline: `$match` and `$group`

![[Pasted image 20240216212543.png|600]]
```

