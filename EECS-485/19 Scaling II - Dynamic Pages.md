[[2023-11-16]] #Scaling #Webpage 

Recall our previously mentioned agenda [[18 Scaling I -  Static Pages#^8fbaf2|here]]. Today we will focus on scaling **dynamic pages**.

### Multi-process, multi-thread and asynchronous
Recall that in server-side dynamic pages, server **response** is the **output of a function**.
- What happen if many clients make requests at the same time?

Let's break this down into cases. First, imagine we have 1 server, 1 process and 2 requests.

![[Pasted image 20231120145637.png|400]]

As seen previously in distributed system sections, this made simultaneous requests go one at a time, causing **long wait time**.
- **Solution 1**: Asynchronous programming  
	- Limited solution: Illusion of parallelism, still runs on one CPU core
- **Solution 2**: **Multiple threads** on one CPU core
	- Limited solution: Illusion of parallelism, still runs on one CPU core
- **Solution 3**: **Multiple processes** on one CPU core 
	- Limited solution: Illusion of parallelism, still runs on one CPU core

We need a multicore CPU! Sometimes one multicore server is not enough, and at that point, the solution will simply be buying or renting more servers.

![[Pasted image 20231120150412.png|400]]

```ad-summary
**Multiprocessing, Multithreading, Asynchronous Programming**
- We can combine multiprocessing with multithreading
	- E.g. Apache with mod_wsgi
- We can combine multiprocessing with asynchronous programming
	- E.g. Gunicorn
- We can combine multiprocessing with multithreading *and* asynchronous programming
	- E.g. Gunicorn again
```

---
### Round Robin DNS, Load Balancing
After resolving the issue of handling two users making simultaneous requests, the problem is now **which server should respond first?**

#### Round Robin DNS
Modified DNS server responds with different IPs to different dynamic pages web servers by **rotating list of IPs**.

![[Pasted image 20231120150942.png|400]]

However, round robin DNS doesn’t consider **server load**. One request might be more time consuming than another. This leads us to **load balancer**.

#### Load Balancer
Also known as **proxy server**, which plays as the middleman that **forwards requests** to backend servers.
- End user doesn't need to know about them

Nevertheless, since a load balancer is a single source of failure, we need to implement fault tolerance - i.e. multiple load balancers.

![[Pasted image 20231120152306.png|400]]

Round robin DNS and load balancing can be acquired via [[18 Scaling I -  Static Pages#^39388c|PaaS]].

---
### Hardware Virtualization
To scale the servers we need to rent a lot of computers/servers from an [[18 Scaling I -  Static Pages#^b1ee91|IaaS]] provider. Nevertheless, this approach comes with a ton of problems.
1. Energy efficiency
	- IaaS provider has **many different computers**
	- Wasteful to run the same **small server-side dynamic pages** program on each server. Difficult to **customize** for each server.
2. Diverse environments
	- Programs have different environment
		- E.g. Linux kernel versions, installed libraries, etc.
	- How to customize the **OS and environment** for each program?
3. Security and isolation
	- IaaS providers has **many customers**
	- How to securely separate different customer's programs from each other?
4. Scaling
	- Variable workload
	- How to add more servers when load is high, and remove them when load is low?

This calls for **hardware virtualization**.

Without hardware virtualization, one physical computer runs one operating system. With hardware virtualization, one physical computer runs **multiple operating systems**.

```ad-tldr
**Hardware virtualization terminology**

```