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

**Problem**: How to scrape password-protected content?
- Bank records, FaceBook, Dropbox, etc.
- Crawlers can't download it
**Solution**: **NO SOLUTION** - can't index these pages

#### Deep Web
- Surface web: content indexed by search engines
- Deep web: content **NOT** **indexed** by search engines
	- Including the **dark web** (see later sections)
	- Size of deep web likely several orders of magnitude **larger** than surface web

**Problem**: How to download client-side dynamic pages?
**Solution**: 
- Skip pages, or
- Have JS interpreter in crawler then use content from the DOM to download texts

```html
<html>
<body>
	<div id="reactEntry>Loading...</div>
	<script src="..."></script>
</body>
</html>
```

**Problem**: Crawlers generate **a lot** of traffic
- Makes many requests and CPU and bandwidth cost money
- Servers **block** IP addresses that are making fast bursts of requests (denial of service attack)
**Solution**: Limit number of requests per site per second

**Problem**: Not all sites want to be indexed
- Personal creations (Artwork on Google Image)
- Parts of dynamic websites 
	- E.g. search engine - if Google indices search results produced by Bing which might again includes results produced by Google...
**Solution**: `robots.txt`
- By convention located at the root of server directory
- Providing the search engine information to make results more useful
	- Search engine does have the option to **ignore** this

---
### Deduplication
Motivation here is that many web pages are duplicates and we need to find an efficient method to detect duplicate pages.
- Has the page changed meaningfully since last downloaded?
- Is it just a clone of another site?

A straightforward algorithm is **hashing**.
- When crawling a new doc, compute hash of all texts
- Look up in hash - $O(1)$ of work
	- If present, it's duplicate
	- Else, insert in hash table

However, this only works if pages are exactly **identical** (intuitively **order matters**), even though they are substantially similar.

#### Quantifying Similarity
Treat document as sets of words, then we can measure the similarity of a pair of docs $A$, $B$ as **Jaccard Similarity**
$$\text{sim}(A,B)= \frac{|A\cap B|}{|A\cup B|}$$
We take the size of the intersection divided by the size of union.
- $0$ for disjoint sets, $1$ for equal sets

However, If we compare two documents as sets of words using Jaccard similarity, then the **order** of words **doesn't matter** at all.

**Solution**: Shingles: sequences of $k$ words.
- E.g. if `text=` "this is some text", then
	- 2-shingles: {"this is", "is some", "some text"}
	- 3-shingles: {"this is some", "is some text"}
	- **Order matters**

To combine shingles with Jaccard similarity, we need the following.
1. Compute 3-shingles for doc $A$ and doc $B$
2. Compute Jaccard similarity between $A$ and $B$
3. If Jaccard exceeds *threshold*, then they're duplicates

Because Jaccard similarity is a property of pairs, we need to compute it across **all pairs**. This is $\frac{n (n-1)}{2} \implies O(n^2)$ .
- $n$ being the number of docs on the internet
	- In the section below, we are only going to reduce the complexity of computing Jaccard similarity (comparison between two documents), but **NOT** with comparison of all docs on the internet

To improve efficiency, we can **pre-compute** some information about every page which makes computing Jaccard similarity faster.

##### Hash Shingles
We can compute hash of each shingle in docs $A$ and $B$.
- Before it was $O(n)$ for string comparison for length $n$
- Now numeric comparison is $O(1)$

##### Random Shingles
However, we still must compute the set union and intersection for $n$ shingles.

**Solution: MinHash**
We can select the element at a random index; however, not all containers support random access. 

Thus, we would turn to a different approach.
2. Select the min hashed value from a document
	- As we compute hashes, we store the minimum
	- Hash functions should map inputs uniformly over the output
		- Hence selecting $\min(h(x))$ is the same as selecting a random item $x$
3. We compare $h_{\text{min}}(A)$ and $h_{\text{min}}(B)$ for $k$ times - document signature
	- If both sets share a random shingle - they might be **more similar**
	- The probability that two random hashes match is the Jaccard similarity
		- $P (h_{\text{min}}(A) == h_{\text{min}}(B)) = \text{Jaccard}(A,B)$ (good approximation)

```ad-summary
**MinHash Motivation**
- Computing hash values for a set is $O(N)$
	- Instead, we choose to only compute $k$ hashes, sometimes called the **signature of the set**
- Now, comparing two sets is now constant time

In summary, for each document, we first break it down into shingles. Then, we compute $k$ hashes of these shingles using $k$ different hash functions - these hashes are known as the **document signature**.

Then, when we compare the Jaccard probability using the number of matched hashes divided by $k$.
```
