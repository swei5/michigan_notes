[[2023-01-11]]

### Recap
Recall the definition of **Binary Linear Classifier**, 
![[2 Linear Classifiers#^b65093]]

and **Training Error**:
![[2 Linear Classifiers#^78fb13]]

---

### Perceptron Algorithm

![[Pasted image 20230110154810.png|500]]

> Remarks:
> 1. Although there might be points that were once correctly classified that are misclassified due to the **update** of $\overline{\theta}$ in the loop, the `while` statement ensures that we never exit the loop until **ALL POINTs** have been correctly classified and the algorithm **converges**.
> 2. The `if` statement always gets executed during the first round since $\overline{\theta}$ is initialized to $\overline{0}$.

If the data are **[[2 Linear Classifiers#^91d7b9|linearly separable]]**, this algorithm finds a **separating [[2 Linear Classifiers#^ba658c |hyperplane]]**.
- Otherwise, we have an **infinite loop**.

#### Example Run-through
- $\overline{\theta}^1=\begin{bmatrix}6\\6\end{bmatrix}$
	![[Pasted image 20230112172003.png|400]]

- $\overline{\theta}^2=\begin{bmatrix}-3\\5\end{bmatrix}$

![[Pasted image 20230112172203.png|400]]
