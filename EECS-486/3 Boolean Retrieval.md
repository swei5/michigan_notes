[[2025-01-17]] #InformationRetrieval 

Before we take a deep dive into a typical IR system, let's focus on a more simplified, **Boolean IR task**.
- E.g. 
	- IR problem: *Which plays by Shakespeare mention Brutus and Caesar, but not Calpurnia?*
	- Boolean Query: *Brutus AND Caesar AND NOT Calpurnia*

![[Pasted image 20250208223009.png|400]]

A naive approach would be to **compile list of documents** that contain the queried words. It is simple, and works well for moderately sized data, but is very slow for large corpora since we need to do **linear scan** for every query.

A better idea is to compute and store **term-document incidence matrices**. We precompute a data structure that makes search fast for every query.

![[Pasted image 20250208223454.png|400]]

Then, we run our queries based off of the matrix.

![[Pasted image 20250208223648.png|400]]

```ad-note
Operator precedence: NOT vs AND vs OR.
```

However, this is **NOT scalable**. Assume corpus has 1 million documents, each document is about 1,000 words long, each word takes 6 bytes, on average, and of the 1 billion word tokens, 500,000 are unique.
- Then, the corpus will take $1M \cdot 1000 \cdot 6=6$ GB
- Term-Document incidence matrix would take: $500000\cdot 1000000=0.5\cdot 10^{12}$ bits

Of the 500 billion entries, at most 1 billion are **non-zero**!
- $99.8\%$ of the entries are zero.
- We could use a sparse representation to reduce storage size -> **inverted index**

### Inverted Index, Boolean Retrieval
We map each term to a posting list of documents containing it.
- Identify each document by a numerical `docID`
- Dictionary of terms usually **in memory**
- Posting list
	- Linked lists of variable-sized arrays

![[Pasted image 20250208224230.png|400]]

**Step 1**
Assemble sequence of `<token, docID>` pairs. 
- Assume text has been tokenized

**Step 2**
Sort by **terms**, then by `docID`s.

**Step 3**
Merge multiple term entries **per document** and split into dictionary and posting lists. 
- Keep posting lists sorted
And, add document **frequency information**.
- Useful for efficient query processing and in document ranking

![[Pasted image 20250208225049.png|400]]

#### Query Processing: AND
Consider processing the query: `A AND B`:
- Locate `A` in the dictionary, retrieve its postings
- Locate `B` in the dictionary, retrieve its postings
- Merge the two postings (intersect the document sets)

A possible implementation may look something like an intersect: 

![[Pasted image 20250208225505.png|300]]

#### Query Processing: OR
We can adapt the `AND` merge algorithm for an `OR` query. This is a union.

#### Query Optimization
For `AND` queries, we want to process in order of increasing frequencies:
- Start with **smallest set**, then keep cutting further
- Use document frequencies stored in the dictionary

```ad-example
**Example**: Query Optimization

Consider a query that is an `AND` of $n$ terms:

![[Pasted image 20250208230125.png|400]]

In this case, we should execute the query as (Calpurnia `AND` Brutus) `AND` Caesar.
```

---
### Boolean Model, Extension
1. **Phrase Queries**
	- Want to **answer the query** as a phrase
	- The concept of phrase queries is one of the few “advanced search” ideas that has proven easily understood by users
		- About 10% of web queries are phrase queries
		- Many more are implicit phrase queries (e.g. person names)
2. **Proximity Queries**
	- Altavista: Python NEAR language
	- Google: Python * language
	- Many search engines use keyword proximity implicitly

#### Phase Queries: Biword Indices
We can index every two consecutive tokens in the text.
- Treat each biword (or bigram) as a vocabulary term
- **Bigram phrase query processing** is now straightforward

For longer phrase queries, we can break them into **conjunction of biwords**.
- E.g. *electrical engineering and computer science* -> *electrical engineering* `AND` *engineering and* `AND` *and computer* `AND` *computer science*

However, there's a few drawbacks:
1. Can have **false positives**
	- Unless retrieved docs are verified
2. Larger dictionary leads to **index blowup**
	- Clearly unfeasible for ngrams larger than bigrams
3. It is not a standard solution for phrase queries

#### Phase Queries: Positional Indices

```bash
for each token:
	for each docID:
		store the positions where tok appears in docID
```

![[Pasted image 20250208231855.png|300]]

We use a merge algorithm at two levels:
1. **Postings level**, to find matching docIDs for query tokens
2. **Document level**, to find consecutive positions for query tokens

For example, if we want to query for the phrase *to be or not to be*, we first
1. Extract index entries for each distinct term: *to, be, or, not*
2. Merge their doc: position lists to enumerate all positions with *to be or not to be*:
	- *to*: `[...]`
	- *be*: `[...]`

A positional index expands **postings storage** substantially, because we need an entry for each occurrence, not just for each document.
- 2 to 4 times as large as a non-positional index

Nevertheless, a positional index is now standardly used because of the **power and usefulness of phrase and proximity queries**.

#### Phase Queries: Combined Strategy
Biword and positional indexes can be fruitfully combined.
1. Use a phrase index, or a biword index, for certain queries
	- Queries known to be **common** based on recent querying behavior
	- Queries where the individual words are common but the **desired phrase is comparatively rare**
1. Use a positional index for remaining phrase queries

```ad-summary
Boolean queries are **precise**:
- A document either matches the query or it does not
- However, hard to tune precision vs. recall:
	- `AND` operator tends to produce high precision but low recall
	- `OR` operator gives low precision but high recall

Most search engines include at least partial implementations of Boolean models.
- Boolean operators
- Phrase search

Still, main search is generally focused on free text queries: **vector space model**.
```
