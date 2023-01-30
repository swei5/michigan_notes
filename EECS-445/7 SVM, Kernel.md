[[2023-01-30]] #SVM #Kernel #DualFormulation #FeatureMap 

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
$$
K (\overline{u}, \overline{v})=exp(-\gamma ||\overline{u}-\overline{v}||^2)
$$

with $\gamma\ge 0$, and $\gamma$ is a hyperparameter that we tune. This maps data to an **infinite dimensional** feature space.

![[Pasted image 20230130101334.png|400]]

The higher the $\gamma$ value, the more **complicated** $\overline{\theta}$ is.

![[Pasted image 20230130101833.png|500]]


---

### Mercer's Theorem
A function $K:\mathbb{R}^{d}\times \mathbb{R}^{d} \to \mathbb{R}$ is a valid kernel **iff** for any $\overline{x}^i$ and 