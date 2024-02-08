[[2024-02-03]] #Database #WebSecurity

In practice, DBMS is often part of a larger software infrastructure.
- Multi-tiered system architecture 
- Access database from another program

### Database Ecosystem
- **Client-Server Architecture**
	- Client sends SQL query to DBMS (server)
	- DBMS sends result back to clients 
- **3-Tier Architecture** (or more)
	- Client sends HTTP request to web server 
	- Web server sends SQL query to DBMS 
	- DBMS feeds result back to server, then to the client

Client is often a **program**, written in a language like Java, Python, or C++.

---
### SQL Integration 
A popular solution is to **embed SQL** in host language, or provide an **API** for processing query results.

#### Java-Database Connectivity (JDBC)
A Java application that located outside the DBMS that communicates with the DBMS.
- Connect to a database using a JDBC driver (`java.sql`)
- Send queries over the JDBC connection 
- Receive results into a Java ResultSet

We would need a `connection` object.  
- Oracle requires password 
- Sqlite3 is file-based and **DOES NOT** require password

An example of this looks like:
```java
Connection conn;
// Insert code here to connect conn to a DB.
// Requires JDBC driver; See sample code in Project 2 for
// Oracle or for Sqlite, Sample.java

String q = "SELECT name FROM Students WHERE GPA > 3.5";
try {
	Statement st = conn.createStatement();
	ResultSet rs = st.executeQuery(q);
    while (rs.next()) { // curson retrieves rows one at a time
		String name = rs.getString(”name");
        System.out.println(name);
	}
	rs.close();
    st.close();
}
catch(SQLException e){System.err.println(e.getMessage());}
```

**ALWAYS** remember to close connections before quitting the program by `conn.close()`. This saves user from overloading resources.

However, we could use an auto-close trick to auto close resources and connections automatically:

```java
...
// auto-close because of try-block
try (Statement st = conn.createStatement()) {
	ResultSet rs = st.executeQuery(q);
	while (rs.next()) {
		String name = rs.getString(”name");
        System.out.println(name);
	}
	// rs.close(); Not needed
    // st.close(); Not needed
}
// Still have to CLOSE conn
...
```

SQL data types differ from those of traditional programming languages. Thus, we need to match these types when retrieving results.

![[Pasted image 20240207160015.png|500]]

```ad-important
**Computation in DBMS versus computation in application**
- **Avoid fetching more** data than necessary
- “Push” data processing to the DBMS when possible
	- For instance, rather than computing the mean of some records of results in JDBC, let DBMS do the processing 
		- DBMS is optimized for computation 
		- Less networking traffic
		- Less conversion from SQL data types to Java objects

![[Pasted image 20240207164214.png|500]]
```

#### SQL Functions, Stored Procedure 
When we have more **complex logic** involved in the application that is difficult to include in one SQL query, we can take advantage of the following:
- SQL UDF (User-defined functions)
- Stored procedures

SQL functions include 
- "Regular functions": `LOG(<number>)`, `NEXTDAY(<date>)`, etc.
- Aggregate functions: `MAX()`, `AVG()`, `STDDEV()`, etc.
- User defined functions: `CREATE FUNCTION <name> AS`

Stored Procedures are more general than functions.
- Can use arbitrary DML, including `INSERT` / `UPDATE` / `DELETE`

#### Prepared Statements 
Are **parameterized** SQL query.
- DBMS “pre-compiles” the query (parses, optimizes, and stores query execution plan)
- Can be used **multiple times**
- Amortizes optimization cost across multiple uses

This is sometimes a more efficient way to run a regular SQL statement.

```java
PreparedStatement ps;
public List<String> getNames (int age) {  
	String q = "SELECT name FROM Students WHERE age = ?"; 
	if (!ps) ps = conn.prepareStatement(q); 
	ps.setDouble(1, age);  // Used in combo with ps - 1 being first arg
	ResultSet rs = ps.executeQuery();  
	...
}
```

This enables us to use a single, parametrized query **multiple times** that is stored and pre-compiled in the DBMS. 
- With regular statements the DBMS may have to run multiple queries for multiple times, which is inefficient

Prepared statements are also helpful in that they
- Does **type checking** on parameters and removal of escape characters
- Helpful for preventing security problems

---
### SQL Injection
SQL Injection is a common vulnerability in database applications.
- Simple but can be prevented with defensive coding

Recall we have covered this previously:
![[6 Web Security#SQL Injection]]

#### Prevention 
1. **Sanitize**
	- Check and sanitize any untrusted inputs
		- E.g. only certain letters, numbers and symbols (`#`, `_`, `$`) are accepted in a password
2. **Always use prepared statements**
3. **DB Access Control**
	- Limit JDBC app’s rights by assigning it **low-privilege account**
	- Most DBMSes allow **restricting read/write access** to tables or columns (and sometimes rows) based on user id
	- **Views**
4. **Encryption**
	- At-rest encryption: Prevents data loss if disk or computer gets stolen; But, data in plaintext at run-time
	- Encrypted databases such as CryptDB and Mylar: Data kept encrypted; **Queries also encrypted**