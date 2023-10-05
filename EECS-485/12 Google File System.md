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
They are not GFS.
- From the user's perspective
	- Directories organized in a tree
	- Files are contiguous
- From the disk's perspective
	- Files and directories are organized as sequences of non-contiguous blocks
- 