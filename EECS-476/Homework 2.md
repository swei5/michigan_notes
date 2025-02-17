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


##### Question 2.4 (c)

##### Question 2.4 (d)

#### Question 2.5

##### Question 2.5 (a)

##### Question 2.5 (b)

###### Question 2.5 (b)(i)

###### Question 2.5 (b)(ii)

###### Question 2.5 (b)(iii)

###### Question 2.5 (b)(iv)

###### Question 2.5 (b)(v)

###### Question 2.5 (b)(vi)