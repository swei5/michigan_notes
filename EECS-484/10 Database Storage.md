[[2024-03-05]] #Database #System #DBMS

The second half of the class focuses more on **building a DBMS**, rather than using it (SQL). This includes
- Storage 
- Execution 
- Planning
- Concurrency Control 
- Recovery

```ad-todo
Roadmap of DBMS (Bottom to Top):
- **Disk Manager**
- Access Methods 
- Operator Execution 
- Query Planning 
- Transaction Manager 
- Log Manager
```

### Storage Architecture 
The DBMS assumes that the primary storage location of the database is on **non-volatile disk**. The DBMS's components manage the movement of data between non-volatile and volatile storage (memory).
- Capacity of disk is usually way larger than memory 
- However, we can only process data **in memory**

![[Pasted image 20240305132213.png|500]]

The volatile storage is typically **byte-addressable** - one can jump to a **random location** in a storage device and access a single byte with the same latency. Non-volatile storage only allows for **block accessing** - we first need to process the block and then would be able to access specific bytes.

For non-volatile devices, also known as **disks**, sequential access is faster than random access due to the construction of disk hardware. 

Dynamic random-access memory (DRAM) is equivalent to "memory", and CPU/CPU Caches are regarded as "CPU" in the context of this class.

```ad-important
DBMS will want to **maximize sequential access** and **minimize I/Os** to disks.
- Algorithms try to reduce number of writes to random pages so that data is stored in **contiguous blocks**
- Allocating multiple pages at the same time is called an **extent**
```

---
### Disk-Oriented DBMS 
Neither the memory nor the disk handle the data; the **execution engine** is responsible for data processing, while the DBMS is in charge of moving data in between disks and the memory.

![[Pasted image 20240305133756.png|500]]

The DBMS can use memory mapping (`mmap`) to store the contents of a file into the address space of a program. The OS is responsible for moving the pages of the file in and out of memory, so the DBMS doesn’t need to worry about it.

![[Pasted image 20240305134647.png|200]]

However, this could be problematic if the OS moved the wrong file into wrong places. 

A mitigation to this would be to allow multiple threads to access the `mmap` files to hide page fault stalls. However, this would only be good enough for read-only access - if there are multiple writers, concurrency becomes an issue.

DBMS (almost) always wants to **control things itself** and can do a better job than the OS.
- Flushing dirty pages to disk in the correct order
- Specialized prefetching
- Buffer replacement policy
- Thread/process scheduling

There are many solutions to using an OS to assist data storage in memory, but generally we would prefer DBMS.

---
### Tracking Pages: File Storage 
The DBMS stores a database as **one or more files** on disk typically in a **proprietary format**.
- The OS doesn't know anything about the contents of these files

The storage manager is responsible for maintaining a database's files. It organizes the files as a **collection of pages**. Some storage managers do their **own scheduling for reads and writes** to improve spatial and temporal locality of pages.
- Tracks data read/written to pages 
- Tracks the available space

A page is a fixed-size **block** of data.
- Contain tuples, meta-data, indexes, log records
- Most systems do not mix page types

Each page is given a **unique identifier**. The DBMS uses an indirection layer to **map page IDs to physical locations**.

There are three different notions of **pages** in a DBMS:
- Hardware Page (usually 4 KB)
	- The largest block of data that the storage device can guarantee failsafe writes: **NO data OR correct, safe data** (atomic write)
- OS Page (usually 4KB)
- Database Page (512 B-16 KB)
	- Trade-off between different pages
		- Larger file size: less number of movement between memory/disk for larger data
		- Smaller file size: better granularity 

#### Database Heap
Heap is the terminology used to describe how data files are **organized**. It is easy to find pages if there is **only a SINGLE** heap file.
- `OFFSET = Page# * Page_Size`
- SQLite works like this

However, with multiple heaps, we need to store meta-data to keep track of what pages exist in multiple files and which ones have free space (like a hash-map).

![[Pasted image 20240305181126.png|200]]

---
### Page Layout 
Every page contains a **header** of **meta-data** about the page's contents:
- Page Size
- Checksum
- DBMS Version
- Transaction Visibility
- Compression Information

Some systems require pages to be **self-contained** (e.g., Oracle).
- Any page should have **ALL necessary information** it needs to recover itself

In terms of data organization, there are two approaches:
1. Tuple-oriented
2. Log-structured

#### Tuple Storage 
The most common layout scheme is called **slotted pages**. The slot array maps "slots" to the tuples' **starting position** offsets. ^3deea0

The header keeps track of:
- The # of used slots
- The offset of the starting location of the last slot used 

![[Pasted image 20240305181837.png|300]]

This design fixes the potential issues of
- **Variable lengths** of tuples, and 
- **Deleted tuples**
	- When one tuple gets deleted, we have two choices: (1) leave it open and wait until next tuple that fits in the vacant spot, or (2) pivot the next tuple to its the deleted position
		- Since we have a slot array that maps to these tuples, the page is able to keep track of tuples at all time

The slot array grows downwards and the tuples grow in opposite direction; when they meet it indicates that the page is filled.

Each tuple is assigned a **unique record identifier**. The most common one is `page_id + offset/slot` or it can also contain **file location** information. These are called **record IDs**. This is similar to the **concept of header to a page**.

```ad-warning
An application **CANNOT rely on these IDs** to mean anything. It is relative to the DBMS.
```

---
### Storing Pages: Storage Models 
Generally, a DBMS faces three main workloads:
1. On-Line Transaction Processing (OLTP)
	- Fast operations that only **read/update a small amount** of data each time
	- E.g. Individual Amazon purchase
1. On-Line Analytical Processing (OLAP)
	- **Complex queries that read a lot of data** to compute aggregates of multiple entities
	- Often uses products generated by OLTP
	- E.g. Data analytics
1. Hybrid Transaction + Analytical Processing
	- OLTP + OLAP together on the same database instance

![[Pasted image 20240307153716.png|500]]

The [[3 Relational Model#^72e116|relational model]] does not specify that we have to store all of a tuple's attributes together in a single page. In fact, the DBMS can store tuples in different ways that are **better for either OLTP or OLAP** workloads.

#### $N$ -ary Storage Model (NSM)
The DBMS stores **all attributes for a single tuple** contiguously in a page. This is ideal for **OLTP** workloads where queries tend to **operate only on an individual entity and insert-heavy workloads**.

![[Pasted image 20240307154331.png|600]]

```ad-summary
**N-ary Storage Model, Pros and Cons**
- **Advantages**
	- Fast inserts, updates, and deletes 
	- Good for queries that need the **entire tuple** 
- **Disadvantages**
	- Not good for **scanning large portions** of the table and/or a subset of the attributes
```

#### Decomposition Storage Model (DSM)
The DBMS stores the **values of a single attribute for all tuples** contiguously in a page. This is also known as a "column store". This is ideal for **OLAP** workloads where **read-only queries perform large scans** over a subset of the table’s attributes.

![[Pasted image 20240307155939.png|600]]

Through this design, if a query only accesses a subset of the attributes in our database, DSM enables the DBMS to **only access NECESSARY pages** in the disk to prevent excessive memory usage and I/O.

##### Tuple Identification 
In DSM, there are two ways to identify a tuple. 
1. Fixed-length Offsets (Most common)
	- The same tuple is stored in the same offset across all pages
1. Embedded Tuple ID
	- Each value is stored with its tuple id in a column

![[Pasted image 20240309192509.png|500]]

```ad-summary
**DSM, Pros and Cons**
- **Advantages**
	- Reduces the amount of waster I/O because the DBMS only reads data it needs 
	- Better **query processing** and **data compression** as data of same type are stored together
- **Disadvantages**
	- Slow for point queries, inserts, updates, and deletes because of tuple splitting and stitching.
```

DSM is becoming increasingly popular in recent decades.