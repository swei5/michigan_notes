[[2023-02-01]] #LinearClassifier #LinearRegression #LossFunction #SGD #Regularization #EmpiricalRisk 

### Recap : Loss

Recall the definition of empirical risk ![[3 Perceptron, Loss Functions#^03b62f]]
Similarly, to measure **generalization error**, we take measure performance on **test set**:
$$R_{n^\prime}^{\mathrm{test}}(\overline{\theta})=\frac{1}{n^\prime}\sum_{i=1}^{n^\prime} \ \mathrm{loss}\left(\overline{y}^i\cdot(\overline{\theta}\cdot \overline{x}^i)\right)$$

### Regression
For supervised learning, we learn to predict $\mathcal{Y}$ from $\mathcal{X}$.
- Labels can be **discrete** or **continuous**
	- Discrete: classification
	- Continuous: regression

Essentially, a regression function $f:\mathbb{R}^{d}\to\mathbb{R}$ where $f \in \mathcal{F}$ (the set of **possible functions**).

A linear regression function is simply a **linear function** of the **feature vector**:
$$f(\overline{x};\overline{\theta},b)=\overline{\theta}\cdot\overline{x}+b$$

in response to training set $S_n=\set{\overline{x}^i,\overline{y}^i}^n_{i=1}$ where $\overline{x}\in\mathbb{R}^d$ and $y \in \mathbb{R}$.

---

### Loss Function
For regression, we slightly tweaked the definition of **empirical risk**:
$$R_n(\overline{\theta})=\frac{1}{n}\sum_{i=1}^{n} \ \mathrm{loss}\left(\overline{y}^i-(\overline{\theta}\cdot \overline{x}^i)\right)$$

where $y^i$ is the **true label** and $\overline{\theta}\cdot \overline{x}^i$  is the **prediction**.

#### Squared Loss
Has the form of
$$\mathrm{Loss}(z)=\frac{z^2}{2}$$

In terms of empirical risk, this is,
$$R_n(\overline{\theta})=\frac{1}{n}\sum_{i=1}^{n} \ \frac{\left(\overline{y}^i-(\overline{\theta}\cdot \overline{x}^i)\right)^2}{2}$$

##### SGD with Squared Loss
Recall the SGD algorithm: ![[4 GD, SGD, SVM#^ae3c39]]

The gradient (with respect to $\overline{\theta}$) of the squared loss function is shown below:
![[Pasted image 20230205122818.png|340]]

Thus, the update step in our SGD becomes:
![[Pasted image 20230205123040.png|300]]

### Optimal Value of $\overline{\theta}$ for Empirical Risk
To find the optimal value of $\overline{\theta}$, we need to find the gradient of $R_n(\overline{\theta})$ with respect to $\overline{\theta}$.