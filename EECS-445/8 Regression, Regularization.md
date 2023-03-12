[[2023-02-01]] #LinearRegression #LossFunction #SGD #Regularization #EmpiricalRisk #FeatureSelection 

### Recap : Loss

Recall the definition of empirical risk ![[3 Perceptron, Loss Functions#^03b62f]]
Similarly, to measure **generalization error**, we take measure performance on **test set**:
$$R_{n^\prime}^{\mathrm{test}}(\overline{\theta})=\frac{1}{n^\prime}\sum_{i=1}^{n^\prime} \ \mathrm{loss}\left(\overline{y}^i\cdot(\overline{\theta}\cdot \overline{x}^i)\right)$$

### Regression

```ad-important
**Definition 8.1**: **Regression**

For supervised learning, we learn to predict $\mathcal{Y}$ from $\mathcal{X}$.
- Labels can be **discrete** or **continuous**
	- Discrete: classification
	- Continuous: regression

Essentially, a regression function $f:\mathbb{R}^{d}\to\mathbb{R}$ where $f \in \mathcal{F}$ (the set of **possible functions**).

```

A linear regression function is simply a **linear function** of the **feature vector**:
$$f(\overline{x};\overline{\theta},b)=\overline{\theta}\cdot\overline{x}+b$$

in response to training set $S_n=\set{\overline{x}^i,\overline{y}^i}^n_{i=1}$ where $\overline{x}\in\mathbb{R}^d$ and $y \in \mathbb{R}$.

---

### Loss Function
For regression, we slightly tweaked the definition of **empirical risk**:
$$R_n(\overline{\theta})=\frac{1}{n}\sum_{i=1}^{n} \ \mathrm{loss}\left(\overline{y}^i-(\overline{\theta}\cdot \overline{x}^i)\right)$$ ^ac935d

where $y^i$ is the **true label** and $\overline{\theta}\cdot \overline{x}^i$  is the **prediction**.

#### Squared Loss
Has the form of
$$\mathrm{Loss}(z)=\frac{z^2}{2}$$

In terms of empirical risk, this is,
$$R_n(\overline{\theta})=\frac{1}{n}\sum_{i=1}^{n} \ \frac{\left(\overline{y}^i-(\overline{\theta}\cdot \overline{x}^i)\right)^2}{2}$$ ^5c42ab

##### SGD with Squared Loss
Recall the SGD algorithm: ![[4 GD, SGD, SVM#^ae3c39]]

The gradient (with respect to $\overline{\theta}$) of the squared loss function is shown below:
![[Pasted image 20230205122818.png|340]]

Thus, the update step in our SGD becomes:
![[Pasted image 20230205123040.png|300]]

### Optimal Value of $\overline{\theta}$ for Empirical Risk
To find the optimal value of $\overline{\theta}$, we need to find the gradient of $R_n(\overline{\theta})$ with respect to $\overline{\theta}$.

Since
$$R_n(\overline{\theta})=\frac{1}{n}\sum_{i=1}^{n} \ \frac{\left(\overline{y}^i-(\overline{\theta}\cdot \overline{x}^i)\right)^2}{2}$$

Taking the gradient and setting it to zero gives
$$
\nabla_{\overline{\theta}}R_n(\overline{\theta})=-\frac{1}{n} \sum\limits_{i=1}^{n} \overline{x}^iy^i+\frac{1}{n} \sum\limits_{i=1}^{n} (\overline{x}^{i}\cdot\overline{\theta})\overline{x}^i=0
$$
$$
\to-\sum\limits_{i=1}^{n} \overline{x}^iy^i+\left(\sum\limits_{i=1}^{n} \overline{x}^i(\overline{x}^i)^T\right)\overline{\theta}=0
$$

Due to the characteristics of dot products.
![[Pasted image 20230205145726.png|600]]

$$\nabla_{\overline{\theta}}R_n(\overline{\theta})\to -X^{T}\overline{y}+X^{T}X \overline{\theta}=0$$
```ad-note
Note that $\dim(\overline{x}^i)$ is $d$. Thus, summing the doct products between $\overline{x}^i$ and $\overline{y}^i$ for all $n$ samples. We can re-write this as $X^{T}\overline{y}$ since

$$X=[\overline{x}^{1},\cdots,\overline{x}^{n}]^{T}\to X^T=[\overline{x}^{1},\cdots,\overline{x}^{n}]$$

where $\overline{x}^i$ is a column vector of size $d$ and $$\overline{y}=[y^1,\cdots,y^n]$$

Finally, note that the matrix product of $X^{T}\overline{y}$ is of size $d\times n \times n \times 1 =d$, which is exactly the sum of the product of our original vectors.

The same reasoning goes for the second part of the summation.
```

```ad-info
**Theorem 8.1**: The closed form of $\overline{\theta}^\star$ for regression with squared loss is

$$\to\overline{\theta}^\star=(X^{T}X)^{-1}X^{T}\overline{y}$$ ^a9289d

A linear algebra proof can be accessed [here](https://pages.pomona.edu/~jsh04747/courses/math158/lr_matrix.pdf).
```

^514acb
Note that we still want to perform SGD just because the above is computationally heavy.

```ad-question
What if $X^TX$ is singular?
- This is possible as some columns are **linearly dependent**.

However, this implies that some features are redundant and signals that we should remove the offending features.
- We could also use regularizations

```

---

### Bias-Variance Tradeoff
Intuitively, to reduce **bias**, we need a larger $\mathcal{F}$. 
- However, if we have noisy/small dataset, this may increase **variance**, which could be attributed to
	- Noisy labels
	- Noisy features

#### Estimation (Variance) and Structural Error (Bias)
**Examples**:
- Low variance $\to$ constant function
- High variance $\to$ high degree polynomial, RBF kernel
- Low bias $\to$ linear regression applied to linear data
- High bias $\to$ constant function model applied to non-linear data

![[Pasted image 20230210162937.png|400]]

---

### Regularization
Recall the definition of L1-regularization and L2-regularization (Embedded Methods): ![[7 SVM, Kernel Trick#^294d65]]

Motivation: we prefer a ***simpler*** hypothesis.
- We want to push parameters towards some **default value** (typically $\textbf{0}$)
- Resists setting parameters away from default value when data weakly tells us otherwise
	- *Bad wording, I think this still means to gravitate parameters towards 0*

Thus, our new objective function can be written as
$$J_{n,\lambda}(\overline{\theta})=\lambda Z(\overline{\theta})+R_n(\overline{\theta})$$

where $\lambda Z(\overline{\theta})$ is the **regularization term** or the **penalty** with $\lambda \ge 0$.

$Z(\overline{\theta})$ should have the following characteristics:
- should **force** components of $\overline{\theta}$ to be small (close to zero)
- Convex, smooth

Some popular choices are $l_p$ norms.

#### Ridge Regression

```ad-important
**Definition 8.2**: **Ridge Regression**

Ridge Regression is regression with L2 regularization with **squared loss function**.
$$
J_{n,\lambda}(\overline{\theta})=\lambda \frac{||\overline{\theta}||^2}{2}+\frac{1}{n}\sum_{i=1}^{n} \ \frac{\left(\overline{y}^i-(\overline{\theta}\cdot \overline{x}^i)\right)^2}{2}
$$
```

```ad-note
When $\lambda=0$
- This is linear regression with squared loss

When $\lambda \to \infty$
- $J$ is minimized at $\overline{\theta} = \mathbf{0}$
	- As in this case the regularization term is the only thing we care about

We will need to pick an appropriate $\lambda$ balances between these two extremes.
```

#### Ridge Regression, Closed Form

^1b0fde

See the notes below:
![[Pasted image 20230210165706.png|600]]

$$\to\overline{\theta}^\star=(\lambda^{\prime}I+X^{T}X)^{-1}X^{T}\overline{y}$$ 
Note that $(\lambda^{\prime}I + X^{T}X)$ is **invertible** as long as $\lambda > 0$.

Note the similarity of this expression with the optimal value of $\overline{\theta}$ without applying the regularization: ![[#^514acb]]

```ad-seealso
Further reading: [Discussion 5 Note](https://umich.instructure.com/courses/583110/files/folder/Discussion%20Notes?preview=29386226)
```
