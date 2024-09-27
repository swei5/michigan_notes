[[2023-11-20]] #Scaling #DistributedSystem #SQL #Database 

### Distributed Network Database
Recall that in [[19 Scaling II - Dynamic Pages]] we talked about how increasing the number of servers/computers is important in scaling server-side dynamic pages. 
- Different dynamic pages servers (front end servers) on every request
	- Servers on physical machines, virtual machines, or containers

However, there are problems that come with **multiple servers**. 
- **Problem**: Out of sync between dynamic pages servers
	- E.g. client updates part of the data
- **Solution**: Use **stateless servers** and store everything else in the database!

#### Network Database
Database software runs on a **different server** on the network. Many dynamic pages servers make network requests to this network database.
- E.g. PostgreSQL, MySQL

An easy implementation of this is **centralized database**, where we essentially just have **ONE** database server.

![[Pasted image 20231121002524.png|400]]

Here, data consistency is guaranteed; however, there is a performance bottleneck and hints at **more database servers**.

**Distributed network database** is simply many network database servers. Now, the consistency problem is back on - How to keep database servers in sync? This is hard and also depends on which servers contain which data.

Then how do we know which server contains which data? We will introduce two solutions, **sharding** and **database replication**.

### Sharding, Database Replication
#### Sharding by Content
Sharding by content means to have different rows or tables on different DB servers. There is only **ONE** copy for every piece of data, still.

![[Pasted image 20231121002922.png|400]]

It has a few strengths:
- **Scalable** because data is split among servers
- **Consistency** is easier because **NO copies**

However, the **throughput bottleneck** remains as a single copy could be a bottleneck for frequently accessed data.

#### Database Replication
Having multiple copies of the entire database.

A write can access any copy and **must be synchronized** to other copies, whereas a read can access any copy.

![[Pasted image 20231121003237.png|400]]

Database replication is advantageous in that it has a high throughput by having **multiple copies**. Nevertheless, it's 
- **Less scalable**: data might be too big to store on all servers
- With **consistency** issues: hard to keep copies in sync

```ad-example
What kind of websites might need
- More front-end servers?
	- Websites that **DON'T change** very often
		- E.g. Wikipedia pages, there are more people reading than editing
- More database servers?
	- Websites that have tons of DB activities

Which of sharding or replication would be better for website that stores the following data?
- Edits to Wikipedia articles: **Replication** as workload is big
- Tweets on Twitter by different users: **Sharding** by users
- Airline reservation system: **Sharding** because consistency is important
```

### CAP Theorem
CAP theorem describes distributed database tradeoffs, which includes three properties:
1. **Consistency**: Every read receives the most recent write or an error
2. **Availability**: Every request receives a (non-error) response, without the guarantee that it contains the most recent write
3. **Partition-tolerance**: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes

The CAP Theorem states only two out of the three can be true the same time.

There are really two broad scenarios here.

#### Network with No Failure
This is when you get consistency and availability. Here, database servers can synchronize, and there is no partition.

#### Network with Failures
When a network failure occurs, database servers **CANNOT** synchronize. This is a **partition**.

![[Pasted image 20231121003842.png|400]]

Here, we have a choice as to whether we want to return (1) an incorrect value - availability, or (2) an error - consistency.

Case (1): Available but not Consistent

![[Pasted image 20231121005015.png|400]]

Case (2): Consistent but not Available

![[Pasted image 20231121005035.png|400]]

```ad-example
Which of the applications require consistency/availability?
- Instagram feed: **Availability** as users want immediate contents
- Airline reservation system: **Consistency** because accuracy matters
- Amazon product search: A mixture of both
```

```ad-summary
**Prioritization of 3 Elements**
- Prioritize consistency over availability
	- Relational databases (AKA RDBMS AKA SQL)
	- PostgreSQL: synchronous replication
- Prioritize availability over consistency
	- "NoSQL" databases
	- MongoDB: primary which is consistent, secondaries that might have stale data
- Avoid Network failure
	- Buy a giant server for your DB so there's only one
	- Centralized network database
```

---
### Practical Database
SQLite, MySQL, and PostgreSQL. Everything can be summarized in this chart below.

![[Pasted image 20231121005317.png|500]]

---
### Distributed File System for Media Uploads
Media uploads include Instagram photos, TikTok videos, etc. 

**Problem**: Media uploads are **expensive** to store in a database. 
**Solution**: Store them to **disk**.
- Write once, read many times. Don't need a database to maintain consistency

However, recall that we have **multiple dynamic pages server** on every request if we want to scale our web app! 

**Problem**: In that case, how do dynamic pages server disks stay in sync?
**Solution**: Store media uploads on network storage.

#### Properties of media uploads
Media uploads have something in common with both static pages and dynamic pages.
- Commonality with static pages
	- Once they’re uploaded, they never change
- Commonality with dynamic pages
	- Content created by users
	- Access permissions per-user

There are two problems we are facing here still, however.

**Problem 1**: How to store the files?
- Many, large, up to exabytes of data
**Solution 1**: Network file system.
- Access files over the network much like local storage (hard drive)
- Can be accessed via PaaS (cloud file storage system)

```ad-summary
**Network file system, Pros and Cons**
- **Pros**
	- Acts like a local, traditional file system
		- Near zero client code changes
- **Cons**
	- Lower speed and scalability
	- Lack of fault tolerance
	- Network file systems weren't designed with the scale of the web in mind
```

**Solution 2**: Distributed file system.
- Access files over the network, often with a special API
	- E.g. [[12 Google File System|GFS]]
- Also available in PaaS

```ad-summary
**Distributed file system, Pros and Cons**
- **Pros**
	- Scalable and fault tolerant
- **Cons**
	- Uses a special API and requires client code changes
```

It's noteworthy that both systems are **distributed systems**. Now, back to our problems.

**Problem 2**: How do we upload and download?
**Solution 1**: Dynamic pages server is a middleman between client and distributed file system.
1. Client sends upload to to dynamic pages server
2. Dynamic pages server sends upload to distributed file system
	- E.g. Amazon S3

![[Pasted image 20231121010755.png|300]]

```ad-summary
**Media uploads proxy, Pros and Cons**
- **Pros**
	- Easy to implement
- **Cons**
	- Large file transfer goes through multiple servers
		- Higher latency
		- Higher bandwidth
```

**Solution 2**: Direct upload
- Directly upload from client to distributed file system
	1. Dynamic pages server **generates signed URL** for client
	2. Client uploads large file directly to distributed file system

```ad-info
We didn't consider [[18 Scaling I -  Static Pages#^b9baf2|CDN]] here even though it is fast because
- Media content changes a lot, CDNs best for content that changes infrequently
- Hard to prevent users from accessing media they aren’t supposed to
- Huge size of file could be expensive
```
