**EECS 476 Homework 3**
Vincent Wei ([vswei@umich.edu](mailto:vswei@umich.edu))
March 27, 2025

## Question 1
### (a)
- A, C, E
### (b)
- C
### (c)
- A, B, C, D

### (d)
By definition of eigenvector, eigenvalue and for some arbitrary square matrix $A$, $$\begin{align}
(A^{T}A)x=\lambda x
\end{align}$$ for some eigenvalue $\lambda$ and eigenvector $x$. Multiply $A$ on both sides (given $\lambda \ne 0$), we have $$(AA^{T})Ax=\lambda Ax$$ where $Ax$ is the eigenvector of $AA^{T}$ and $\lambda$ again is the eigenvalue. Hence, we can conclude $AA^{T}$ and $A^{T}A$ share the same non-zero eigenvalues. The setup above also shows the eigenvectors of the two are not necessarily the same as $Ax\ne x$ in all cases.

---
## Question 2
### (a)
We first compute total energy by the singular values: $$\text{Total Energy}=\sum\limits_{i=1}^{r} \sigma_{i}^{2}=261.85$$ We want to find rank $k$ such that $\sum_{i}^{k} \sigma_{i}^{2} / \text{Total Energy}$ is $0.9$. We observe that
- For $k=1$, $169 / 261.85=0.645$
- For $k=2, 253.64/261.85=0.969$, which is greater than $0.9$

Hence we retain the first two singular values. Thus, the low-rank approximation is $$U_{k}\Sigma_{k}V^{T}_{k}=\begin{bmatrix}-0.12 & 0.03 \\ -0.41 & 0.08  \\  \vdots & \vdots  \\ -0.13 & -0.27\end{bmatrix} \begin{bmatrix}13 & 0  \\ 0 & 9.2\end{bmatrix} \begin{bmatrix}-0.51 & -0.56 & \dots & -0.3  \\ 0.28 & 0.13 & \dots & -0.64\end{bmatrix}$$

### (b)
We can compute $\hat{M}=U_{k}\Sigma_{k}V^{T}_{k}$ explicitly using technology and obtained $$\hat{M}= \begin{bmatrix}1.002 & 0.969 & 1.002 & -0.088 & -0.003  \\ 2.924 & 3.080 & 2.924 & 0.854 & 1.128 \\ \vdots & & \ddots & & \vdots  \\ 0.166 & 0.623 & 0.166 & 2.037 & 2.097 \end{bmatrix}$$
We note that the reconstructed $\hat{M}$ is fairly close to the original matrix $M$ in terms of values and thus captures most of the structure.

### (c)
We can represent Sally's preference as $$q=\begin{bmatrix}0 & 0 &0 & 3 & 0\end{bmatrix}$$ We could then map her preference to the concept space using lower-rank SVD we just constructed and thus obtain $$qV_{k}=\begin{bmatrix}-0.75 & -1.95\end{bmatrix}$$
We can obtain recommendations for Sally in the movie space by multiplying the concept vector with $V^{T}$: $$\begin{bmatrix}-0.75 & -1.95\end{bmatrix} \times V_{k}^{T}=\begin{bmatrix}-0.164 & -0.167 & -0.164 & 1.455 & 1.473 \end{bmatrix}$$
Based off of this, I would recommend *Joker* to Sally.

### (d)
Using a similar approach to (d), we can project user-movie space to movie-concept space. Let $$M=\begin{bmatrix}1 & 1 & 1 & 0 & 0 \\ 3 & 3 & 3 & 2 & 0 \\ \vdots & & \ddots & & \vdots \\ 0 & 1 & 0 & 2 & 2 \end{bmatrix}$$ Then, $$MV_{k}=\begin{bmatrix}-1.58 & 0.69  \\ -5.24 & 0.77 \\ -7.22 & 0.84  \\ -7.9 & 3.45 \\ -3.32 & -4.9 \\ -2.75 & -6.45 \\ -1.66 & -2.45\end{bmatrix}$$ Then, we can iterate through all rows to compute their distances from $q=\begin{bmatrix}-0.75 & -1.95\end{bmatrix}$. According to this, Sally is most similar to Rafael. 

```python
import numpy as np

M = np.array([
    [1, 1, 1, 0, 0],
    [3, 3, 3, 2, 0],
    [4, 4, 4, 0, 3],
    [5, 5, 5, 0, 0],
    [0, 2, 0, 4, 4],
    [0, 0, 0, 5, 5],
    [0, 1, 0, 2, 2],
])

V = np.array([
    [-0.51, 0.28],
    [-0.56, 0.13],
    [-0.51, 0.28],
    [-0.25, -0.65],
    [-0.3, -0.64]
])

result = np.dot(M, V)
q = np.array([-0.75, -1.95])

distances = np.linalg.norm(result - q, axis=1)
closest_index = np.argmin(distances)

print(f"Closest row index: {closest_index}")
print(f"Closest row: {result[closest_index]}")
```

---
## Question 3
