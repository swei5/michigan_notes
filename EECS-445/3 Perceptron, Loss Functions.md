[[2023-01-11]] #Perceptron #SupervisedLearning #LinearClassifier #GD #Optimization 

### Recap
Recall the definition of **Binary Linear Classifier**, 
![[2 Linear Classifiers#^b65093]]

and **Training Error**:
![[2 Linear Classifiers#^78fb13]]

---

### Perceptron Algorithm

![[Pasted image 20230110154810.png|500]]

> **Remarks**:
> 1. Although there might be points that were once correctly classified that are misclassified due to the **update** of $\overline{\theta}$ in the loop, the `while` statement ensures that we never exit the loop until **ALL POINTs** have been correctly classified and the algorithm **converges**.
> 2. The `if` statement always gets executed during the first round since $\overline{\theta}$ is initialized to $\overline{0}$.
> 3. The algorithm `return` $\overline{\theta}$.
> 4. The algorithm iterates through all data, then check if all points have been classified correctly; if not, we restart the `for` loop.
> 5. Data **ON** the hyperplane are **misclassified** denoted by `<=`.
> 6. The algorithm could re-mis-classified points.

If the data are **[[2 Linear Classifiers#^91d7b9|linearly separable]]**, this algorithm finds a **separating [[2 Linear Classifiers#^ba658c |hyperplane]]**.
- Otherwise, we have an **infinite loop**.

#### Example Run-through
- $\overline{\theta}^1=\begin{bmatrix}6\\6\end{bmatrix}$

	![[Pasted image 20230112172003.png|400]]

- $\overline{\theta}^2=\begin{bmatrix}-3\\5\end{bmatrix}$

![[Pasted image 20230112172203.png|400]]


```ad-note
1. The algorithm updates based on a **SINGLE** (misclassified) point at a time
	- Very inefficient for large dataset
2. The algorithm *moves* the hyperplane in the *right* direction based on that point. **Proof**:

	Motivation: we want $y^i(\overline{\theta}^{(k+1)}\cdot\overline{x}^i)$ to be *less* negative or even positive. Since
 		$$\overline{\theta}^{(k+1)}=\overline{\theta}^{(k)}+y^i\overline{x}^i$$
 	We have $$y^i(\overline{\theta}^{(k+1)}\cdot\overline{x}^i)=y^i(\overline{\theta}^{(k)}\cdot\overline{x}^i)+||\overline{x}^i||_2^2$$
 	And  $$||\overline{x}^i||_2^2>0$$
 	Unless $\overline{x}^i$ is at $(0,0)$, in which case we must need an **offset**, the above is true. Thus, $$y^i(\overline{\theta}^{(k+1)}\cdot\overline{x}^i)>y^i(\overline{\theta}^{(k)}\cdot\overline{x}^i)$$
 	However, it's not guaranteed that the point will be **correctly classified** after this particular **swing** (see lecture slides for example).
```

We are **NOT** so interested in whether data are **linearly separable**, as those cases would be handled by other algorithms.

---

### Perceptron with Offset

#### Algorithm

![[Pasted image 20230113002054.png|400]]

```ad-note
1. Note that $\overline{x}^{i^{\prime}}=[1,\overline{x}^i]^T$.
2. Note that $\overline{\theta}^{k^{\prime}}=[b^k,\overline{\theta}^k]^T$
```

```ad-important
**Theorem 3.1**: The Perceptron Algorithm **converges** after a finite number of steps when the training examples are linearly separable.
- This is a single layer Neural Network.
```

---

### Non-linearly Separable Data

```ad-question
Idea: Focus on **minimizing** [[2 Linear Classifiers#^52880f|empirical risk]] by finding the parameter $\overline{\theta}$.

Problem: Minimization of this function is NP-Hard.
```

Therefore, we might want to take a new approach.

```ad-important
**Definition 3.1**: Re-definition of **Empirical Risk** under the context of **loss function**:
$$R_n(\overline{\theta})=\frac{1}{n}\sum_{i=1}^{n} \ \mathrm{loss}\left(\overline{y}^i\cdot(\overline{\theta}\cdot \overline{x}^i)\right)$$
```

^03b62f

There are a number of ways to define **loss functions**:

![[Pasted image 20230113004516.png|300]]

for some $z=\overline{y}^i\cdot(\overline{\theta}\cdot \overline{x})$, essentially our classifier without `sign()`.

```ad-note
**Hinge** loss function is convex - **worse** mistakes force predictions to be **more** than just a *little correct*.
- Informally, a convex function is characterized as "*bowl*-shape".
```

---

### Gradient Descent

^74a030

Brief idea: take a small step in the **opposite** direction to which the gradient points. ^4bb007
- Start with a guess, ends at the point where the gradient is 0

