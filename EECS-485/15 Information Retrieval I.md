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

