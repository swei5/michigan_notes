[[2023-02-06]] #DecisionTree #Bias #Variance #Entropy

### Recap
- Linear models, regression
	- Loss: ![[8 Regression, Regularization#^5c42ab]]
	- Optimization method
		- Gradient descent, [[8 Regression, Regularization#^1b0fde|closed form]].
- Linear models, classification
	- Loss (optimization problem): ![[5 Soft-Margin SVM#^accc27]]
	- We want to find $\overline{\theta}^\star$ that minimizes the loss
- Non-linearly separable datasets
	- Feature mapping
	- Kernel trick
		- **Non-linear** transformation of the feature space allows us to separate the data
		- Avoid explicitly computing feature mappings

So far we have focused on accuracy, but we also want **interpretability**.
- Decision trees are non-linear and interpretable

---

### Decision Tree

```ad-important
**Definition 9.1**: **Decision Tree**

$f: \mathcal{X} \to \mathcal{Y}$. It is non-linear and contains axis-aligned **partitions**.
- Axis-aligned means **parallel** to the $x, y$ axis
- In addition, decision trees can also handle **binary input** (yes/no) or **categorical input** (type) at the same time
```

There are two main types of trees:
- Classification trees, where $y\in\{0,1\}$
- Regression trees, where $y\in\mathbb{R}$

```ad-example
A classification tree: Predict if a patient is healthy or not ($y$) using their age and systolic blood pressure ($\overline{x}$)

![[Pasted image 20230210172617.png|600]]
```

#### Structure
- Internal nodes
	- Defined by **dimension** **index** $j$ and **split value** $s$
	- Has 2 child nodes: either **internal** **nodes** or **leaves**
- Leaf nodes
	- Assign a label (discrete/continuous)
- Path
	- The nodes “traversed” by a group of data points

#### Classification Rules
1. Follow the **path** for new test $\overline{x}$
2. Get **majority label** from training data

In mathematical terms, this is
$$f(\overline{x})=\mu_1[[\overline{x}\in R_1]]+\cdots \mu_n[[\overline{x}\in R_n]]=\sum\limits_{m=1}^{M} \mu_{m}[[\overline{x}\in R_m]]$$
for a training model of $M$ partitions, where $[[\star]]$ is an indicator function that returns $0$ or $1$ and $\mu_i$ returns the **majority label** in region $R_i$. ^213987

```ad-example
Using the example above, for an arbitrary $\overline{x}\in R_2$,
$$f(\overline{x})=0(0)+1(1)+1(0)+0(0)=1.$$
```

---

#### Prediction Rules
For regression trees, instead of choosing the **majority label**, we choose the **average label** for data points falling in leaf.

```ad-example
![[Pasted image 20230210174123.png|550]]
```

#### Characteristics of a Good Tree
- Simplest (smallest) accurate decision tree
- Accurate

Since we can't define our parameters in advance (thus no training loss found), we might want to try **all possible trees** and pick the best.
- Brute force

---

### Training Decision Trees: Intro
- Brute force
	- Number of possible trees grows **exponentially** with the **dimension** of the features and # of distinct values
		- $d!$ possible ways to order $d$ possible tests
		- NP-Complete problem
- Greedy
	- Greedy approach to picking the best feature to split on
	- Want our splits to **minimize** **uncertainty** in the label
		- How?

#### Shannon Entropy

```ad-important
**Definition 9.2**: **Entropy**

A measure of the amount of “**uncertainty**” in a variable. In mathematical terms, for $y \in [y_{1}, \cdots, y_{n}]$:
$$H(Y)=-\sum\limits_{i=1}^{n}p(y_i)\log_{2}(p(y_i))$$
where $p(y_i)$ is the proportion of $y$ that takes on value of $y_i$.
```

For instance, if $y\in \set{0,1}$:
$$H(Y)=-p(Y=1)\log_2p(Y=1)-p(Y=0)\log_2p(Y=0)$$
![[Pasted image 20230210183716.png|400]]

#### Entropy of $Y$ conditioned on $X$
The entropy of $Y$ conditioned on $X=x$ is
$$H(Y|X=x)=-\sum\limits_{y}p(Y=y|X=x) \log_{2}p(Y=y|X=x)$$

It indicates the **uncertainty** in $Y$ after we learn the value of $X$.

The conditional entropy is defined as
$$H(Y|X)=\sum\limits_{x}p(X=x)H(Y|X=x)$$ ^5c5641
```ad-warning
Note the difference!
```

```ad-example
For probability chart that looks like the below:
![[Pasted image 20230211120112.png|300]]

The entropy of $Y$ given that $x_1$ is cloudy is
$$H(Y|X_1=c)=-p(Y=0|X_1=c)\log_{2}p(Y=0|X_1=c)-p(Y=1|X_1=c)\log_{2}p(Y=1|X_1=c)$$

$$=-\frac{2}{3}\log_{2}\frac{2}{3}-\frac{1}{3}\log_{2}\frac{1}{3}$$

Whereas,
$$H(Y|X_1)=p(X_1=s)H(Y|X_1=s)+p(X_1=c)H(Y|X_1=c)$$
$$=\frac{2}{5}(-\frac{1}{2}\log_{2}\frac{1}{2}+\frac{1}{2}\log_{2}\frac{1}{2})+\frac{3}{5}(-\frac{2}{3}\log_{2}\frac{2}{3}-\frac{1}{3}\log_{2}\frac{1}{3}).$$
```


