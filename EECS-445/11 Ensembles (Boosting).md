[[2023-02-13]] #Ensemble #DecisionTree #LinearClassifier #SupervisedLearning 

### Boosting

```ad-important
**Definition 11.1**: Boosting

Combining simple **weak classifiers** into a more complex/strong classifier.
- Decreases **bias** (structural error) and **variance** (estimation error)
- **Sequentially** build classifiers, each one improves on the previous classifiers
```

A classifier a general form of
$$h_M(\overline{x})=\sum\limits_{m=1}^{M}\alpha_{m}h(\overline{x}^{i};\overline{\theta}_{m})$$ ^c54656

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

^06a470


#### Algorithm

^705f6d

![[Pasted image 20230213100642.png|500]] ^367ada

Note that an **alternate expression** for Step 2 d is
$$\tilde{w}^{i}_{m}=\frac{\exp(-y^{i}h_{m}(\overline{x}^i))}{Z^\prime_{m}}$$ ^240105

```ad-note
**Remarks**:
- Step 2 a: find the $\overline{\theta}^{\star}_m$ that **minimizes** the loss, given the function
$$\sum\limits_{i=1}^{n} \tilde{w}^{i}_{m-1}[[y^{i}\ne h(\overline{x}^{i}; \overline{\theta})]]$$

- Step 2 b: given $\overline{\theta}_m$, we compute the **misclassified error**. It's guaranteed that $0 \le \hat{\epsilon} \le1$.

- Step 2 c: calculate the **weighted misclassified rate**. The natural logarithm is a direct consequence of the fact that Adaboost minimizes exponential loss. $\alpha_{m} \ge 0$ because of step 2b.

- Step 2 d: note that $h_m(\overline{x})=\sum\limits_{j=1}^{m} \alpha_{j}h(\overline{x}^i; \overline{\theta}_j)$.
	- If $\overline{x}^i$ is correctly classified, $\tilde{w}^{i}_{m}=\tilde{w}^{i}_{m-1} \exp(-\alpha_m)$ such that $\exp({-\alpha_{m}}) < 1$
	- If $\overline{x}^i$ is misclassified, $\tilde{w}^{i}_{m}=\tilde{w}^{i}_{m-1} \exp(\alpha_m)$ such that $\exp({\alpha_{m}}) > 1$

This explains why **weights** of misclassified points are larger, and correctly classified points are smaller.

- $Z_{m}= \sum\limits_{i=1}^{n} \tilde{w}_{m-1}^{i} \exp(-y^{i}\alpha_{m}h(\overline{x}^{i};\overline{\theta}_{m}))$
```

```ad-important
- $\tilde{w}$ means that the weights are normalized to sum up to $1$. 
	- Thus, step 2-b sums up to $1$  at most (given every point is misclassified)
- $Z'_{m}$ is a term to ensure normalization holds (sum of weights add up to 1)
- Note that for the **update step** (2.d), update is the same for **ALL** correctly classified points, and the same for **ALL** incorrectly classified points.
```

#### Properties
![[Pasted image 20230213100828.png|400]] ^7a5bd6

- Red: test error
- Blue: training error
- Iterations: the number of **weak classifier**