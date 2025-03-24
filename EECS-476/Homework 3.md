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