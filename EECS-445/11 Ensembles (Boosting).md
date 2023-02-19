[[2023-02-13]] #Ensemble #Boosting #DecisionTree 

### Boosting
Combining simple **weak classifiers** into a more complex/strong classifier.
- Decreases **bias** (structural error) and **variance** (estimation error)
- **Sequentially** build classifiers, each one improves on the previous classifiers

A classifier a general form of
$$h_M(\overline{x})=\sum\limits_{m=1}^{M}\alpha_{m}h(\overline{x}^{i};\overline{\theta}_{m})$$

- Different boosting algorithms differ on loss function, and how to pick $\alpha$

---

### AdaBoost
A **weak classifier** is said to be a *decision stump* - a single split (short) tree.
- There can only be $n+1$ binary stumps (one at a time)
- Misclassification rate $\le 50\%$

```ad-example
- First and second iteration

	![[Pasted image 20230213093027.png|300]]

After the first iteration, we assign heavier weights on the three **misclassified points** in the data.

Note that for the second iteration, despite the same number of points are misclassified, $\epsilon_{1}\ne \epsilon_{2}$

Also, note that $\alpha_{m}$ is inversely proportional to $\epsilon_{m}$.

- Eventually, $h_M(\overline{x})=$

	![[Pasted image 20230213093409.png|400]]

```

### AdaBoost: Details
We use the **exponential loss**:
$$\mathrm{Loss}_{\mathrm{exp}}(z)=\exp(-z)$$

Consequently, AdaBoost aims to minimize:
$$\sum\limits_{i=1}^{n}\exp(-y^{i}h_{M}(\overline{x}^i))$$

```ad-example
We have a classifier $h(\overline{x};\overline{\theta})=\mathrm{sign}(\theta_{1}(x_{d}-\theta_{0}))$, where
$$\overline{\theta}=[d,\theta_{0},\theta_{1}]^T$$

Here,
- $d$ is the index of the variable we **split** on
- $\theta_0$ is the value of the variable we **split** on
- $\theta_1$ is the direction we split on

Example: try $\mathbf{x}=[6,0]^T$.

```


#### Algorithm
![[Pasted image 20230213100642.png|500]]

```ad-note
**Remarks**:
- Step 2 a: find the $\overline{\theta}^{\star}_m$ that **minimizes** the loss, given the function
$$\sum\limits_{i=1}^{n} \tilde{w}^{i}_{m-1}[[y^{i}\ne h(\overline{x}^{i}; \overline{\theta})]]$$

- Step 2 b: given $\overline{\theta}_m$, we compute the **misclassified error**. It's guaranteed that $0 \le \hat{\epsilon} \le1$.

- Step 2 c: the natural logarithm is a direct consequence of the fact that Adaboost minimizes exponential loss.

- Step 2 d: note that $h_m(\overline{x})=\sum\limits_{j=1}^{m} \alpha_{j}h(\overline{x}_{j}; \overline{\theta}_j)$
```


```ad-note
- $\tilde{w}$ means that the weights are normalized to sum up to $1$. 
	- Thus, step 2-b sums up to $1$  at most (given every point is misclassified)
- $Z'_{m}$ is a term to ensure normalization holds (sum of weights add up to 1)
```

#### Properties
![[Pasted image 20230213100828.png|400]]

- Red: test error
- Blue: training error
- Iterations: the number of **weak classifier**