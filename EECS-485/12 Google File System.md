[[2023-10-05]] #DistributedSystem

### Distributed System
Recall that [[11 MapReduce#^56108f|distributed system]] is about multiple computers cooperating on a task. In last section, we covered MapReduce, which is a distributed system for compute. Today, we will dive into Google File System (GFS), a distributed system for storage.
- Goal: Store more data than fits on one computer

We would often need storage that's too big for one computer in the following scenarios:
- Store server logs from thousands of servers over many days
- Store a copy of the entire web
	- Needed to build a search engine
- Archive social media data for billions of users

#### Traditional File System
It is not GFS.
- From the user's perspective (illusion)
	- Directories organized in a tree
	- Files are contiguous
- From the disk's perspective (physical storage)
	- Files and directories are organized as sequences of **non-contiguous** blocks

In traditional file system, disk is organized as an array of blocks.
- Analogous to memory organized as an array of bytes
- Actual file content

On-disk **inode** (index node) data structure **describes** each file or directory.
- Pointers to data blocks
- Timestamps, e.g., last modified

![[Pasted image 20231005223230.png|500]]

Again, note that a file or directory's data blocks will probably **NOT** be **contiguous** on disk.
- The data blocks can be anywhere on the disk (why? It allows computer to efficiently packs data onto the disk)

An obvious problem with traditional file system is that it organizes the data on **one** hard disk drive.
- Web data is too big to fit on one disk or even one computer with several disks

---
### GFS Design

```ad-tldr
Google File System
- Created in 2003
- Motivation
	- Need to store enormous amount of data, but don't want to lose data when a server fails

Solutions to the problem:
- Store multiple copies of everything
- Keep track of where those copies are

Comparison with Traditional System
- Divide file into ~MB chunks
- Place chunks on **different HDD** in **different computers**
```

In essence, GFS files are stored as a series of **chunks**, similar to blocks in a traditional file system. It achieves reliability through **replication**, as each chunk is replicated across 3+ **chunk servers**. Then, we have a single **main server** coordinates **access and metadata**.
- Chunk servers are computers that only store chunks
- We would only get the **right** file if we glued the chunks in **correct order**

![[Pasted image 20231005223844.png|500]]

A even cooler illustration:

![[Pasted image 20231005223910.png|500]] ^beed5c

Client asks for information about the chunks (not the files) through the main reverted index table. The main returns a list of chunks and chunk locations. The client makes request to the chunk servers and get corresponding chunk responses.

#### GFS Read
1. Client asks main for file name info.
2. Main responds with **chunk list**, and location(s) for each chunk
	- `chunk-33 / cserver 1,4`
	- `chunk-95 / cserver 0,2`
	- `chunk-65 / cserver 1.4.5`
	- Gluing these together gets us the file!
1. Client fetches each chunk, in sequence, from a c-server

#### GFS Write
A write would work just like a read if
- Clients take turns; and
- There are no errors

This is known as **serial**, **success writes**.

However, this is **NOT** always true. Let's again first go over the main steps.
1. Client asks main for file name info.
2. Main responds with **chunk list**, and location (s) for each chunk
3. Client sends modified chunks, in sequence, to c-servers

There are a few types of writes we want to discuss here.

##### One Write
All chunk servers get the **same value** for the **same chunk**.

![[Pasted image 20231005224814.png|500]]

##### Serial Writes
Assume we have **TWO** write tasks.
- In the first write, all chunk servers get the **same value** for the **same chunk**
- In the second write, All chunk servers get the **same NEW value** for the **same** chunk

![[Pasted image 20231005225107.png|500]]

##### Concurrent Writes
This time, assume that we have two writes from two clients happen at (almost) the same time.
- There is now a problem because now we have two **different values** on two different chunk servers for the **same chunk**: inconsistent!

![[Pasted image 20231005225341.png|500]]

In simple terms, consistent means that **every copy** has the **SAME value**. To guarantee consistency, writes need to be **coordinated**.
- In GFS context: Every chunk server has the **same value** for the **same chunk**

The coordination in GFS is called **write forwarding**. In this technique, we let one chunk server to be designated as **primary** for each chunk id. Then, the primary forwards writes to replica chunk servers.

Let's show using the illustration below. Since client always writes to chunk server 1 then forward to chunk server 2, we will not be having any inconsistency.

![[Pasted image 20231005230257.png|500]]

Another key property we need to look out for is **defined**, which requires that changes to **different chunks** of the **same file** from **different writers** did **NOT** get mixed together.
- Simultaneous writes to **different chunks** in the **same region** of one file

Why would this be a problem? Let's take a look at this GFS that is consistent yet **undefined**.

![[Pasted image 20231005230650.png|500]]

In this example our file `crawl.txt` is split into two chunks, `c100` and `c101`. Chunk server 1 is the primary for `c100` and chunk server 2 is the primary for `c101`.

Note that even though our chunks are consistent, `crawl.txt` is undefined because chunks written by different writers are mixed together.
- When we glue the chunks for `crawl.txt` using `c100` and `c101`, note that they would always come from **different writers**

To fix the problem, we have two solutions. The status by which we achieve is called **sequential consistency**, which means **concurrent writes** get the **same answer** as **sequential writes**, every time.
1. Main server coordinates writes
	- Easy to implement, but
	- **SLOW**
2. Distributed write coordination (GFS successor)
	- Really **HARD** to implement,
	- A little slow

Or, we could simply just not bother. This is known as **relaxed consistency**, by which we sacrifice some correctness for performance.

In fact, GFS made a deliberate design decision **NOT** to provide sequential consistency but provides relaxed consistency. In response to this, programmers would take advantage of
- Applications that detects and corrects undefined file regions
- **GFS Atomic Record Append**
	- Works for some applications: GFS and Google Apps

---
### Append
Through this operation, we want to achieve something like this:

![[Pasted image 20231006000510.png|400]]

Before we proceed, let's cover the concept of **atomic operation**.
- An atomic operation appears to the rest of the system as if it happened **instantaneously**, which avoids consistency problems

GFS uses something called the **atomic record append** for GFS append operation. 
- Record append only modifies **one chunk**
- Uses a similar strategy as **write forwarding**
	- One chunk server dedicated to one chunk id
	- Primary forwards appends to replica chunk servers

![[Pasted image 20231005232306.png|500]]

![[Pasted image 20231005232321.png|500]]

Note that we don't have any consistency problem in the concurrent record appends!

There are a few caveats, still.
- What if the new data is bigger than one chunk?
	- **NOT allowed**
- What if the new data would cross a chunk boundary?
	- Pad the last chunk to "fill it up"
	- Reply to the client to try again

GFS is optimized for fast, **atomic append**.
- Unique to GFS
- Google applications that use GFS are optimized to use append
	- E.g. web crawling

---
### Fault Tolerance
GFS needs to tolerate two kinds of fails.
- Failed chunk server
	- Chunk servers report to main server every few seconds.
		- Heartbeat message
	- If the main loses the heartbeat, marks the server as **down**
	- Main then asks chunk servers to **reduplicate** data on the lost server
	- Typically 3 copies of every chunk
- Failed main server
	- Main maintains critical data structures
		- A map: `filename -> chunk_id`
		- A map: `chunk id -> location`
	- Main writes a **log** to disk when data structures change
	- **Shadow main** consumes log and keeps **copies** of these data structures up-to-date
		- If main goes down, shadow main **read-only access** until main restart

```ad-summary
**GFS Advantages**
- Store lots of data
- Fault tolerant
- High throughput
	- Can read lots of data from many chunkservers at same time
- Good for throughput: batch applications
	- Web crawling
	- Web indexing
	- Apps when high throughput is important
- Optimized for atomic record append

**GFS Weaknesses**
- Bad for small files
	- Chunks are ~64MB
- Main node single point of failure
- High latency
	- Have to talk to two servers (at least main + chunk server) to fetch any data, might need multiple chunks
- Bad for real-time applications
	- Gmail
	- Google Docs
	- Apps when low latency is important
```
