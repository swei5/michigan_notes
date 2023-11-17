[[2023-01-30]] #SVM #FeatureMap #Regularization #Optimization 

### Recap
Recall our derivations for dual variable $\overline{\alpha}_i$ and support vectors, ![[6 Dual Formulation of SVM#^b23318]]

For **dual SVM** with offset, see [[6 Dual Formulation of SVM#^bfb2e3|derivation here]].

- For hard margin SVMs support vectors can include only
	- Points on the margin
- For soft margin SVMs support vectors can include only
	- Points on the margin, **and**
	- Points on the *wrong side* of the margin
		- Misclassified
		- Points within margins

---

### SVMs and the Kernel Trick

If we apply a feature mapping to our input dataset, the Hard-Margin SVM problem becomes

$$\min_{\overline{\theta}} \frac{||\overline{\theta}||^2}{2}$$

subject to $y^i (\overline{\theta}\cdot \phi (\overline{x}^{i})) \ge 1$ and $\alpha_{i}\ge 0$. Or, we can directly transform it into **dual formulation**: 

![[Pasted image 20230130094105.png|400]]

given by the same set of constraints. This function maps $\mathbb{R}^{p}\to \mathbb{R}$, where $p$ is the dimension of the space we map to.

Note that we can also write this as

![[Pasted image 20230130094334.png|400]]

Sometimes, it's much more efficient to compute $K(\overline{x}^{i}, \overline{x}^{j})$ directly, as this is just mapping from $\mathbb{R}^{d} \to \mathbb{R}$.

```ad-important
$K$ is a measure of similarity between $x^i$ and $x^j$. Note that there exists some valid kernel function,
$$
K(\overline{u}, \overline{v})=\phi(\overline{u})\cdot \phi(\overline{v})
$$

which enables us **NOT** to compute the feature mapping while **mapping data** to a higher dimension, which allows us to find a separating hyperplane.
```


#### Classifying new $\overline{x}$

With the new kernel trick and the fact that we have learned that $\overline{\theta}^{\star}=\sum\limits_{i=1}^{n} \hat{\alpha}_{i}y^ix^i$, then

$$h(\overline{x})=\mathrm{sign} \left(\sum\limits_{i=1}^{n}\alpha_{i}y^{i}\phi(\overline{x}^{i})\right)\cdot \phi(\overline{x}^i)$$
$$
=\mathrm{sign} \left(\sum\limits_{i=1}^{n}\alpha_{i}y^{i}K(\overline{x}^i,\overline{x})\right)
$$

We can thus apply this to get our new classifier in a higher dimension **WITHOUT** explicitly transforming $x^i$ to a higher dimension through $\phi$.

---

### Kernels and Feature Maps
$\phi$ takes input $\overline{x}\in \mathcal{X}$ (e.g. $\mathbb{R}^d$) and maps it to feature space $\mathcal{F}$ (e.g. $\mathbb{R}^p$). Each kernel has an **associated feature mapping**

$$
K(\overline{u}, \overline{v})=\phi(\overline{u})\cdot\phi(\overline{v})
$$

where $K:\mathcal{X}\times \mathcal{X}\to\mathbb{R}$.


#### Kernel Algebra
Let $K_1$ and $K_2$ be valid kernel, then the following are valid
- Sum:  $K_1(\overline{x},\overline{z})+K_2(\overline{x},\overline{z})$ 
- Scalar product: $\alpha K_1(\overline{x},\overline{z})$ 
- Direct product: $K_1(\overline{x},\overline{z})K_2(\overline{x},\overline{z})$

#### Examples of Kernels
- Linear: $K (\overline{u}, \overline{v})=\overline{u}\cdot \overline{v}$
- Quadratic: $K (\overline{u}, \overline{v})=(\overline{u}\cdot \overline{v}+r)^2$ with $r\ge 0$

---

#### Gaussian Kernel (RBF)

^aa7257

$$
K (\overline{u}, \overline{v})=\exp(-\gamma ||\overline{u}-\overline{v}||^2)
$$

^f606f6

with $\gamma\ge 0$, and $\gamma$ is a hyperparameter that  we tune. This maps data to an **infinite dimensional** feature space.

![[Pasted image 20230130101334.png|400]]

The higher the $\gamma$ value, the more **complicated** $\overline{\theta}$ is.

![[Pasted image 20230130101833.png|500]]

```ad-example
Let's take $\gamma=\frac{1}{2}$ without loss and generality.
![[Pasted image 20230205112040.png|500]]
```


---

### Feature Selection
*Motivation*
- When you have few examples and a large number of features ($d\gg n$), it becomes very easy to overfit training data
- How can we remove **uninformative** features?

#### Filter Approach
Rank features according to some metric (independent of learning algorithm).
- Filter out features that fall below a certain threshold

```ad-example
We can create a label (correlation with output) using Pearson's correlation:
	![[Pasted image 20230205102259.png|500]]

And we rank the features according to $r_{x_({1})}$, $r_{x_({2})}$, $\cdots$, $r_{x_({d})}$
```

#### Wrapper Approach
Utilizes learning algorithm to score subsets according to predictive power.
- Learning algorithm is “wrapped” in a search algorithm

![[Pasted image 20230205102609.png|500]]

Pros and cons for **filter approach** and **wrapper approach**

![[Pasted image 20230205102659.png|500]]

#### Embedded Methods
Incorporate **variable selection** as part of the training process.
```ad-important
**Definition 7.1**: **Regularization**

- L2 regularization
$$\min_{\overline{\theta}, b, \overline{\xi}} \frac{||\overline{\theta}||^2}{2}+C\sum\limits_{i=1}^{n}\xi_i$$
 ^556b69
- L1 regularization
$$\min_{\overline{\theta}, b, \overline{\xi}} ||\overline{\theta}||_1+C\sum\limits_{i=1}^{n}\xi_i$$ ^9cec77

such that $y^{i}(\overline{\theta}\cdot x^{i}+b) \ge 1-\xi_{i}\mathrm{\ \ \ \ for\ } i \in \{1,\cdots, n\}, \xi_i\ge0$.
```

^294d65

```ad-note
Note that L1-norm is essentially
$$
||\overline{\theta}||_1=\sum\limits_{i=1}^{d} |\overline{\theta}_i|
$$
```

When $C$ is sufficiently small, the **L1-norm** penalty will **shrink** some parameters to  
exactly zero, which is implicit (or embedded) feature selection.

```ad-seealso
- Readings on [why L1 regularization induces sparsity](https://stats.stackexchange.com/a/45644)
- Readings on [what is the loss function of SVM?](https://stats.stackexchange.com/questions/74499/what-is-the-loss-function-of-hard-margin-svm)
- Readings on [differences between L1 and L2 regularization (中文)](https://zhuanlan.zhihu.com/p/127755008)
```

---

### Mercer's Theorem

```ad-important
**Definition 7.2**: **Mercer's Theorem**

A function $K:\mathbb{R}^{d}\times \mathbb{R}^{d} \to \mathbb{R}$ is a valid kernel **iff** for any $\overline{x}^{i}\in \mathbb{R}^d$ and finite $n$ the $n \times n$ matrix $G$ with $G_{ij}=K(\overline{x}_i,\overline{x}_{j})$ is **positive-semidefinite**.
- That is, $G$ is symmetric: $G=G^T$
- $\forall z \in \mathbb{R}^{n}\ z^TGz\ge0$ 

```

In other words, for such a function $K(\overline{u}, \overline{v})$, there exists a function $\varphi$ such that $K (\overline{u}, \overline{v})=\varphi({\overline{u}})\cdot \varphi({\overline{v}})$.

