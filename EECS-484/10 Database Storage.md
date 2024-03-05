[[2024-03-05]] #Database

The second half of the class focuses more on building a DBMS, rather than using it (SQL). This includes
- Storage 
- Execution 
- Planning
- Concurrency Control 
- Recovery

### Storage Architecture 
The DBMS assumes that the primary storage location of the database is on **non-volatile disk**. The DBMS's components manage the movement of data between non-volatile and volatile storage (memory).
- Capacity of disk is usually way larger than memory 
- However, we can only process data **in memory**

![[Pasted image 20240305132213.png|500]]

The volatile storage is typically **byte-addressable** - one can jump to a **random location** in a storage device and access a single byte with the same latency. Non-volatile storage only allows for **block accessing** - we first need to process the block and then would be able to access specific bytes.

For non-volatile devices, also known as **disks**, sequential access is faster than random access due to the construction of disk hardware. 

Dynamic random-access memory (DRAM) is equivalent to "memory", and CPU/CPU Caches are regarded as "CPU" in the context of this class.

DBMS will want to **maximize sequential access** and **minimize I/Os** to disks.
- Algorithms try to reduce number of writes to random pages so that data is stored in **contiguous blocks**
- Allocating multiple pages at the same time is called an **extent**

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
### File Storage 
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