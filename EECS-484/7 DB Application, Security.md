[[2024-02-03]] #Database 

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
- Connect to a database using a JDBC driver (`java.sql`)
- Send queries over the JDBC connection 
- Receive results into a Java ResultSet
