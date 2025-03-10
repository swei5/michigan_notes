**EECS 476 Project 2**
Vincent Wei ([vswei@umich.edu](mailto:vswei@umich.edu))
February 24, 2025

## Part 1
### Question 1.3
| Method                                    | LSH Runtime | Total Runtime | # Candidate Pairs |
| ----------------------------------------- | ----------- | ------------- | ----------------- |
| LSH Shingle $1$: `k=10, n=120, r=5, b=24` | $0.00$      | $58.03$       | $209$             |
| LSH Shingle $2$: `k=10, n=120, r=20, b=6` | $0.00$      | $58.11$       | $26$              |
| LSH Shingle $3$: `k=10, n=120, r=60, b=2` | $0.00$      | $57.83$       | $12$              |
| LSH Shingle $4$: `k=3, n=120, r=20, b=6`  | $0.00$      | $4.22$        | $23$              |
| Naive Shingle $1$: `k=3`                  | N/A         | $1.75$        | $20$              |
| Naive Shingle $2$: `k=10`                 | N/A         | $41.91$       | $11$              |
Runtimes recorded in seconds. 

The runtime increases with larger shingles in the naive method, with `k=3` taking $1.75$ s and `k=10` taking 41.91s. LSH reduces candidate pairs but seemingly adds overhead for building hash functions. For example for `k=10`, LSH runtimes are around $58$ s, higher than the naive method, due to this extra cost. The impact of LSH parameters like $b$ and $r$ is minimal, with the primary factor being the shingle size. The number of candidate pairs is significantly reduced in LSH compared to the naive method, with LSH variants having between $11$ and $209$ pairs, while naive methods result in 20 and 11 pairs, respectively, reflecting LSH's efficiency in pruning non-relevant candidates.

### Question 1.4
| Method                                    | False Positive | False Negative |
| ----------------------------------------- | -------------- | -------------- |
| LSH Shingle $1$: `k=10, n=120, r=5, b=24` | $198$          | $0$            |
| LSH Shingle $2$: `k=10, n=120, r=20, b=6` | $17$           | $2$            |
| LSH Shingle $3$: `k=10, n=120, r=60, b=2` | $9$            | $8$            |
| LSH Shingle $4$: `k=3, n=120, r=20, b=6`  | $3$            | $0$            |

### Question 1.5
| Method                                    | Expected False Negative           | Expected False Positive |
| ----------------------------------------- | --------------------------------- | ----------------------- |
| LSH Shingle $1$: `k=10, n=120, r=5, b=24` | $(1-0.9^5)^{20}=1.76\cdot10^{-8}$ | $0.9999$                |
| LSH Shingle $2$: `k=10, n=120, r=20, b=6` | $0.4594$                          | $0.5406$                |
| LSH Shingle $3$: `k=10, n=120, r=60, b=2` | $0.9964$                          | $0.0036$                |
| LSH Shingle $4$: `k=3, n=120, r=20, b=6`  | $0.4594$                          | $0.5406$                |
The expected and observed false positives and negatives align, confirming LSH's trade-offs between recall and precision. Higher $r$ increases false negatives, while higher $b$ raises false positives. 

### Question 1.6
The candidate pairs returned by the variant for `k=10` is a subset of what's returned by `k=3`. Most of the candidate pairs that were **NOT** included in variant for `k=10` but for `k=3` turned out to be false negatives, i.e. those are actually very much identical documents by human judgement but were excluded due to insufficient overlapping shingles. This suggests that a smaller $k$ captures more potential near-duplicates but may also introduce more false positives. To improve the algorithm, $k$ should be set based on the desired balance between precision and recall, with a moderate value ensuring that near-duplicates are detected while minimizing irrelevant matches. In this case, we could benefit by choosing a lower $k$ value.

### Question 2.3
| Method                             | Total Runtime | # Candidate Pairs |
| ---------------------------------- | ------------- | ----------------- |
| LSH Cosine $1$: `n=120, r=5, b=24` | $0.03$        | $2774$            |
| LSH Cosine $2$: `n=120, r=20, b=6` | $0.02$        | $112$             |
| LSH Cosine $3$: `n=120, r=60, b=2` | $0.02$        | $10$              |
| Naive Cosine: `k=10`               | $0.01$        | $56$              |
Runtimes recorded in seconds. 

As $r$ increases and $b$ decreases, the number of candidate pairs drops, making LSH more selective similar to what we saw in previous questions. LSH can capture more potential near-duplicates than the naive method but may introduce more false positives.

### Question 2.4
| Method                             | False Positive | False Negative |
| ---------------------------------- | -------------- | -------------- |
| LSH Cosine $1$: `n=120, r=5, b=24` | $2718$         | $0$            |
| LSH Cosine $2$: `n=120, r=20, b=6` | $69$           | $13$           |
| LSH Cosine $3$: `n=120, r=60, b=2` | $0$            | $46$           |

### Question 2.5
The embeddings and cosine similarity approach identified significantly more near-duplicate pairs ($56$ vs. $20$) compared to shingling and Jaccard similarity. Most of these additional pairs were genuinely similar based on human judgment, suggesting that word embeddings capture semantic similarities more effectively than shingling, which relies on exact substring matches. However, one document pair was detected by shingling but not embeddings, indicating that embeddings may sometimes miss certain structural similarities. Overall, embeddings with cosine similarity are more powerful for near-duplicate detection, as they consider meaning rather than just textual overlap.

---
## Part 2
### Question 2.1

![[n2_random.csv.png|400]]

### Question 2.2 

![[n2_select.csv.png|400]]

### Question 2.3
| Algorithm              | % change in cost |
| ---------------------- | ---------------- |
| Random Initialization  | $-26.7\%$        |
| SelectC Initialization | $-79.1\%$        |
No, random initialization is **NOT** better than initialization using `selectC.txt` in terms of cost. The results suggests that `selectC.txt` likely provides a better starting point for $k$ -means, positioning centroids closer to natural clusters, leading to faster convergence and lower final cost.

### Question 2.4
Based on the cost progression over 25 episodes for random initialization, the cost starts to stabilize around the $6$ th or $7$ th cluster. After that, the reduction in cost becomes much smaller. Thus, I would pick $k=7$ as the optimal number of clusters, as it provides a good balance between reducing cost and avoiding overfitting.

### Question 2.5
Proof by contradiction: Let's assume that after updating the centroid, the cost increases. That is, the squared distances between the points and the new centroids are greater than the squared distances to the old centroids. If the cost increased after updating the centroid, it would mean that at least one point is farther away from the new centroid than it was from the old one. However, by the way the centroids are updated (as the mean of the points), this cannot happen because the mean minimizes the sum of squared distances.

Therefore, if a point were farther away from the new centroid, it would have been assigned to a different cluster in the previous iteration, because $k$ -means assigns each point to the closest centroid. This contradicts with our initial assumption.

### Question 3.1

![[n1_random.csv.png|400]]
### Question 3.2

![[n1_select.csv.png|400]]

### Question 3.3
| Algorithm              | % change in cost |
| ---------------------- | ---------------- |
| Random Initialization  | $-17.7\%$        |
| SelectC Initialization | $-54.0\%$        |
No, random initialization is **NOT** better than initialization using `selectC.txt` in terms of cost. The results suggests that `selectC.txt` likely provides a better starting point for $k$ -means, positioning centroids closer to natural clusters, leading to faster convergence and lower final cost.

---
## Part 3

### Question 2
- Precision: $0.5333333333333333$
- Recall: $0.13333333333333333$
- F 1 Score: $0.21333333333333335$

### Question 3
It could be that `k=1` is too small, making the algorithm unreliable in estimating local density. Increasing $k$ could potentially provide a better comparison. In addition, the cutoff `c=1.3` maybe too high and conservative for the dataset, which could result in a significant number of false negatives.

### Question 4

![[precision_vs_c.png|400]]

![[recall_vs_c.png|400]]

![[f1_vs_c.png|400]]

`c=1` results in the highest F1-score.

### Question 5

![[Figure_1 1.png|400]]
It seems that fake and legit articles cluster differently in the reduced space as shown by the visualization of the embeddings.

### Question 6
The top outliers found were `260`, `460`, `116`, `159`, and `353`. The top-5 outliers, marked in black, are located in a transition region between the two distinct clusters (red and blue) in the t-SNE plot below. This suggests that these points have ambiguous characteristics, making them stand out as anomalies. LOF is working well in this case because it identifies points with lower local density compared to their neighbors, which aligns with the observed structure of the data.

![[Pasted image 20250308210028.png|400]]


### Question 7
$k$ -means could be used for identifying fake news by clustering articles based on their embeddings.  To choose the optimal number of clusters $k$ for identifying fake news, I would start with the elbow method, which involves plotting the sum of squared distances for different values of $k$ and selecting the point where the decrease in inertia starts to level off. This gives $k=3$  as a potential candidate. I do not expect $k$ -means to work better than the LOF algorithm for identifying fake news in this context. $k$ -means is a clustering algorithm that assumes clusters are spherical and well-separated, which may not always hold for high-dimensional text embeddings. It forces every data point into a cluster, making it less effective at identifying true anomalies or outliers.

![[Pasted image 20250308211429.png|400]]