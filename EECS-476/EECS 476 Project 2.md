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
| Naive Shingle $1$: `k=3`                  | N/A         | 41.91         | 11                |
| Naive Shingle $2$: `k=10`                 | N/A         | 1.75          | 20                |
Runtimes recorded in seconds. 

The runtime increases with larger shingles in the naive method, with `k=3` taking $1.75$ s and `k=10` taking 41.91s. LSH reduces candidate pairs but seemingly adds overhead for building hash functions. For example for `k=10`, LSH runtimes are around $58$ s, higher than the naive method, due to this extra cost. The impact of LSH parameters like `b` and `r` is minimal, with the primary factor being the shingle size. The number of candidate pairs is significantly reduced in LSH compared to the naive method, with LSH variants having between $11$ and $209$ pairs, while naive methods result in 20 and 11 pairs, respectively, reflecting LSH's efficiency in pruning non-relevant candidates.

### Question 1.4
| Method                                    | False Positive | False Negative |
| ----------------------------------------- | -------------- | -------------- |
| LSH Shingle $1$: `k=10, n=120, r=5, b=24` | 198            | 0              |
| LSH Shingle $2$: `k=10, n=120, r=20, b=6` | 17             | 2              |
| LSH Shingle $3$: `k=10, n=120, r=60, b=2` | 9              | 8              |
| LSH Shingle $4$: `k=3, n=120, r=20, b=6`  | 3              | 0              |
### Question 1.5
| Method                                    | Expected False Positive | Expected False Negative |
| ----------------------------------------- | ----------------------- | ----------------------- |
| LSH Shingle $1$: `k=10, n=120, r=5, b=24` |                         |                         |
| LSH Shingle $2$: `k=10, n=120, r=20, b=6` |                         |                         |
| LSH Shingle $3$: `k=10, n=120, r=60, b=2` |                         |                         |
| LSH Shingle $4$: `k=3, n=120, r=20, b=6`  |                         |                         |
### Question 1.6
