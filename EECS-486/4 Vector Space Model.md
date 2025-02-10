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
We start with $t$ distinct terms remain after preprocessing (unique terms that are from the vocabulary). These $t$ -dimensional orthogonal terms form a **vector space**.
- $\dim = t$

Each term $i$, in a document (or query) $j$ is given a real-valued weight $w_{ij}$. Then, we can express both the documents and queries as $t$ -dimensional vectors: $$d_{j}=[w_{1j},w_{2j},\cdots,w_{tj}]$$

```ad-note
Note that here we regard query as **short document**.
```

We return the documents **ranked by the closeness** of their vectors to the query, also represented as a vector.

![[Pasted image 20250210004044.png|400]]

We can represent the collection of $n$ documents in the vector space model by a **term-document matrix**. Each entry in the matrix corresponds to the **weight** of a term in the document; zero means the term has no significance in the document or it simply doesn’t exist in the document. $$\begin{bmatrix} w_{11} & w_{21} & \dots & w_{t1} \\ w_{12} & w_{22} & \dots & w_{t2}   \\ \vdots  & \vdots & \ddots & \vdots  \\ w_{1n} & w_{2n} & \dots & w_{tn}\end{bmatrix}$$
#### Term Frequency
The **term frequency** $tf_{t,d}$ of term $t$ in document $d$ is defined as the **number of times** that $t$ occurs in $d$. More frequent terms in a document are more important, i.e. more indicative of the topic. We could normalize term frequency by the following: $$tf_{t,d}=\frac{f_{t,d}}{\max{(f_{t,d})}}$$
This is because raw term frequency is not what we want - Relevance does not increase **proportionally with term frequency**.
- A document with 10 occurrences of the term is more relevant (but **NOT 10 times**) than a document with 1 occurrence of the term

#### Document Frequency
We have previously noted that rare terms are more informative than frequent terms.
- Most frequent terms are stopwords

A document containing a rare term in the document is **very likely to be relevant** to the query that contains that specific term.
- We want a high weight for rare terms

We will use **document frequency** (df) to capture this. $df_{t}$ is the document frequency of $t$: the number of documents that contain $t$. It is an inverse measure of the **informativeness** of $t$. We define the $idf$ (inverse document frequency) of $t$ by $$idf_{t}=\log_{10} \left(\frac{N}{df_{t}}\right)$$ where $N$ is the total number of documents in the collection. We apply the log transformation to dampen the effect of **idf**.

```ad-note
Here we could choose either the **document frequency** (df) or the **collection frequency** - the number of occurrences of $t$ in the collection. It is often a design choice.

We would usually go with df.
```

#### tf-idf
The **tf-idf** weight of a term is the product of its tf weight and its idf weight: $$w_{t,d}=tf_{t,d} \cdot \log_{10} \left(\frac{N}{df_{t}}\right) $$
It is best known weighting scheme in information retrieval.
- Theoretically proven to work well
- **Increases** with the **number of occurrences** within a document
- **Increases** with the **rarity of the term in the collection**

```ad-example
**Example**: tf-idf calculation

![[Pasted image 20250210011859.png|500]]
```

In this way, we can transform our [[3 Boolean Retrieval#^b306f2|term-document incidence matrix]] (boolean) to term-document **frequencies** matrix, and finally to a term-document matrix represented by **weights** (tf-idf). Each document is now represented by a real-valued vector of tf-idf weights $\in \mathbb{R}^{|V|}$

Now we have the necessary tools to discuss the vector space model. In this setup, **terms** are **axes** of the space, and **documents** are **points of vectors** in this space.
- This could be very high-dimensional: tens of millions of dimensions if applied to search engines

```ad-note
Again, query is also typically treated as a document and also **tf-idf weighted**.
```

#### Similarity Measures
A similarity measure is a function that computes the degree of similarity between two vectors. Here, we care about the similarity measure between the query and each document.

Two naive approaches on top of our heads are 
- Euclidean Distance: $\sqrt{\sum(x_{i}-y_{i})^{2}}$ 
- Manhattan Distance: $\sum|x_{i}-y_{i}|$

However, we soon notice that under these approaches, documents with **NO terms in common** are **NOT penalized** by the measure.
- E.g. $(1,0)$ and $(0,1)$ have the same Manhattan Distance as $(1,0)$ and $(3,0)$

And, we still haven’t dealt with the issue of **length normalization**.
- Long documents would be more similar to each other by virtue of length, not topic

Our third cut is the **inner product**. The similarity between vectors for the document $d_{i}$ and query $q$ can be computed as the vector inner product: $$\text{sim}(d_{j},q)=d_{j}\cdot q=\sum\limits_{i=1}^{t}w_{ij}\cdot w_{iq}$$
For binary vectors, the inner product is the number of matched query terms in the document (size of intersection). For weighted term vectors, it is the sum of the products of the weights of the matched terms.

```ad-note
**Properties of Inner Product**
It favors long documents with a large number of unique terms.
- Issues with normalization

It measures how many terms matched but not how many terms are **NOT matched**.
```

Lastly, we introduce the **Cosine Similarity**. It is the distance between vector $d_{1}$ and $d_{2}$ captured by the **cosine** of the angle $\theta$ between them.

![[Pasted image 20250210013246.png|400]]

It is given by 