[[2023-11-15]] #InformationRetrieval #DistributedSystem 

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
1. Select the min hashed value from a document
	- As we compute hashes, we store the minimum
	- Hash functions should map inputs uniformly over the output
		- Hence selecting $\min(h(x))$ is the same as selecting a random item $x$
2. We compare $h_{\text{min}}(A)$ and $h_{\text{min}}(B)$ for $k$ times - document signature
	- If both sets share a random shingle - they might be **more similar**
	- The probability that two random hashes match is the Jaccard similarity
		- $P (h_{\text{min}}(A) == h_{\text{min}}(B)) = \text{Jaccard}(A,B)$ (good approximation)

```ad-summary
**MinHash Motivation**
- Computing hash values for a set is $O(N)$
	- Instead, we choose to only compute $k$ hashes, sometimes called the **signature of the set**
- Now, comparing two sets is now constant time

In summary, for each document, we first break it down into shingles. Then, we compute hashes of these shingles using $k$ different hash functions and selected the minimum hash of each of the $k$ groups - these hashes are known as the **document signature**.
- Or simply the $k$ smallest values

Then, when we compare two sets, the Jaccard probability using the number of matched hashes divided by $k$.
```

---
### Inverted Index Construction
After crawling is finished, we will have a big database of documents - hence the need to build an index (look-up table).

A **forward index** is a list of words in each document.
- `doc_id -> words: list`
An **inverted index** maps words to docs that contain those words
- What we want in a search engine!
- `word -> doc_ids: list`
- Old example: concordance - list of every word in alphabetical order constructed manually

In general, for each word, list all the documents where that word can be found.
- Key to fast query processing

In a search engine's perspective, an inverted index looks like this.

![[Pasted image 20231116153348.png|400]]

After knowing which documents contain which words, we can then **rank** these documents to produce the search result.

If we need to search for a list of docs for the search: `such as`, we would use the two pointer approach to iterate over the list of documents with `as` and `such`. Then, iteratively
- If `ptr1==ptr2`, advance both
- Else, advance the smaller `ptr`
- Abort when a list is exhausted

#### Construction
Inverted index is very large.
- **Magnetic disk** seeks are very **expensive** (5 ms)
- **Continuous disk** reads or writes are OK (50-120 MB/sec)

To build or index efficiently, the basic tasks are:
1. Compile `term`-`termid`, `doc`-`docid` maps
2. Assemble all `termid`-`docid` pairs
3. Sort pairs first by `termid`, then `docid`
4. Write out in inverted-index form

However, the doc set could be so large that it doesn't fit in the memory. For that we'll need an external sort algorithms work on sets larger than memory.

Below is a basic layout of the **block-sort-based index algorithm**.
```python
n = 0
while docsRemain:
	n++
	block = ParseNextBlock()
	BSBI-Invert(block)
	WriteToDisk(block, fn)
	MergeBlocks(f1, ..., fn) => fmerged
```

`ParseNextBlock` accumulates `termid` - `docid` pairs in memory **until block is full**, and `BSBI-Invert` generates small in-memory inverted index. In simple terms, we first build a series of small in-memory inverted indexes, writing each one to disk, then merge them.

![[Pasted image 20231116154546.png|400]]

---
### Distributed Search Architecture
Not even the inverted index is small enough for one machine to handle it.
- This calls for **parallel query processing** and **fault tolerance**
	- Segment by document
	- Segment by search terms

#### Segment by Documents
Let's begin with an illustration.

![[Pasted image 20231116154800.png|400]]

The client first submits a request to the search server, which in turn sends a copy of the same query to each of the search back end servers. Each back end servers has **different documents**, but every server **has every** **term**.

After the servers finish, they return the document ids that contain the word back to the search server, who later combines it.

#### Segment by Terms
Again let's show an illustration.

![[Pasted image 20231116155032.png|400]]

As opposed to segmenting by documents, every server now has **ALL the documents** for particular terms that it is assigned to. The search server knows who to send the query based on the term.

```ad-summary
**Segmentation Trade-off**

- Segment by document
	- Easy to partition (just MOD the docid)
	- Easy to add new documents
	- If machine fails, quality goes down but queries don’t die
- Segment by term
	- Harder to partition (terms uneven)
	- Trickier to add a new document (need to touch many machines)
	- If machine fails, search term might disappear, but not critical pages (e.g., cnn.com/index.html)
```

```ad-summary
**Summary, Information Retrieval**
Information retrieval is the heart of web search.
- **Document vector model** for page content
- **Network model** for link analysis
- **Metrics**: precision, recall, Kendall’s Tau, Mean Reciprocal Rank
- **Implementation**: crawler, deduplication, inverted index
```
