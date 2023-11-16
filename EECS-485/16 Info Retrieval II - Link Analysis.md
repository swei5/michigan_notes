[[2023-11-15]] #InformationRetrieval 

### Document Importance
Yet another way to improve search results, other than ranking on document content, as introduced in [[15 Info Retrieval I - Text Analysis]].

A good way to visualize relations between document is through graphs. In this sense, web can be interpreted as a graph (AKA a network).
- Each web page is a vertex
- Each hyperlink is a **directed edge**

![[Pasted image 20231115211723.png|400]]

It's not hard to observe the following general facts:
1. If lots of pages link to a page, it is important, and in turn
2. If an important page links to a page, that page also is important

Since the 1990s, web search has been improved thanks to hyperlink graphs. We will be looking at these two algorithms.
- PageRank
- HITS

Essentially, to reflect page importance in search results, our goal is to
1. Make a graph of the web
2. Figure out which sites are more **important**
3. Rank those sites higher in search results

---
### PageRank
The algorithm that makes Google famous. The intuition behind PageRank is simple:
- Humans know better than computers which pages are important
- Humans indicate **importance through links**
	- Like citations in papers
	- Assume someone starts on a random webpage, clicks on a random link on that page and repeats that process over and over - eventually webpages with **more visits** are **more important**

#### Algorithm
In graph theory, this translates to that a node with $C$ links contributes $\frac{1}{C}$ of its PageRank to each **target node**.
$$P(A)=\frac{1-d}{N}+d\sum\limits_{i} \frac{P(I_{i})}{C(I_{i})}$$
Where $N$ is the number of nodes in graph, $d$ is the damping factor and is usually $0.85$, and $C(I_i)$ is the number of nodes that node $i$ directs to (inverse of contribution).

```ad-example
![[Pasted image 20231115213759.png|500]]

In this example, since vertices $B,C,D$ are only leading to $A$, they are each making a contribution of $1$ to $A$.
```

This is an **iterative process**. We would first set $P$ to $\frac{1}{N}$ for all nodes and use that for inputs for the first iteration, then use the outputs from PageRank to compute the iterations after that. The algorithm stops until convergence.

![[Pasted image 20231115214313.png|400]]

Note that it's likely that we have to normalize the probabilities before going into the next iteration.

```ad-note
Changing the order of nodes in an iteration doesn't matter; only the structure of the graph does.
```

#### Sink Nodes and Regions
In the first example above, it's clear that after many iterations, PageRank for node $A$ will become increasingly big and the PageRank for other nodes will diminish (drain rank from the system). To combat that, we require
- Nodes with no outlinks are **disallowed**
- **SOLUTION**: Adding an edge from the sink to every other node

![[Pasted image 20231115215908.png|400]]

We can also encounter sink regions, which are a group of nodes that do not have any outlinks to any nodes outside of that group. To solve this, we
- Require a **non-zero probability** of reaching every node from every other node
- **SOLUTION**: Damping factor!
	- The first element of the PageRank formula gives a non-zero probability of a surfer randomly **typing** in a random URL than clicking a link

```ad-tldr
PageRank is a combination of random clicking and random typing into the browser's URL bar.
```

#### Adding PageRank to Search Engine
Incorporate what we learned in [[15 Info Retrieval I - Text Analysis#^c16cfc|document similarity]] from last lecture and what we learned today about **page importance**, we can come up with a even more comprehensive rank for a doc.
$$P(q,p)=w \text{ sim}(q,p)+(1-w) P(p)$$
Where $w$ is weight determined experimentally by the designer and is in $[0,1]$ and $\text{sim}(q,p)$ and $P(p)$ needs to be normalized.
- To get the best weight, train it on a set of web pages

---
### HITS
Unlike PageRank, HITS is **query** **dependent**. The intuition here is that
- A page is a good authority if it is pointed-to by many good hubs
- A page is a good hub if it points to many good authorities
	- Good hubs and authorities **reinforce** each other

$$\text{auth}(p)=\sum\limits_{i}\text{hub}(i)$$
$$\text{hub}(p)=\sum\limits_{i}\text{auth}(i)$$

#### Algorithm
1. Obtain **root set** using input query
	- Implying we are **NOT** looking at a graph of the entire web, but a subset
1. Expand the root set by radius $1$. This is called the base set
2. Iteratively compute hub and authority scores for each node in graph
	1. For all nodes, update authority scores
	2. For all nodes, update hub scores
	3. Normalize scores
		- Divide each auth by its sum of squares
		- Divide each hub by its sum of squares
	4. If converges, end; else goto 1

```ad-example
Let's use an example to illustrate.

First, we build our root set - which are documents that have both of our query terms.
![[Pasted image 20231115221928.png|400]]

Using these two nodes, we build our base set by including nodes that are a distance of $1$ from our base nodes. 

![[Pasted image 20231115222039.png|400]]

From now on we will only focus on this subgraph. Then, we initialize all hub and authority scores to $1$.

![[Pasted image 20231115222126.png|400]]

Then, we update authority and hub scores.

![[Pasted image 20231115222255.png|400]]

![[Pasted image 20231115222313.png|400]]

Then we continue to do so until the graph converges...
- Notice in this example we intentionally skipped the normalization step

![[Pasted image 20231115222409.png|400]]
```

```ad-summary
**HITS vs. PageRank**
- HITS is dependent on the query
	- Need the query to build the root set and base set
- PageRank is independent of a query
	- One considers the **links** between pages, nothing else
	- We can compute it **ONCE** and save the result
	- Faster at runtime

Scores from either/both algorithms can be **combined** with vector model tf-idf similarity score.
```

---
### Search Engine Optimization
Techniques to **increase the visibility** of your page in search engine results.
- More clicks, more business

Search engine needs **TEXT** to extract keywords.
- Use text rather than images for important content
- Site should work with JavaScript disabled
	- Some search engine use a JavaScript rendering engine on some pages, but don't count on it
#### TF-IDF
What can we do to influence TF-IDF?
- Pages focus on a particular topic (rare)
- You'll probably naturally use important (relevant) words multiple times
- Length won't help (normalization)
- Create a page with many repetitions of keywords
	- Search engine hates this - with a spam filter

#### Links
What can you do to influence Page Rank?
- Bots add comments to blogs. Comments link back to your site (Link Spam)
	- Use `rel="nofollow"` attributes for links in comments so search engine will ignore this
	- Search engine hates this

Another important consideration is to avoid links that look like form queries.
- `http://www.mysite.com/info?about`
Also, use **different** links for **different** **pages**.
- Different page with different content will have the **same link** in the web graph

In fact, relevancy is determined by over 200 factors, one of which is the PageRank.