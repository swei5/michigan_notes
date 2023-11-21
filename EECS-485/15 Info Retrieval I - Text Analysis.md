[[2023-11-14]] #InformationRetrieval

### Search
Let's take searching on Google as an example.
- Ahead of time, Google
	- Downloads the internet, and
	- Build an index
- At run time
	- Use index
	- Rank results
	- Do it really fast and return results to client

Ranking is **important**!
- 33% of clicks on **top result**
- Web search usually returns a vast number of options
- Two methods
	- Use words on the page
	- Use links on the page
		- Google's own innovation

---
### Boolean Retrieval
For each document (webpage), there are two possible outcomes of query processing.
- `TRUE` or `FALSE`
- Exact match retrieval - are the words on the page or not
- Simplest form of search and **used to be most common**

```ad-question
How do we answer boolean queries like this efficiently?
```

Basically, we need to have a table (index) of Booleans that stores all the information. For instance, if we are looking for the answers to "Which plays of Shakespeare contain the words Brutus AND Caesar but NOT Calpurnia?", we will need something like this (1 if table contains the term and 0 otherwise)

![[Pasted image 20231115011013.png|400]]

Boolean queries simply **include** or **exclude** a document from results; when we have many hits, we need to sort (rank)!
- There is **NO** **ordering** in Boolean queries

In the subsequent segment, we will talk more about ranking.

---
### Vector Space Model

^a3a539

Think of each document is a vector of values, and there is one component for each term.
- Thus we have a **vector space**
	- A doc is a point in the space
	- The dimension of the space is almost **infinite** - a dimension for every possible term
	- The space is sparse because most documents do not have most words

Documents that are **close together** in space are also **close** in meaning.
- Essentially, we are sorting all the documents based on how close they are

![[Pasted image 20231115011836.png|400]]

In this example, it's shown that `eecs.umich.edu` and `andrewdeorio.com` are likely to be more closely linked both graphically and intuitively because they have component `andrewdeorio=1` and `eecs485=1`.

#### Vector Query Model
1. Treat a query as a short document
2. Sort documents by **increasing distance** (decreasing similarity) to the query document
3. Easy to compute, both query and doc are vectors

This system was first used in 1970 and now used by almost all IR systems.

In the previous example, if we are searching for a query `"chickens"`, then the vector we produced in the vector space will be closest to `chickens.com` and farther away from the other two websites. Hence when ranking results, `chickens.com` will come first and the rest will come in either order.

In general, docs and queries are vectors, and weight of term stored in each position.
$$D_i=w_{d_{i1}},w_{d_{i2}},\cdots,w_{d_{it}}$$
$$Q=w_{q1},w_{q2},\cdots,w_{qt}$$
$w=0$ if a term is absent.

Term **weights** indicate **length** of document vector along a dimension.

With Boolean search, the values in a vector (one for each term) are 0 or 1; next, we will consider values in $[0,1]$.

---
### TF-IDF
Stands for term frequency-inverse document frequency. This section is regarding computing the **weight** for the vector in Boolean retrieval.

#### Term Frequency
How many times the word appears in current document.
- **Higher** is better

```ad-example
Which doc is a better match for the query "kangaroo", one with a single mention of "kangaroo", or a doc that mentions it 10 times?
```

#### Document Frequency
How often a word appears in doc collection.
- Doc collection means "vocabulary", or the entire WWW
- **Lower** is better

```ad-example
Which term is more indicative of document similarity, "Book" or "Rumpelstiltskin"?

Rumpelstiltskin is a more rare word - if a rare word occurs in your search, it's probably really important.
```

#### Inverse Document Frequency
Inverse Document Frequency (IDF) provides **high values** for **rare words**, low values for common words.
$$IDF=\frac{N}{n_{k}}$$
Here, $N$ is the number of total docs in collection $C$, and $n_k$ is the number of docs in $C$ that contains $T_k$. In our example, ^5975fc
- $N$ is the number of total pages
- $n_k$ is the number of pages with the word "Rumpelstiltskin"

#### Log Inverse Document Frequency
However, is the term "book" 100,000 times less useful than "Rumpelstiltskin"?
- This hints at a log transformation
$$IDF=\log\left(\frac{N}{n_{k}}\right)$$
Here, we use a sublinear function decreases the weight of "Rumpelstiltskin" compared to "book", but still monotonically increasing, so **order** is preserved.

Now let's sum this up. Intuitively, Term-Frequency $\times$ Inverse-Document-Frequency is high for **common word in one document** that is **rare in the collection**. Thus, we have
$$w_{ik}=tf_{ik}\cdot \log\left(\frac{N}{n_{k}}\right)$$
Where 
-  $T_k$ is the term $k$ in document $D_i$
- $tf_{ik}$ is the frequency of term $T_k$ in doc $D_i$

#### TF-IDF Normalization
We also want to avoid giving **longer docs** more weight just because they are long.
- A term likely has a higher frequency in a longer doc, meaning $tf$ is most likely higher

This calls for normalization. Here, we normalize the weights to the sum of squares:
$$w_{ik}=\frac{tf_{ik}\cdot \log\left(\frac{N}{n_{k}}\right)}{\sqrt{\sum\limits_{k=1}^{t}(tf_{ik})^{2} \left(\log(\frac{N}{n_{k}})\right)^{2}}}$$

After we updated our definitions for weights (previously $\{0,1\}$), vector lengths are now TF-IDF values. However, vectors that are "**close**" are still similar in meaning.

#### Vector Space Similarity
Vector space similarity is also called the **cosine**, or **normalized inner product**.
- Depends on two adjacent vector lengths
- $=1$ when angle is zero

Hence, the similarity of two docs is
$$\text{sim}(D_{i},D_{j})=\sum\limits_{k=1}^{t} w_{ik} \cdot w_{jk}$$
In the case when $D_i$ and $D_j$ are **NOT normalized** ahead of time (at query time), this is essentially
$$\text{sim}(D_{i},D_{j})=\cos{(\theta)}=\frac{\textbf{d}_{i} \cdot \textbf{d}_{j}}{||\textbf{d}_{i}|||\textbf{d}_{j}||}=\frac{\sum_{k=1}^{t} w_{ik} \cdot w_{jk}}{\sqrt{\sum_{k=1}^{t} w_{ik}^{2} \sum_{k=1}^{t} w_{ik}^{2}}}$$ ^c16cfc

---
### Assessing Rank Quality
How to know if a ranker is any good?
- Relevance
	- Many contributing factors
	- People disagree on what is relevant

#### Assessing Quality
We first get results from an experimental search engine, then we use a large hand-marked query results from the **answer key** for the ranker.
- Published by TREC

#### Precision/Recall Curve
• True positive (TP)
	• Relevant doc returned
• False positive (FP)
	• Irrelevant doc returned
• True negative (TN)
	• Irrelevant doc not returned
• False negative (FN)
	• Relevant doc not returned

```ad-note
Positives are about documents that are returned, and negatives are about documents that are **NOT returned**.
```

We can compute precision and recall based on these statistics.
- **Precision**: fraction of **retrieved** docs that are **relevant**
	- $P=\frac{TP}{TP+FP}$
- **Recall**: fraction of **relevant** docs that are **retrieved**
	- $R=\frac{TP}{TP+FN}$

There is generally a trade-off in Precision vs. Recall.
- Recall is a **non-decreasing** function of the # of docs retrieved, but precision usually **decreases** with **more** docs retrieved

Drawbacks include
- Binary relevance (ordering are not accounted for)
- Need human judgments
- Must average over large corpus

To create a precision-recall curve, we will first need a search engine will create a total ordering on all documents, then return top $k$ results to the user.
- Later on, we can calculate precision and recall for several values of $k$, which creates the precision-recall curve

![[Pasted image 20231115132853.png|400]]

This curve tells us that docs 1,2,4,15 are relevant because they result in a **non-negative change in precision** and an **increase in recall** (implying a relevant document is retrieved).

#### Kendall's Tau
Precision and recall, however, don't tell us about the order of the results that are returned.

Kendall’s Tau is for comparing sorts and uses a real ordering of documents, not just binary ones. Let's start by an example where the correct document ordering is: $[1,2,3,4]$. We have a search engine A outputs is $[1,2,4,3]$ and search engine B's output is $[4,3,1,2]$. Intuitively, A is better, but how do we know?

Kendall's Tau has some nice properties:
- If **agreement** between 2 ranks is perfect, then $KT = 1$
- If **disagreement** is perfect, then $KT=-1$
- If rankings are **uncorrelated**, then $KT = 0$ on average

Kendall's Tau is defined as
$$\tau=\frac{n_{c}-n_{d}}{\frac{1}{2}n(n-1)}$$
Where $n_{c}$ is the number of pairs that **agree** and $n_{d}$ is the number of pairs that **disagree**. The denominator is the total number of pairs.

```ad-note
The intuition here is to compute fraction of pairwise orderings that are **consistent**.
```

The non-normalized version is called **Kendall’s Tau Distance**.

```ad-example
![[Pasted image 20231115134123.png|500]]
```

#### Mean Reciprocal Rank
The mean reciprocal rank looks at how **close** to the **top of the search results** is the **1 st correct answer**.
- Good for search results
- Great for systems that return a single guess
$$MRR=\frac{1}{|Q|}\sum_{i=1}^{|Q|} \frac{1}{\text{rank}_{i}}$$
Here, $\text{rank}_{i}$ is the rank of the **correct answer** in the $i$ th query.

```ad-example
![[Pasted image 20231115142425.png|400]]
```

