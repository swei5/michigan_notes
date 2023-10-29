[[2023-10-28]] #DistributedSystem 

### Motivation
Recall that distributed system is multiple computers cooperating on a task. Distributed systems are implemented through threads and processes for parallelization. 
![[13 OS, Parallelism#^45a21f]]

Today, we will be covering the topic of networking - how communication is conducted within a distributed system.

In MapReduce, we've briefly touched on how a manager communicates with a worker.
![[11 MapReduce#^d9ff86]]
- Here, the messages are sent in JSON blob over TCP/IP.

Similarly, in GFS, we've also learned how a heartbeat message is being sent to the Main server.
![[12 Google File System#^beed5c]]
- Here, the messages are sent in JSON blob over UDP

---
### IP (Internet Protocol)
