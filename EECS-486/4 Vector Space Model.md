[[2025-01-24]] #InformationRetrieval 

Boolean queries often result in either too few (0) or too many (1000s) results. It takes a lot of skill to come up with a query that produces a *manageable number* of hits.
- Hard to tune **precision vs. recall**
	- `AND` operator tends to produce high precision but low recall
	- `OR` operator gives low precision but high recall

This introduces **ranked retrieval models**. Rather than a set of documents satisfying a query expression, in **ranked retrieval**, the system returns an ordering over the (top) documents in the collection for a query.
- Return the **same number** of documents as in unranked retrieval
- However, large result sets are not an issue, we just need to show the top $k$ results
- We need a valid ranking algorithm

Another method is **free text queries** Rather than a query language of operators and expressions, the user’s query is just one or more words in a human language.

```ad-note
In principle, these are two separate choices. In practice, ranked retrieval has normally been associated with free text queries and vice versa.
```

In a ranked system, we wish to return in order the documents most likely to be useful to the searcher. This motivates some types of **score measures**. This score measures how well the document and the query **match**.

### Jaccard Coefficient
Our first take is the **Jaccard Coefficient**. It is a commonly used measure of overlap of two sets $A$ and $B$. It is defined as follows: $$\text{Jaccard}(A,B)=\frac{|A\cap B|}{|A \cup B|}$$
Note that $\text{Jaccard}(A,A)=1$ and $\text{Jaccard}(A, B)=0$ if $A \cap B = \emptyset$. It always gives a value between 0 and 1.

```ad-note
**Issues with Jaccard for Scoring**

It does not consider term frequency (how many times a term occurs in a document).
- This is because rare terms in a collection are more **informative** than frequent terms - Jaccard does not account for this

We need a more sophisticated way of normalizing for length.
```

---
### Vector Space Model 
