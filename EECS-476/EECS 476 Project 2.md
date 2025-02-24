**EECS 476 Project 2**
Vincent Wei ([vswei@umich.edu](mailto:vswei@umich.edu))
February 24, 2025

## Part 1
### Question 1.3
| Method                                    | LSH Runtime | Total Runtime | # Candidate Pairs |
| ----------------------------------------- | ----------- | ------------- | ----------------- |
| LSH Shingle $1$: `k=10, n=120, r=5, b=24` | 0.00        | 58.03         | 209               |
| LSH Shingle $2$: `k=10, n=120, r=20, b=6` | 0.00        | 58.11         | 26                |
| LSH Shingle $3$: `k=10, n=120, r=60, b=2` | 0.00        | 57.83         | 12                |
| LSH Shingle $4$: `k=3, n=120, r=20, b=6`  | 0.00        | 4.22          | 23                |
| Naive Shingle $1$: `k=3`                  | N/A         | 1.75          | 20                |
| Naive Shingle $2$: `k=10`                 | N/A         | 41.91         | 11                |
Runtimes recorded in seconds. 

The runtime increases with larger shingles in the naive method, with `k=3` taking $1.75$ s and `k=10` taking 41.91s. LSH reduces candidate pairs but seemingly adds overhead for building hash functions. For example for `k=10`, LSH runtimes are around $58$ s, higher than the naive method, due to this extra cost. The impact of LSH parameters like $b$ and $r$ is minimal, with the primary factor being the shingle size. The number of candidate pairs is significantly reduced in LSH compared to the naive method, with LSH variants having between $11$ and $209$ pairs, while naive methods result in 20 and 11 pairs, respectively, reflecting LSH's efficiency in pruning non-relevant candidates.

### Question 1.4
| Method                                    | False Positive | False Negative |
| ----------------------------------------- | -------------- | -------------- |
| LSH Shingle $1$: `k=10, n=120, r=5, b=24` | 198            | 0              |
| LSH Shingle $2$: `k=10, n=120, r=20, b=6` | 17             | 2              |
| LSH Shingle $3$: `k=10, n=120, r=60, b=2` | 9              | 8              |
| LSH Shingle $4$: `k=3, n=120, r=20, b=6`  | 3              | 0              |

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
| LSH Cosine $1$: `n=120, r=5, b=24` | 0.03          | 2774              |
| LSH Cosine $2$: `n=120, r=20, b=6` | 0.02          | 112               |
| LSH Cosine $3$: `n=120, r=60, b=2` | 0.02          | 10                |
| Naive Cosine: `k=10`               | 0.01          | 56                |
Runtimes recorded in seconds. 

As $r$ increases and $b$ decreases, the number of candidate pairs drops, making LSH more selective similar to what we saw in previous questions. LSH can capture more potential near-duplicates than the naive method but may introduce more false positives.

### Question 2.4
| Method                             | False Positive | False Negative |
| ---------------------------------- | -------------- | -------------- |
| LSH Cosine $1$: `n=120, r=5, b=24` | 2718           | 0              |
| LSH Cosine $2$: `n=120, r=20, b=6` | 69             | 13             |
| LSH Cosine $3$: `n=120, r=60, b=2` | 0              | 46             |

### Question 2.5
The embeddings and cosine similarity approach identified significantly more near-duplicate pairs ($56$ vs. $20$) compared to shingling and Jaccard similarity. Most of these additional pairs were genuinely similar based on human judgment, suggesting that word embeddings capture semantic similarities more effectively than shingling, which relies on exact substring matches. However, one document pair was detected by shingling but not embeddings, indicating that embeddings may sometimes miss certain structural similarities. Overall, embeddings with cosine similarity are more powerful for near-duplicate detection, as they consider meaning rather than just textual overlap.

---
## Part 2
### Question 2.1
