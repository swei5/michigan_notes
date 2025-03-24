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