[[2023-10-28]] #DistributedSystem #OperatingSystem 

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
- Every computer on the Internet has an address ^a6362e
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
Computers are connected through the router. A router **forwards data** from one network to the next, and is usually a purpose-built Linux computer with multiple network connections. ^0addf7

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

Packets are great for the following reasons.
- Multiple programs on one computer can share one connection
- Multiple computers can share one connection

---
### TCP (Transmission Control Protocol)
IP is **NOT reliable** for the following causes.
1. Packets may arrive **out of order**
2. Packets may **disappear**
3. Packets may be **repeated**

TCP offers an abstraction to make it look like these problems don't exist.

To tend to problem (1), packets may arrive **out of order**, we do the following:
1. Sender assigns a **sequence number** to each packet
2. Receiver reassembles packets in order by sequence number
3. Receiver gives data to application (e.g. browser) in order

![[Pasted image 20231029192356.png|400]]

In the example above, even though `seq1` arrives ahead of `seq1`, the receiver is still able to assemble the message in order by the sequence number. 

The timeouts occurred between the receiver OS and the receiver application is due to the receiver's failure in sending out the entirety of the ordered data. Once the receiver finishes reassembling the packets, it then successfully sends it to the application.

To solve for problem (2), packets may **disappear**, we use the following strategies under TCP: ^90401c
1. Sender stores a copy of each packet
2. Sender sets a timer for each packet when it goes out
3. Receiver sends an **acknowledgement** (ACK) for each packet
4. Sender **resends** the packet if the **timer expires**
	- Delete the packet upon receiving ACK

![[Pasted image 20231029192828.png|400]]

If packet is delivered and ACK is received by the sender, it deletes the packet copy.

![[Pasted image 20231029192919.png|400]]

If the packet fails to deliver, the timer will result in a  timeout which asks the sender to resend the packet again.

Lastly, to deal with problem (3) that packets may be **repeated**, we ask the receiver to **ignore** sequence number that it **has seen before**. A packet may be repeated because
- ACK is dropped
- Packet is slow

Let's show an example of how a slow packet can screw things up. Imagine we have a timeout because a packet (`seq0`) fails to deliver (it's still en route because of how slow it is).

![[Pasted image 20231030203713.png|400]]

Then, the sender sends a copy of `seq0` because of the timeout as described [[#^90401c|above]]. This second packet may arrive even faster than the first one.

![[Pasted image 20231030203824.png|400]]

In this case, the slow packet will be seen as a duplicate and we should drop it by looking up its sequence number.

#### Flow Control
At times, we might have problems such that a **fast sender** overloads a **slow receiver**.
- E.g. sender is AWS and receiver is a smart watch

The solution to this is **flow control**, which is having the receiver tell the sender how many **empty buffer spots** it has.
- Sender can then put that much data on the network at one time
- Also called *sliding window*, or *receiver window (RWND)* 

![[Pasted image 20231030204811.png|400]]

#### Congestion Control
Another problem is when **many senders** overload a network router.
- Routers have buffers, but they drop packets when its buffer is **full**

![[Pasted image 20231030205101.png|400]]

In this case, the first packet from sender 2 fails to deliver as the router buffer is full, and the router will have to drop this packet.
- Causing a timeout from sender 2 - sending two packets at once must be too much!

The solution to this problem is **congestion control**. Here, we ask the sender to maintain a **congestion window** (CWND), which tracks the maximum packets awaiting acknowledgements (also **number of sent packages**). Traditionally, the sender
- Decrease congestion window when sender **loses** a packet
	- Assumes that a router dropped a packet because it was too busy
- Increase congestion window when sender **receives an ACK**
	- Packet got there safely, maybe there's room to send more

```ad-summary
**Flow control** (AKA sliding window)
- Keep a fast sender from overloading a slow receiver: RWND

**Congestion control**
- Keep a set of senders from overloading the network: CWND

We need both!
- TCP Window: $\min(\text{congestion window}, \text{receiver window})$
```

---
### UDP (User Datagram Protocol)
TCP's reliability mechanism can cause packets to be late.
- Late messages matter to applications like
	- Voice/video chats
	- Video games
	- Anything real-time
		- Packages will have to **wait** before any of the previously missing packages, as TCP doesn't allow for out-of-order networking
		- E.g. laggy videos

The solution to this is UDP (User Datagram Protocol) - letting the application decide what it wants to do.
- Better for real-time applications
- Doesn't handle out-of-order and dropped packets

---
### Sockets
The Socket API is an abstraction provided by OS functions that let applications use the network.
- Provides the TCP buffers that store packets
- Implements TCP sliding window

To allow for multiple programs **sharing the network**, we can create **multiple sockets**, each with its own **buffer** and **unique port number** (unique identifier for a socket on a host).

![[Pasted image 20231030214319.png|400]]

For example, we could have one server runs two programs that use the network.
- Nginx listens for incoming HTTP requests
- SSHD listens for incoming SSH connections

Both processes run on the **same host** using **different ports**.
- HTTP: port 80
	- HTTPS: port 445
- SSH: port 22

When a client sends a message to the host, it includes the **port number** of the operating system that it is looking to connect.