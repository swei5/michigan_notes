EECS 476 Homework 2
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
