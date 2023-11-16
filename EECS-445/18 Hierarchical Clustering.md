[[2023-03-22]] #UnsupervisedLearning #Clustering

### Overview
Hard clustering algorithms include
- [[16 k-Means Clustering|k-means Clustering]]
- [[17 Spectral Clustering|Spectral clustering]]
- Agglomerative clustering

### Agglomerative Hierarchical Clustering
An issue with the previous clustering algorithms is that they **DON'T** return additional information on how the clusters **relate** to one another.

We want to build a hierarchy - a dendrogram.

#### Algorithm
![[Pasted image 20230325233308.png]]

There are a few **linkage criteria**.
1. Single linkage: $D (C_{i}, C_{j})=\min_{\overline{x}\in C_{i}; \overline{x}^{\prime} \in C_{j}} d(\overline{x},\overline{x}^{\prime})$
	- Susceptible to **outliers** and **chaining phenomenon**: larger clusters are naturally biased toward having **closer distances** to other points and will therefore attract a successively larger number of points
2. Complete linkage: $D (C_{i}, C_{j})=\max_{\overline{x}\in C_{i}; \overline{x}^{\prime} \in C_{j}} d(\overline{x},\overline{x}^{\prime})$
	- Susceptible to **outliers** but tends to merge clusters so that all clusters tend to have the **same diameter**
3. Average-linkage: $D (C_{i}, C_{j})=\frac{1}{|C_{i}|} \frac{1}{|C_{j}|} \sum\limits_{\overline{x}\in C_{i}; \overline{x}^{\prime}\in C_{j}} d(\overline{x}, \overline{x}^{\prime})$
	- This is less **susceptible** to outliers but leads to **inefficiency** concerns