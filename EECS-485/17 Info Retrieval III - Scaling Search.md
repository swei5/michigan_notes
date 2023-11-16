[[2023-11-15]] #InformationRetrieval 

This is the third section in the sequence - how does web search work - especially at scale?

### Motivation
There are 5B - 100B web pages.
- Around 100 TB of data to index
- Each page has 1 minute to 1 month freshness

We need to come up with an efficient way to address this scaling issue.

---
### Crawler Design
To search the web, we need to build an index and link graphs.
- Download the **ENTIRE** web
	- Web robots, Spiders, `googlebot`

**Problem**: There is **NO** list of all the pages!
**Solution**: Do a **traversal** of the link graph.
- To begin with a **seed page**, follow all links like a BFS/DFS

**Problem**: What if there are $n$ disconnected components? 
**Solution**: Mercator

#### Mercator
An example of web crawler; it is exceptionally well-documented and useful to study, despite the many years.
- Start with seed URLs
- Has parallel HTTP engines that download webpages
- Queue is for traversal

![[Pasted image 20231115231021.png|400]]

