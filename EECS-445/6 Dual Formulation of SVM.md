[[2023-01-25]] #SVM #SoftMarginSVM #FeatureMap #DualFormulation #PrimalFormulation

### Recap

Recall what a Soft-Margin SVM is: ![[5 Soft-Margin SVM#^accc27]]

Our goal here is to find the **max** **margin separator**.
- Intuition: the margin has length $\frac{1}{||\overline{\theta}||}$ , and we would want the magnitude of $\overline{\theta}$ to shrink as much as possible.

Soft-Margin SVM does some amazing things:
1. Handle data that are **NOT** linearly separable
2. Reduce effect of outliers/overfitting

```ad-important
Soft margin SVM is an optimization problem with the following objective function (with **hinge loss**): ![[3 Perceptron, Loss Functions#^03b62f]]

and $l_2$-norm regularizer.
```

---

### Feature Mapping
Another way to handle data that are **NOT** linearly separable is by mapping.

We can map data to a **higher dimension space** in which there exists a **separating hyperplane** (corresponds to a non-linear decision boundary in the original space).

$$S_n=\{\overline{x}^{i},\overline{y}^{i}\}_{i=1}^{n}\to S_n^\prime=\{\phi(\overline{x}^i), \overline{y}^i\}_{i=1}^{n}$$

In which $\phi$ is a feature map s.t. $\phi:\mathbb{R}^d\to\mathbb{R}^p$. 

![[Pasted image 20230125232653.png|500]]

In the example above, we map data in $\mathbb{R}^2$ to $\mathbb{R}^3$ using the mapping function.

```ad-example
If we have a mapping function $\phi(\overline{x})=[x_1^2,x_2^2,x_1,x_{2,}1]^T$ and one data point $\overline{x}=[2,2]^T$, then
$$\phi(\overline{x})=[4,4,2,2,1]^T$$
as $\overline{x}$ is mapped from $\mathbb{R}^2$ to $\mathbb{R}^5$.

Note that for the linear classifier to make sense, $\overline{\theta}$ needs to be in the same dimension after the transformation. Here, $\overline{\theta}=[1,1,-4,-4,7]^T$, and our new classifeir $\mathrm{sign}(\overline{\theta}\cdot\phi(\overline{x}))$ separates the data.
```

This implies, however, our parameter is now learnt in $\mathbb{R}^p$ space, which can be potentially very inefficient. 

---

### Dual Formulation
Consider a Hard-Margin SVM problem with no offset parameter: ![[4 GD, SGD, SVM#^ec1126]]
Here, we want to re-write the solution in **dual form**:

![[Pasted image 20230125234256.png|400]]

The output of this optimization problem is $\hat{\overline{\alpha}}$:
$$
\overline{\theta}^{\star}=\sum\limits_{i=1}^{n} \hat{\alpha}_{i}y^ix^i
$$

Suppose we want to map to a higher dimensional space, using dual form to solve, this is equivalent to

![[Pasted image 20230125234601.png|400]]

As sometimes, $\phi (\overline{x}^i)\cdot\phi (\overline{x}^j)$ can be computed **MUCH MORE efficiently** than separately computing them.

Before that, we can abstract our original QP function into $f(\overline{w})$.

---

#### Primal Formulation

```ad-important
- Original problem: $\min_{\overline{w}} {f(\overline{w})}$ s.t. $h_i(\overline{w})\le0$
- Lagrangian: $L(\overline{w}, \overline{\alpha})=f(\overline{w})+\sum\limits_{i=1}^{n} \alpha_{i}h_{i}(\overline{w})$ given that $\alpha_{i}\ge 0$
	- Here, $\overline{w}$ is known as the **primal variable** and $\alpha$ is known as the **dual variable**

We define $g_p(\overline{w}) = \max_{\overline{\alpha}, \alpha_{i}\ge 0}{L(\overline{w}, \overline{\alpha})}$
- This means $g$ is only a function $\overline{w}$ and no longer of $\overline{\alpha}$.
	- $\alpha_i$ is a choice
```

Then, we have two cases:
- Case 1: Constraints are satisfied: $h_i (\overline{w})\le 0\ \ \forall i$.
	- $g_p (\overline{w})=\max_{\overline{\alpha}}\left((f(\overline{w})+\sum\limits_{i=1}^{n}\alpha_{i}h_{i}(\overline{w}))\right)$
	- Since, however, $h_i (\overline{w})\le 0\ \ \forall i$ and $\alpha_{i}\ge0$, the maximal value of $g_p$ occurs when $h_i (\overline{w})= 0\ \ \forall i$.
		- Hence, $g_p(\overline{w})=f(\overline{w})$

- Case 2: Suppose the $j$ -th  constraint is violated, meaning that $h_j (\overline{w})>0$.
	- Then, there is no upper bound as we can set $\alpha_j$ to infinity in order to maximize the function
		- Hence, $g_p (\overline{w})=\infty$, without loss of generality

```ad-note
Note that $\min_{\overline{w}}g_p(\overline{w})=\min_{\overline{w}} \left(\max_{\overline{\alpha}, \alpha_{i}\ge 0} L (\overline{w},\overline{\alpha})\right)=min_{\overline{w}}f(\overline{w})$ if constraints are satisfied.

If constraints are **NOT** satisfied, it is futile to find the min from infinity.
```

```ad-important
**Definition 6.1**: $$
\min_{\overline{w}}\max_{\overline{\alpha}, \alpha_{i}\ge 0} L (\overline{w},\overline{\alpha})$$ 

is called the **primal formulation**.

Its opposite, $$
\max_{\overline{\alpha}, \alpha_{i}\ge 0}\min_{\overline{w}}L (\overline{w},\overline{\alpha})$$

is called the **dual formulation**.
```

#### Duality Gap
The difference between primal formulation and dual formulation is called the **duality gap**.
- The **dual** gives a **lower bound** on the solution of the **primal**
- Under certain conditions, the gap is **ZERO**
	- For quadratic convex objective, constraint functions affine, primal/dual feasible


#### SVM Dual Formulation
Using the methodology above, we can derive the dual formulation for SVM.

```ad-example
**Proof**: The Lagrangian can be written as
$$
L (\overline{w},\overline{\alpha}) = \frac{||\overline{\theta}||^{2}}{2} +\sum\limits_{i=1}^{n}\alpha_{i}(1-y^{i}(\overline{\theta}\cdot\overline{x}^{i}))
$$

such that $\alpha_{i}\ge 0$ and where
$$
h_i(\overline{w}):=1-y^{i}\overline{\theta}\cdot \overline{x}^{i}\le 0.
$$

To find the minimum of $L$, we take the gradient and set it to zero. We get
$$
\overline{\theta}-\nabla_{\overline{\theta}} \sum\limits_{i=1}^{n} \alpha_{i}y^{i}\overline{\theta}\cdot \overline{x}^{i}=\overline{\theta}- \sum\limits _{i=1}^{n} \alpha_{i}y^{i}\cdot \overline{x}^{i}
$$

Hence, $$ \overline{\theta}^{\star}= \sum\limits_{i=1}^{n} \alpha_{i}y^{i}\cdot \overline{x}^{i} $$

like what we saw before.

Then, we can now rewrite everything in **dual formulation**:
$$
\max_{\overline{\alpha}, \alpha_{i}\ge 0}\min_{\overline{w}}L (\overline{w},\overline{\alpha}) = \max_{\overline{\alpha}, \alpha_{i}\ge 0} \left(\frac{||\overline{\theta}||^{2}}{2} +\sum\limits_{i=1}^{n}\alpha_{i}(1-y^{i}(\overline{\theta}\cdot\overline{x}^{i}))\right)
$$

$$
= \max_{\overline{\alpha}, \alpha_{i}\ge 0} \frac{\left(\sum\limits_{i=1}^{n} \alpha_{i}y^{i}\cdot \overline{x}^{i}\right)\cdot \left(\sum\limits_{i=1}^{n} \alpha_{i}y^{i}\cdot \overline{x}^{i}\right)}{2} + \sum\limits_{i=1}^{n} \left(1-y^{i}\left(\sum\limits_{i=1}^{n} \alpha_{i}y^{i}\cdot \overline{x}^{i} \right)\cdot \overline{x}^i\right)
$$
```

Let optimal values be given by $\overline{\theta}^{\star}$ and we substitute that in our original equation, we get

![[Pasted image 20230126182455.png]]

Note that $b$ eventually evaluates to zero
