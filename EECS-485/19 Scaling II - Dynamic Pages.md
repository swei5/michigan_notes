[[2023-11-16]] #WebScaling

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
Modified DNS server responds with **different IPs** to **different clients** with different dynamic pages web servers by **rotating list of IPs**.
- The list of IPs are usually in the **same datacenter**

![[Pasted image 20231120150942.png|400]]

However, round robin DNS doesn’t consider **server load**. One request might be more time consuming than another. This leads us to **load balancer**.

#### Load Balancer
Also known as **proxy server**, which plays as the middleman that **forwards requests** to backend servers.
- End user doesn't need to know about them
- Server reports to load balancer about its status

Nevertheless, since a load balancer is a single source of failure, we need to implement fault tolerance - i.e. multiple load balancers.

![[Pasted image 20231120152306.png|400]]

Round robin DNS and load balancing can be acquired via [[18 Scaling I -  Static Pages#^39388c|PaaS]].
- Very unlikely one has to write it on their own

---
### Hardware Virtualization
As previously introduced, to scale the servers we need to rent a lot of computers/servers from an [[18 Scaling I -  Static Pages#^b1ee91|IaaS]] provider. Nevertheless, this approach comes with a ton of problems.
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

![[Pasted image 20231120220332.png]]

Without hardware virtualization, one physical computer runs one operating system. With hardware virtualization, one physical computer runs **multiple operating systems**.

Virtualization is an **abstraction** for hardware.

```ad-tldr
**Hardware virtualization terminology**
- **Host**: **Physical computer** running OS and virtualization software
- **Guest**: Operating system(s) being run as “programs”, not on dedicated physical hardware
- **Hypervisor**: Virtualization software runs guests on host
	- VirtualBox, VMWare, et al.
- **Hardware emulation**: Host shares physical resources like network, disk, etc. with guests via virtual hardware
```

Examples of hardware virtualization include WSL2 (Windows Subsystem for Linux 2). Apparently you can also circumvent remote proctoring software using a virtual machine (unless there is VM detection).

```ad-summary
Hardware virtualization has these advantages:
- **Energy efficiency**  
	- Run every physical machine at maximum efficiency
- **Diverse environments**  
	- Each service gets its own preferred OS, libraries, etc.
- **Security and isolation**  
	- Compromised guest OS won’t affect other guest VMs
- **Scaling**  
	- Add more VMs as more users hit your site
	- "Add another computer" is now "start a VM"
- **Replication for correctness**  
	- Development VM has exact same guest OS, SW, libraries, packages as production VM  
		- Catch bugs in development (VM), not production
- **Replication for disaster recovery**
	- Easy to relocate VM

On the flip side, it has these disadvantages:
- **Memory**  
	- Guest includes a complete OS including drivers, binaries, and libraries
- **Slow start**  
	- 10s ~ 100s to boot a VM
```

Again, hardware virtualization is offered through IaaS.

---
### Containerization
Virtualization isn't all pretty. One key issue is that it could have high overhead and a slow start. The solution to this is **containerization** - sharing OS with its binaries and libraries.

Container includes application code and dependencies, but does **NOT** include the OS. It is an abstraction of the OS.

![[Pasted image 20231120222826.png]]

To use a container we need to install **container runtime**. An example of this is **Docker**. Then, we would create docker image by copying our application `app` to container and configure it to execute server by `docker build`, and finally run it by `docker run app`.

To integrate this with our previous round robin DNS and load balancing implementation:

![[Pasted image 20231120223246.png|500]]

```ad-summary
Containerization has these advantages:
- Lower memory footprint
- Faster start-up time at 0.1 ~ 1 s

On the flip side, containerization can be disadvantagenous because
- All containers that run on a Linux host must be designed to run on Linux
- A security vulnerability in the OS will impact every container on that host machine
```

```ad-tldr
**Comparison between Virtualization and Containerization**

![[Pasted image 20231120223910.png|500]]

We can combine virtualization with containerization.
```

#### Stateful VS. Stateless
In a stateful virtual machine, VMs have a virtual, persistent hard drive.
- Hard drive is a file on the **host computer**
- VM writes to the HDD $\implies$ file modified

Hence, when we restart the VM, our file is restored, including changes.

![[Pasted image 20231120224150.png|300]]

For stateless containers, container software layer **simulates** hard disk drive.
- Hard drive is a **data structure** in the container runtime
- Container writes to the HDD $\implies$ data structure modified
- Refreshes on restart

However, when we restart the container, changes are **LOST**.
- This is a feature! It is a **separation of storage and compute**

![[Pasted image 20231120224308.png|400]]

#### Service-oriented architecture (SOA)
Breaking up a web app into **services** that **communicate over a network**.
- Runs different services on different machines
	- Static pages servers
	- Server-side dynamic pages servers
	- Database servers
	- Log storage servers
	- Etc.

**Microservice architecture** is a service-oriented architecture with "smaller pieces" and is common with containerization.
- Many instances of a web app
- Web apps communicate with servers over the network

![[Pasted image 20231120224527.png|400]]

We can acquire container execution via PaaS - renting servers that run container runtime such as Docker. This is sometimes called serverless computing.

### Scaling Client-side Dynamic Pages
This is simply scaling JavaScript with same technique as **static pages** and REST API as server-side dynamic pages.
- `bundle.js` via CDN
- REST API server code in a container on PaaS