[[2023-11-14]] #WebSearch #InformationRetrieval

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
### tf-idf
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
Here, $N$ is the number of total docs in collection $C$, and $n_k$ is the number of docs in $C$ that contains $T_k$. In our example,
- 
