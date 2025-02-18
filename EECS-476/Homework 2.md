**EECS 476 Homework 2**
Vincent Wei ([vswei@umich.edu](mailto:vswei@umich.edu))
February 20, 2025

### Question 1
#### Question 1.1
If an itemset is frequent in the full dataset (support $\ge s$), then in expectation, it should appear with support at least $p \cdot s$ in random subsets. If it is never found frequent in any subset, its total support must be less than $s$, meaning it cannot be truly frequent. Since the SON algorithm selects candidates from subsets, all globally frequent itemsets must appear in at least one subset if an itemset is frequent in the full dataset.

#### Question 1.2
##### Question 1.2 (i)
We will first split each signature vector into `num_bands` bands, where each band consists of a contiguous subset of hash values. We obtain Using `flatMap`, we generate tuples of the form `((band_index, hash(band_signature)), column_index)`, (`band_signature` and `column_index` are obtained from `column`) where the hash function is applied to each band to map it to a bucket. Next, we call `groupByKey` to aggregate all columns that fall into the same bucket. To retrieve candidate pairs, we filter out singleton buckets using `filter`, keeping only those that contain multiple columns. Finally, we apply `flatMap` and `combinations` to generate unique candidate pairs for similarity check.

##### Question 1.2 (ii)
Here we are trying to balance the trade-off between false positives and false negatives in LSH. Increasing $b$ makes it easier for similar items to be considered candidates, while increasing $r$ makes the hash function more selective, reducing false positives but potentially missing similar pairs

#### Question 1.3
Jaccard similarity is more suitable for comparing **sets** of words or shingles in documents because it measures overlap without considering term frequency, making it effective for detecting copied or highly similar text, such as document plagiarism detection.

Cosine similarity is better for user-item recommendation models since it captures the angle between high-dimensional vectors, making it effective in comparing user preferences based on **term frequency**.

#### Question 1.4
$k$ -Means is well-suited for reducing the number of colors in an image by clustering similar pixel colors, as it efficiently assigns each pixel to a centroid, making it computationally efficient for large images.

Hierarchical clustering could be useful for detecting fraudulent transactions because it can uncover nested patterns and relationships in transaction data, making it useful for identifying anomalies at different levels of similarity.

#### Question 1.5
Distance-based outlier detection identifies outliers as points that are far from most other points, while Density-based outlier detection considers local data density, flagging points as outliers if they exist in low-density regions compared to their neighbors. Density-based outlier detection could be useful when data has clusters of varying densities or irregular shapes, as they can detect outliers more effectively in non-uniform distributions.

---
### Question 2
#### Question 2.1 
##### Question 2.1 (i)
| Item | Support Count |
| ---- | ------------- |
| 1    | 4             |
| 2    | 5             |
| 3    | 7             |
| 4    | 7             |
| 5    | 6             |
| 6    | 6             |

| Item Pair | Support Count |
| --------- | ------------- |
| (1,2)     | 2             |
| (1,3)     | 3             |
| (2,3)     | 3             |
| (1,5)     | 1             |
| (3,5)     | 3             |
| (2,4)     | 4             |
| (3,4)     | 2             |
| (2,5)     | 1             |
| (4,5)     | 4             |
| (3,6)     | 3             |
| (5,6)     | 3             |
| (4,6)     | 3             |
| (1,4)     | 1             |
| (1,6)     | 1             |
| (2,6)     | 2             |

##### Question 2.1 (ii)
Frequent items: $1,2,3,4,5,6$.
Frequent pairs of items: $\{2,4\}, \{4,5\}$.

##### Question 2.1 (iii)
| Basket | Pairs               | Count |
| ------ | ------------------- | ----- |
| B0     | (4,6)               | 3     |
| B1     | (5,6)               | 3     |
| B2     |                     |       |
| B3     | (1,2)               | 2     |
| B4     | (1,3)               | 3     |
| B5     | (2,3), (1,4)        | 4     |
| B6     | (1,5), (2,4)        | 5     |
| B7     | (3,4), (2,5), (1,6) | 4     |
| B8     | (3,5), (2,6)        | 5     |
| B9     | (4,5), (3,6)        | 8     |

##### Question 2.1 (iv)
The candidate pairs are $\{2,3\}$, $\{1,4\}$, $\{1,5\}$, $\{2,4\}$, $\{3,4\}$, $\{2,5\}$, $\{1,6\}$, $\{3,5\}$, $\{2,6\}$, $\{4,5\}$, and $\{3,6\}$ after pass 1.

| Basket | Pairs               | Count |
| ------ | ------------------- | ----- |
| B0     | (4,5), (3,6)        | 8     |
| B1     |                     |       |
| B2     |                     |       |
| B3     |                     |       |
| B4     |                     |       |
| B5     | (2,3), (1,4)        | 4     |
| B6     | (1,5), (2,4)        | 5     |
| B7     | (3,4), (2,5), (1,6) | 4     |
| B8     | (3,5), (2,6)        | 5     |

##### Question 2.1 (v)
No.

#### Question 2.2 
The largest number of $k$ -shingles is $n-k+1$.
- First $k$ -shingle starts at position 0 and ends at index $k-1$, last $k$ -shingle starts at index $n-k$ and ends at index $n-1$ (0-indexed)

#### Question 2.3
##### Question 2.3 (a)
Sentence 1: $[1, 1, 1, 1, 1, 1, 1, 0, 0]$
Sentence 2: $[1, 1, 1, 0, 1, 1, 1, 1, 1]$

##### Question 2.3 (b)
$$\text{cos\_sim}(\mathbf{x}, \mathbf{y})= \frac{6}{\sqrt{7}\sqrt{8}}=0.802$$
##### Question 2.3 (c)
$$\text{dist}(\mathbf{x}, \mathbf{y})=\sqrt{3}=1.732$$
##### Question 2.3 (d)
Because $\mathbf{x}, \mathbf{y}$ are normalized, $\text{cos\_sim}(\mathbf{x}, \mathbf{y})=\mathbf{x}^{T}\mathbf{y}$ . Then, $$\begin{align}
\text{dist}(\mathbf{x}, \mathbf{y}) &= ||\mathbf{x}-\mathbf{y}||\\
&= \sqrt{(\mathbf{x}-\mathbf{y})^{T}(\mathbf{x}-\mathbf{y})}\\
&= \sqrt{1-2 \mathbf{x}^{T}\mathbf{y}+1}\\
&= \sqrt{2-2\text{cos\_sim}(\mathbf{x}, \mathbf{y})}
\end{align}$$
##### Question 2.3 (e)
Sentence 1b: $[1, 1, 1, 1, 1, 1, 0, 0, 0, 1]$
Sentence 2:   $[1, 1, 1, 0, 1, 1, 1, 1, 1, 0]$

$$\text{cos\_sim}(\mathbf{x}, \mathbf{y})= \frac{5}{\sqrt{7}\sqrt{8}}=0.668$$
$$\text{dist}(\mathbf{x}, \mathbf{y})=\sqrt{5}=2.236$$

##### Question 2.3 (f)
The presence of synonyms reduced the similarity and distance measures between two sentences, which, by human judgement, were inherently identical prior/after the synonym change. One key limitation seen here is that Bag-of-words fails to capture semantic meaning because they ignore word order, context, and relationships between words.

##### Question 2.3 (g)
- Similarity between Sentence 1, Sentence 2: $0.959$
- Similarity between Sentence 1b, Sentence 2: $0.951$
- Euclidean Distance between Sentence 1, Sentence 2: $2.858$
- Euclidean Distance between Sentence 1b, Sentence 2: $3.104$

##### Question 2.3 (h)
When using the bag-of-words model, replacing a word with its synonym caused a noticeable drop in cosine similarity and an increase in Euclidean distance, even though the sentences were essentially the same in meaning. When using embeddings, the similarity scores between Sentence 1 and Sentence 2, as well as between Sentence 1b and Sentence 2, remained much higher and suggests that embeddings better capture synonymy and meaning, leading to more accurate similarity measurements.

#### Question 2.4

##### Question 2.4 (a)
Centroid: $(10.500, 4.250)$
Diameter: $5.000$

##### Question 2.4 (b)
$(11,4)$.

##### Question 2.4 (c)
$(10,5)$.

##### Question 2.4 (d)
Centroid: The centroid shifts towards the outlier because it is the average of all points.
Diameter: The diameter increases significantly since the maximum pairwise distance will likely involve the outlier.
Clustroid: The clustroid may change as the point that minimizes the maximum or sum of distances may shift, depending on how the outlier affects the overall distance structure.

#### Question 2.5

##### Question 2.5 (a)
We would see a bigger benefit from using LSH in the database with a large number of near-duplicate documents. This is because LSH hashes similar documents into the same buckets with high probability, allowing us to only compare documents within buckets rather than all pairs and hence reducing the number of pairwise comparisons.

##### Question 2.5 (b)

###### Question 2.5 (b)(i)
First, we initialize the document matrix:

|     | $C_1$ | $C_2$ | $C_3$ | $C_4$ |
| --- | ----- | ----- | ----- | ----- |
| $0$ | $0$   | $0$   | $1$   | $0$   |
| $1$ | $1$   | $0$   | $0$   | $1$   |
| $2$ | $0$   | $0$   | $1$   | $0$   |
| $3$ | $0$   | $1$   | $0$   | $1$   |
| $4$ | $1$   | $0$   | $1$   | $1$   |
| $5$ | $0$   | $1$   | $0$   | $1$   |

Then, for each hash function $h$ and row $r$ we compute the corresponding hash values: 

|       | $C_1$ | $C_2$ | $C_3$ | $C_4$ | $h_1 (x)$ | $h_2 (x)$ | $h_3 (x)$ | $h_4 (x)$ |
| ----- | ----- | ----- | ----- | ----- | --------- | --------- | --------- | --------- |
| $r_0$ | $0$   | $0$   | $1$   | $0$   | $0$       | $3$       | $1$       | $1$       |
| $r_1$ | $1$   | $0$   | $0$   | $1$   | $1$       | $5$       | $2$       | $4$       |
| $r_2$ | $0$   | $0$   | $1$   | $0$   | $2$       | $7$       | $3$       | $2$       |
| $r_3$ | $0$   | $1$   | $0$   | $1$   | $3$       | $9$       | $4$       | $0$       |
| $r_4$ | $1$   | $0$   | $1$   | $1$   | $4$       | $1$       | $5$       | $3$       |
| $r_5$ | $0$   | $1$   | $0$   | $1$   | $5$       | $3$       | $6$       | $1$       |
Finally, we can derive the minhash signatures based on the minhash values computed in the previous step.

|       | $C_1$ | $C_2$ | $C_3$ | $C_4$ |
| ----- | ----- | ----- | ----- | ----- |
| $h_1$ | $1$   | $3$   | $0$   | $1$   |
| $h_2$ | $1$   | $3$   | $1$   | $1$   |
| $h_3$ | $2$   | $4$   | $1$   | $2$   |
| $h_4$ | $3$   | $0$   | $1$   | $0$   |

###### Question 2.5 (b)(ii)

|       | $C_1$ | $C_2$ | $C_3$ | $C_4$ |
| ----- | ----- | ----- | ----- | ----- |
| $C_1$ | $1$   | $1/5$ | $1/4$ | $1/2$ |
| $C_2$ |       | $1$   | $1/4$ | $1/5$ |
| $C_3$ |       |       | $1$   | $2/3$ |
| $C_4$ |       |       |       | $1$   |

###### Question 2.5 (b)(iii)
First band $b_{1}$: $$\begin{bmatrix}1 & 3 & 0 & 1  \\  1 & 3 & 1 & 1\end{bmatrix}$$
After applying the hash function, 
- $h(1,1)=3$ - $d_{1}$ to bucket $3$
- $h(3,3)=5$ - $d_{2}$ to bucket $5$
- $h(0,1)=2$ - $d_{3}$ to bucket $2$
- $h(1,1)=3$ - $d_{4}$ to bucket $3$

Second band $b_{2}$: : $$\begin{bmatrix}2 & 4 & 1 & 2  \\  3 & 0 & 1 & 0\end{bmatrix}$$
After applying the hash function,
- $h(2,3)=0$ - $d_{1}$ to bucket $0$
- $h(4,0)=6$ - $d_{2}$ to bucket $6$
- $h(1,1)=3$ - $d_{3}$ to bucket $3$
- $h(2,0)=4$ - $d_{4}$ to bucket $4$

The only candidate pair is $(d_{1},d_{4})$ since they are both hashed to bucket $3$ in $b_{1}$.

###### Question 2.5 (b)(iv)
The number of candidate pairs would have increased - since there are more bands, there are more chances for a pair of documents to be hashed into the same bucket in at least one band. This increases the likelihood of finding candidate pairs compared to using just one band.

###### Question 2.5 (b)(v)
The number of candidate pairs would have decreased - since the entire signature of each document is considered as a single band, all $4$ rows are hashed together. This makes it harder for documents to be considered similar unless their entire signature matches across all rows in that single band.

###### Question 2.5 (b)(vi)
No. Any $b,r$ such that $br=4$ works (for the length of the signature for each document is $4$); however, $b,r$ are integers and the only permutations are aforementioned $(2,2), (4,1)$, and $(1,4)$.
