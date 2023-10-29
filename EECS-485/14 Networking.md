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
How messages are passed from one computer to another computer. Examples include
- MapReduce Manager server assigns task to Worker
- Google File System chunkserver tells Main server "I'm alive"
- GET request from a client web browser to an HTTP server

#### IP Address
Since there are numerous computers connected to the internet, to tell them apart we would require some form of **addresses**.
- Every computer on the Internet has an address
	- IPv4: 32-bit allows for 4 billion computers
	- IPv6: 64-bit, way more

Your **internet service provider** gives you an IP address when you connect to the wifi (or wired) network.

At the command line, we can input `curl ipinfo.io/ip` to get our IP address.
- If you're on a home network, this is likely the IP address of your router

```ad-question
How can we ever know Amazon's IP address?

This has to do with Domain Name Service, covered later this term.
```

#### Routers
Computers are connected through the router. A router **forwards data** from one network to the next, and is usually a purpose-built Linux computer with multiple network connections.

![[Pasted image 20231029190803.png|500]]

Each router has a **routing table**, which maps groups of IP address to their destination. The destination could be another router.
- This is done through **longest prefix matching**

For example, to see how our computer is being routed to Amazon's AWS, we can input the following in command line:

```bash
$ traceroute aws.amazon.com
```

From which we would get all the steps, also known as a hop, of the routing process.

#### Internet Geolocation
Internet geolocation tells us where a device is located. It uses a database that **maps IP address to approximate location**.

We could use this utility in CLI by `curl 'https://api.ipgeolocation.io/ipgeo?apiKey=API_KEY'`. Read more [here](https://app.ipgeolocation.io/auth/login).

#### Packets
A message is broken down into **packets** by the sender.
- Usually ~1,000 to 1,500 B in size

We use packets for the following reasons.
- 