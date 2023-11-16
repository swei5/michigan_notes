[[2023-11-15]] #InformationRetrieval 

### Document Importance
Yet another way to improve search results, other than ranking on document content, as introduced in [[15 Info Retrieval I - Text Analysis]].

A good way to visualize relations between document is through graphs. In this sense, web can be interpreted as a graph (AKA a network).
- Each web page is a vertex
- Each hyperlink is a directed edge

![[Pasted image 20231115211723.png|400]]

It's not hard to observe the following general facts:
1. If lots of pages link to a page, it is important, and in turn
2. If an important page links to a page, that page also is important

Since the 1990s, web search has been improved thanks to hyperlink graphs. We will be looking at these two algorithms.
- PageRank
- HITS

Essentially, to reflect page importance in search results, we need to
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

In graph theory, this translates to that a node with $C$ links contributes $\frac{1}{C}$ of its PageRank to each target node.
$$P(A)=\frac{1-d}{N}+d\sum\limits_{i} \frac{P(I_{i})}{C(I_{i})}$$
Where $N$ is the number of nodes in graph