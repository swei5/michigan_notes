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