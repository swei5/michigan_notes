**EECS 476 Project 2**
Vincent Wei ([vswei@umich.edu](mailto:vswei@umich.edu))
March 31, 2025 

## Part 1
### Question 1
#### (c) (i)
Singular values: $13.2355$, $9.5923$, $2.3943$, $1.9972$, $1.0428$

#### (c) (ii)
Eigenvalues: $1.0873$, $3.9886$, $5.7329$, $92.0114$, $175.1798$

#### (c) (iii)
The singular values of matrix $A$ are the square roots of the eigenvalues of $A^{T}A$. For instance, $13.2355^{2} \approx 175.1798$, $9.5923^{2} \approx 92.0114$, etc..

#### (d)
The columns of $V$ are the eigenvectors of $A^{T}A$ (the output matrices are the same from both programs). Given SVD: $$A=U\Sigma V^{T}$$ then, $$\begin{align}
A^{T}A &= (U\Sigma V^{T})^{T}(U\Sigma V^{T}) \\
&= V\Sigma^{T}U^{T}U\Sigma V^{T}\\
&= V\Sigma^{2}V^{T} &(U^{T}U=I, \Sigma \text{ is a diagonal matrix})
\end{align}$$ which directly follows that $$A^{T}AV=V\Sigma^{2}$$ since $V^{T}V=I$. Thus, we have shown that the columns of $V$ are the eigenvectors of $A^{T}A$ and $\Sigma^{2}$, or the squared values of the singular values of $A$ are the eigenvalues of $A^{T}A$.

### Question 2
#### (b)
I kept 13 components. 

![[grayscale_image.jpg|300]] 

![[matrix.png|400]]

#### (c)
As the number of components increase, the reconstructed image looks more and more similar to the original image. We don't see a significant improvement, however, after `components=13`, confirming our hypothesis in the previous question that `energy=0.9` is a good benchmark for dimensionality reduction.

![[output_1.png|300]]
![[output_7.png|300]]
![[output_13.png|300]]
![[output_19.png|300]]
![[output_25.png|300]]
![[output_31.png|300]]

#### (d)
$18733$. This is the sum of the size of $U$, $\Sigma$, and $V^{T}$.

#### (e)
$518400$. This is the size of the original matrix.

---
## Part 2
### Question 1
$$\begin{align}
\epsilon_{iu}&=\frac{\partial E}{\partial r_{iu}}\\
&= \frac{\partial}{\partial r_{iu}} (r_{iu}-q_{i}\cdot p^{T}_{u})^{2}\\
&= 2(r_{iu}-q_{i}\cdot p^{T}_{u})
\end{align}$$

### Question 2
$$\begin{align}
\frac{\partial E}{\partial q_{i}}&= 2(r_{iu}-q_{i}\cdot p^{T}_{u})(-p_{u})+2\lambda q_{i}\\
&= -2\epsilon_{iu} p_{u}+2\lambda q_{i}
\end{align}$$
Similarly, $$\begin{align}
\frac{\partial E}{\partial p_{u}}&= 2(r_{iu}-q_{i}\cdot p^{T}_{u})(-q_{i})+2\lambda p_{u}\\
&= -2\epsilon_{iu} q_{i}+2\lambda p_{u}
\end{align}$$ Hence, the update rules are: $$\begin{align}
q_{i} &\leftarrow q_{i}+2\mu(\epsilon_{iu} p_{u}-\lambda q_{i})\\
p_{u} &\leftarrow p_{u}+2\mu(\epsilon_{iu} q_{i}-\lambda p_{u})
\end{align}$$