[[2023-12-04]] #WebApplication 

### Introduction
The **dark web** is the part of the Web accessible only through an **anonymous connection**.
- Tl;dr: Tor

The **deep web** is pages that search engines can't find.
- Behind paywalls: WSJ
- Behind passwords: Gmail, Facebook

The deep web includes the dark web.

Recall that information are communicated using the [[14 Networking#^84949a|internet protocol]].
- Every computer has an address

Information go through different **routers** to arrive at the final destination, known as **routing**. ![[14 Networking#^0f6fce]]

Or, to put it a different way, your packet traverses **many routers** controlled by **third parties**.
- Eavesdropping by attack vector: capture a router and listen in

Assume a powerful adversary as such, there are a few desirable properties we want over a communication. Some other brought up previously: ![[5 Encryption#^ff793a]]
In addition, we would also like the following two:
- Freshness
	- Message was sent “recently”
		- Solved with symmetric encryption and timestamp, sequence number or nonce
- **Anonymity**
	- Attacker should not know that we are communicating
		- Solved with VPN (weak) or Tor (strong)

---
### VPN Proxies
Each packet has source IP address & destination IP address.
- Can hardly change source (spoofing)
- If you change destination, routers won’t deliver it to the right place

Packet **source** and **destination** are visible to anyone observing the network. The packet contains some metadata which includes
- ISP
- Destination web site
- Any intermediate router
- Network eavesdropper

These are public information so it's easy to know two parties are communicating.

#### Proxy Server
Proxy server is a middleman that makes your internet traffic appear to come from somewhere else.
- In essence, hiding your IP address

![[Pasted image 20231204215335.png|400]]

A VPN proxy server, even better, **encrypts your traffic** in addition to hiding your IP address.

![[Pasted image 20231204215416.png|400]]

A eavesdropper can see client communicate with VPN proxy, and VPN communicate with Web site.

```ad-summary
**VPN proxy server vulnerabilities**
- Single point of failure: VPN knows all
- Vulnerable to **subpoenas**
	- Court asks proxy server admin for server logs
- Some VPNs have a "**destroy logs**" policy, however this is a trusting issue
- Vulnerable to **misconfiguration**
	- Out-of-date software with security vulnerabilities leads to exfiltrating data
	- Doesn't support IPv6
- Vulnerable to **traffic analysis**
	- ISP or other eavesdroppers can use traffic analysis even with a VPN and SSL
```

---
### Tor
Stands for **The Onion Router**. It is used to fight traffic analysis. The key idea is to add **layers of indirection** using Tor nodes to relay traffics from clients to services.
- Run by volunteers

```ad-example
**How Tor works**
![[Pasted image 20231204220508.png|400]]
![[Pasted image 20231204220519.png|400]]
![[Pasted image 20231204220531.png|400]]
```

Tor encrypts outgoing data **multiple** times, like layers of an onion. There is one **encryption layer** for each step in the relay.

Each layer of encryption is peeled off by a **relay node** in the network. At each layer:
- Decrypt the current layer
- Forward to the next destination

```ad-summary
**Tor vulnerabilities**
- Only the first relay node knows the source IP address
- Only the last relay node knows the destination IP address
- Statistical correlation attack
	- With enough log data from ISP and final destination
		- **More Tor users** conducting more activity on Tor **reduces vulnerability**
```

---
### Tor Services, Browser, History
Tor was originally developed by US Naval Research Laboratory, aiming at protecting US intelligence communications.
- Now a nonprofit

online

Tor allows users to anonymously publish services.
- E.g. web pages, or a chat server, hidden service search engines, Tor Mail

However, service locations (rendezvous points) must be **known** to clients.

**Problem**: Using Tor services are **NOT enough** to keep anonymity, however. **Session cookies**, including third party trackers are ubiquitous. In other words, your browser reveal you.

**Solution**: Add-ons like uBlock Origin, Privacy Badger, Brave, Disconnect or ScriptNo block tracking ads. They monitor embedded links and blacklist trackers.

Even-better **Solution**: **Tor browser** (modified version of Firefox)
- Routes all traffic via Tor
- Essential privacy plugins
	- NoScript and HTTPS Everywhere
- Non-tracking search engine
	- DuckDuckGo instead of Google, Bing and friends

Tor browser also **avoids** **browser fingerprinting**.
- User-Agent string  
- Screen size  
- Browser plugin details 
- Language  
- Platform

The middle ground between fully anonymous browsing in Tor and naked browsing in Google Chrome is Firefox and DuckDuckGo.

```ad-summary
**Uses of Tor**
- Break censorship barriers  
	- E.g. in places that censor web content
- Political activism  
	- E.g. in places that discourage free speech
- Net neutrality  
	- E.g. ISP speeds up some traffic, slows down other traffic
- Reporting abuse or corruption
	- E.g. whistleblower concerned for safety, Tor enables anonymous communication
```
