[[2023-11-16]] #Webpage #Scaling 

### Cloud
What is cloud? Linux servers mostly.

**Motivation**: how to scale the a web app to many, millions of users? 
- We need a production environment
- What we've done in the past is solely on one computer in a development environment

**Development environment** runs on one computer, sometimes just one program
- Example: `flask run`
**Production environment**, however, is many programs running on many computers for end users.
- Specialized and scalable

#### On premise Servers
**Problem**: Where do these computers go?
**Solution**: Buy servers - this is also known as private cloud, or **On premise servers**
- Buy your own servers
- Build [[11 MapReduce#^cd42b1|data centers]]

However, there's a lot more to just building a data center.

**Problem**: On-premises physical servers are a pain
- Power and AC
- Backup and failover
- Hire engineers and technicians
**Solution**: Rent computers - **IaaS**

#### IaaS
**Infrastructure as a Service** (IaaS) is **renting a computer** in somebody else's datacenter.
- Usually connected via SSH (no monitor, mouse, keyboard)
- Might be a virtual machine (more on that next time)
- E.g. Amazon EC2, Google Compute Engine, Microsoft Azure Virtual Machines
	- Rented computers

Benefits of IaaS include
- Physical datacenter handled by someone else  
- Backups and failover handled by someone else (a lot of times)
- Instantly and automatically rent more computers

**Problem**: Managing software on many rented computers is a pain
- Security updates, complex configuration
**Solution**: Rent computers with software already installed and configured

#### PaaS

^39388c

**Platform as a Service** (PaaS) is renting a computer in somebody else's data center.
- Just like IaaS, but PaaS gets **software management and configuration**
- E.g. AWS Relational Database Service (RDS), Google Cloud SQL, Microsoft Azure SQL
	- These softwares come with the platform

**Problem**: Don't want to write an app, don't want to run an app on rented servers. Just want to use app.
**Solution**: Rent a web app running on somebody else's computers.

#### SaaS
**Software as a Service** (SaaS) is renting a web app built, hosted, and maintained by somebody else.
- Zero effort
- E.g. Github, Dropbox, Gmail

```ad-summary
A cool graph to sum it up.

![[Pasted image 20231116213542.png|500]]
```

#### Web Apps in the Cloud
We will scale one thing at at time, and divide apps into different things they need to be scaled. ^8fbaf2
- Static pages ^7422ab
- Server-side dynamic pages 
- Client-side dynamic pages 
- Database storage  
- Media uploads storage

We will dive into scaling these elements in the coming three sections of the course.

---
### Intro to Scaling Static Pages
Static pages is really about a request **returns a copy of a file**.
- E.g.  `logo.png`, `style.css`, `bundle.js`

To serve many copies to many clients at the same time around the globe, we will need **many servers** in different geographic locations.

Recall how information travels using IP. ![[14 Networking#^a6362e]]
In order to direct clients to the IP address of a nearby server, we would need DNS.

---
### Domain Name System (DNS)
The Domain Name System (DNS) translates **domain names to IP** addresses.
- Basically a database of (hostname, IP) pairs
- E.g. To get to `umich.edu` website, there are essentially two steps
	1. Client goes to a nameserver, which returns the IP address of `umich.edu` through DNS
	2. Through the IP address, client connects to the web server

Few things are required for DNS.
- Needs to be up all the time  
- Continuously updated by many parties  
- Must be accurate; errors prevent connections
- Serves massive query load  
- Distributed administration

There are four aspects of DNS.

#### Namespace
Domain names must be unique, need a **namespace**.
- Tree structure, 1-63 chars per node
	- Only DNS root has no parent
- Fully-qualified domain name is **leaf-to-root** name

![[Pasted image 20231116215249.png|400]]

Each node is usually a server running **DNS server software**. Nodes grouped into **administrative zones**.
- E.g. root, com, google.com
Each zone served by **authority servers**.
- AKA authoritative nameservers
- Authority server can **delegate subdomains** to other authority servers.

Servers are primary or secondary.
- Primary authority servers are given content by admins
- Secondary authority servers grab data from primaries
	- Duplicative information

A domain registrar inserts your name into the primary authority server for .com (or .net, .org, etc).

Purchased domain names inserted into 1 primary, 1 secondary (in case of primary failure).

To check the domain name, we can use `whois` in CLI.
- Or `nslookup` - this is essentially what our browser does every time it tries to connect to a server

#### Resolution
Content of DNS consists of resource records. In a nutshell, client makes requests to authority servers, and the server responds with **resource record**.
- Host, IP pairs
- We can use `host` in command line to achieve this

DNS clients are built into network libraries, and clients use UDP to grab data.

So how do we get a DNS server? First, we know that IP addresses are commonly assigned automatically when we connect to the internet.
- Through the **DHCP**: Dynamic Host Configuration Protocol, we are assigned an IP, which later provides us DNS servers

For example, if we want to look up the IP address of `umich.edu`, there are really five steps under the hood.
1. Local nameserver provides IP of root DNS server
	- Local nameserver is provided by the network provider
1. Root DNS server provides IP of .edu DNS server
2. Edu DNS server provides IP of umich. Edu DNS server
3. Umich. Edu DNS server provides IP of umich. Edu

![[Pasted image 20231116221802.png|500]]

##### Resolution with Caching
There may be a chain of caching DNS servers between client and authority server.
- Caches DNS requests; answers new requests from cache whenever possible
- Each record may have a time-to-live (TTL) in seconds
	- **Counts down** from moment authority server emits resource record (RR)
	- Caches and clients must throw out resource records with expired time-to-live

Responsible for locating the **top level domain** name servers (.com, .net, .org, ...).
- 13 root name servers in world
	- Their locations are hard-coded in resolving DNS servers
	- Due to caching, it only involved in few queries

There are SaaS companies that provide SaaS DNS registration and servers.

#### Administration
**IANA** (Internet Assigned Numbers Authority) oversees global IP allocation and DNS root zones.
- Previously run by Jon Postel
	- Implementations must be **conservative** in sending behavior, and **liberal** in its receiving behavior

Today, IANA is part of the non-profit ICANN, which is under contract with US government.
- For profit registrars
- There is much US control of top level DNS, as DNS server returns the IP address of a web server that practically allows you to surf 

#### Scaling and Security
DNS operates at shocking scale and is super important.
- Almost every communication starts with a domain name
- Affects PC, phone, internet of things (IoT) services
- E.g. spam filtering requires 10+ DNS lookups per message!

DNS is an extreme security vulnerability because the ability to remap a host to an IP, or in turn prevent a user to reach a website, is potentially a terrorism target.

##### DNS Cache Poisoning
- Bad info can be inserted into a DNS server and cached
	- Both inadvertent and malicious
	- China firewall - redirect requests to the wrong IP

There are some remedies. If you use HTTPS/SSL/TSL, the problem is mitigated.
- Wrong server would send wrong certificate - then the computer realizes it's the wrong website
- Or use cryptographically signed DNS entries

---
### Content Delivery Network (CDN)
DNS turns a hostname into the IP address of a server. But how do we return the IP address of a **nearby** server?

```ad-important
A content delivery network (CDN) **stores static files** at many locations throughout the world and dynamically **serves nearby clients**.
- Static files: HTML, images, videos, JS source codes
- Large read-only data stored close to client
```

```ad-example
If we type `curl -skL eecs485.org | grep cdn` in CLI, we will see how eecs485.org uses CDN to store its JS and CSS codes.
```

It reduces **latency and load** to faraway datacenter and **bandwidth costs** by sending data only a short distance.

**Akamai** is the first CDN; it is built on top of a giant DNS hack. 
- We might think the image is from CNN, but Akamai is serving it because CNN pays it
- 170k servers
- Serves 15%-30% of all web traffic

So, how does it work? 
1. Akamai places servers across country.
2. Akamai gets client data, especially images and videos. Then it copies to all nodes in network
3. Website rewrites their URLs to uses CDN URLs
	- `http://cnn.com/...` to `http://akadns.akamai.net/....`
4. Then, specialized Akamai DNS server **resolves name to nearby server** (tiny TTL)
	- The decision made by the DNS server depends on location of client; network conditions; load on Akamai servers; traffic estimation errors, etc.

If CDN goes down, website goes down.

CDN is available via PaaS.
- Akamai
- AWS Cloud Front
- Cloudfare