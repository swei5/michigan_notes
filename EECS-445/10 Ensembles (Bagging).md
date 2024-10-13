[[2023-02-09]] #DecisionTree #Ensemble #LinearClassifier #SupervisedLearning 

### Information Gain
Recall the definition of **conditional entropy** ![[9 Decision Tree#^5c5641]] 
```ad-important
**Definition 10.1**: **Information Gain**

Information gain is defined as
$$IG(X,Y)=H(Y)-H(Y|X)$$

In other words, this denotes how much does **knowing** the value of $X$ **reduce** my **uncertainty** in the variable $Y$.

```

---

### Training Decision Trees: Algorithm
1. Start with an empty tree
2. Split on the best feature
	- Feature with **min** entropy or feature with **max** IG
		- $x^\star=\mathrm{argmax}_{x_s}IG(X_s,Y)=\mathrm{argmax}_{x_s}H(Y)-H(Y|X_s)\equiv \mathrm{argmin}_{x_s}H(Y|X_s)$
1. Recurse until a **stopping criterion** is met

#### Stopping Criteria
1. When all data have the **same** **label** (assuming this is possible)
2. If all data have **identical** **features** (**NO** further splits possible)
3. If all features have **zero** IG
	- Might be short-sighted

#### Pseudo Code
```python
BuildTree(DS)  
	if (y(i) == y) for all examples in DS
		return y  
	elif (x(i) == x) for all examples in DS
		return majority label
	
	else
		x_s = argminx H(y|x) 
		for each value v of x_s
			DS_v = {examples in DS where x_s = v} 
			BuildTree(DS_v)
```

**Stopping criteria #1 **
- When all data have the **same** **label** (assuming this is possible)
```python
if (y(i) == y) for all examples in DS
	return y
```

**Stopping criteria #2 **
- If all data have **identical** **features** (**NO** further splits possible)
```python
elif (x(i) == x) for all examples in DS
		return majority label
```

**Splitting on the BEST feature**
```python
x_s = argminx H(y|x) 
```

---

### Regularization
*Motivation*: We shouldn’t stop growing the tree when we stop seeing reductions in IG.
- Prone to overfitting

#### Methods
1. Set a max depth
	- E.g. $\max(d)=10$
2. Grow full tree then prune
	- Weakest link pruning

A small difference in the training data could lead to a **large difference** in the decision tree output.

---

### Bias Variance Trade-off and Generalization Error
Our goal is to minimize generalization error.
- Generalization error is an error in unseen data that is not estimable

Sources of generalization errors
- Bias (structural error)
- Variance (estimation error)
- Irreducible noise

![[Pasted image 20230211165043.png|400]]

More information on the trade-off between Bias and Variance: [readings (中文)](机器学习中的 Bias（偏差）、Error（误差）、Variance（方差）有什么区别和联系？ - J JR的回答 - 知乎 https://www.zhihu.com/question/27068705/answer/82132134)

---

### Ensemble Methods
- Idea: Create a set of different **models** whose individual **decisions** are **combined** in some way
	- Models: typically **trees** and more commonly classification
	- Main advantage: **Reduces** **variance** without increasing bias
	- Describes a set of approaches that differ in training and combination methods

There are two main types of **ensembles**:
1. Bagging
		- Vanilla bagging
		- Random Forests
2. Boosting (Adaboost)

#### Bagging

```ad-important
**Definition 10.2**: **Bagging**

Bootstrap (vanilla bagging) sampling is defined as to randomly **sample** $n$ data points with **replacement**. Do that $B$ times.
```

![[Pasted image 20230211202954.png|600]]

Bagging simply means **Bootstrap aggregating**.
1. Sample $n$ points $B$ times with replacement
2. Build $B$ decision trees using **each** of the $B$ bootstrap replicates
3. Aggregate their prediction

In mathematical terms, the third step is equivalent to
$$f^\star(\overline{x})=\mathrm{argmax}_{y}\sum\limits_{b=1}^{B}[[f^{b}(\overline{x})==y]]$$

where $y\in\set{0,1}$.

In simple terms, we want to take what is the majority of our decision after aggregating all separate models.

##### Assumptions
- Each **decision tree** has a misclassification rate better than 50%
- Classifiers are **independent**
	- Predictions from the base classifiers are **uncorrelated**

If assumptions are satisfied, as $B\to \infty$, misclassification rate $\to 0$.

---

#### Random Forests
*Aim*: De-correlate the prediction from the **base classifiers**.
1. Bootstrap sampling
2. At each node, best split is chosen from **random subset** of $m<d$ features.