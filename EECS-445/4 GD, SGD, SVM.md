[[2023-01-18]] #SGD #SVM #GradientDescent #SupervisedLearning #LinearClassifier #GD #HardMarginSVM 

### Gradient Descent
Recall the definition of gradient descent: ![[3 Perceptron, Loss Functions#^4bb007]]
```ad-important
**Definition 4.1**: Gradient Descent

Given $S_n=\set{\overline{x}^i,\overline{y}^i}^n_{i=1}$
- Find the value of parameter $\overline{\theta}$ that **minimizes empirical risk** $R_n(\overline{\theta})$.
	- $\frac{1}{n}\sum_{i=1}^{n} \ \mathrm{max}\left(\overline{y}^i\cdot(\overline{\theta}\cdot \overline{x}),0\right)$
```

#### Algorithm

![[Pasted image 20230119142611.png|700]]

Here, `grad(t[k]) * risk(t[k])` can be expressed as the following mathematically,
$$\nabla_\overline{\theta}R_n(\overline{\theta})=\left[\frac{\partial R_n(\overline{\theta})}{\partial \theta_1},\cdots,\frac{\partial R_n(\overline{\theta})}{\partial \theta_d}\right]^T$$

Since 
$$R_n(\overline{\theta})=\frac{1}{n}\sum_{i=1}^{n}  \mathrm{loss}\left(\overline{y}^i\cdot(\overline{\theta}\cdot \overline{x})\right)=\frac{1}{n}\sum_{i=1}^{n}\left(\max(1-y^i(\overline{\theta}\cdot\overline{x}^i),0))\right)$$

Then, 
$$\nabla R_n(\overline{\theta})=\nabla\frac{1}{n}\sum_{i=1}^{n}\left(\max(1-y^i(\overline{\theta}\cdot\overline{x}^i),0))\right)=\frac{1}{n}\sum\limits_{i=1}^{n}-y^i\overline{x}^i.$$ ^7ade40

```ad-warning
We have to divide the entire expression by $n$ still, even though **NOT ALL** $n$ points are mis-classified or has a loss.
```

---

#### Convergence

```ad-important
**Definition 4.2**: The convergence criteria, or stopping criteria, is often defined according to:
- Some max number of iterations is reached
- When there's only **negligible** change in $\overline{\theta}$
- When there's only **negligible** change in $R_n(\overline{\theta}^{(k)})$
```

#### Learning Rate
$\eta$ is known as the **learning rate** of gradient descent. 
- If $\eta$ is too large, it might “overshoot” the global minimum, and the algorithm will **NOT** converge
- If $\eta$ is too small, it takes a **LONG** time to reach convergence.
	- If learning rate needs to be **constant**, we can use **cross-validation** and select the learning rate (a  hyperparameter) that performs well **on average** for each of the validation folds.
	- Can also make the step size a function of $k$:
		- $\eta_k=\frac{1}{k+1}$ - this function ensures our **stochastic sub-gradient descent** algorithm converges to the minimum.

In fact, any **positive** learning rate that satisfies $\sum\limits_{k=0}^{\infty} \eta_k^2<\infty$ and $\sum\limits_{k=0}^{\infty} \eta_k=\infty$ would permit the algorithm to converge, though the rate may vary.

```ad-warning
Gradient Descent with a large dataset is extremely time-consuming as computing $R_n(\overline{\theta})$ requires us to look at all datapoints in the set.

Recall the definition of **empirical risk** ![[3 Perceptron, Loss Functions#^03b62f]]
```

---

### Stochastic Gradient Descent
Since this can be computationally expensive, a slight variation of (batch) Gradient Descent known as **Stochastic Gradient Descent** (SGD) is more commonly used.

#### Algorithm
The algorithm is shown as the following:
![[Pasted image 20230119141956.png|700]] ^ae3c39

```ad-example
Here, we use the **[[3 Perceptron, Loss Functions#^e177f0|hinge loss]]** function:
$$\mathrm{loss}(y^i(\overline{\theta}\cdot\overline{x}^i))=\max(1-y^i(\overline{\theta}\cdot\overline{x}^i),0))$$

Note that there are two cases for this $\mathrm{loss}$ function.
- Case 1: $y^i(\overline{\theta}\cdot\overline{x}^i)>1$
	- Then, $\mathrm{loss}$ is 0
	- Gradient is $\overline{0}$
	- **NO** update is made
- Case 2: $y^i(\overline{\theta}\cdot\overline{x}^i)<1$
	- $\nabla(^i(\overline{\theta}\cdot\overline{x}^i))=-y^ix^i$
	- Note how this relates to the gradient of a loss function (hinge): [[3 Perceptron, Loss Functions#^e177f0]]
```

The algorithm is therefore equivalent to

![[Pasted image 20230119180926.png|500]]

```ad-note
Note the similarity between the above and #Perceptron algorithm.
- Not exactly the same as we modify $\overline{\theta}$ in SGD when the **loss function** $>0$, i.e. $y^i(\overline{\theta}\cdot\overline{x}^i)<1$, instead of $y^i(\overline{\theta}\cdot\overline{x}^i)<=0$.
	- This criterion **ALSO** selects points that are correctly classified but to a *slight* degree
```


##### Difference GD and SGD
- In each iteration
	- GD considers **ALL** examples, requires more computation - result is *true* gradient
	- SGD only considers **ONE** example, i.e. $y^{t}, \overline{x}^t$ - result is *approximate* gradient

- The empirical risk, $R_n\left(\overline{\theta}^{(k)}\right)$, for
	- GD is **monotonically decreasing** by definition
	- SGD decreases in general, but **fluctuates** in the process

![[Pasted image 20230119143058.png|600]]

```ad-note
In practice, we use **mini-batch stochastic gradient descent**, i.e. computing the gradient based on **a subset of the entire training set**. It is faster than batch GD, while giving a more accurate approximation of the true gradient than SGD.
```

In SGD, it is often quite beneficial to keep track of the **best solution** obtained so far in addition to the current $\overline{\theta}^{(k)}$. In other words, we would also keep $\overline{\theta}^{(i_k)}$, where $i_{k}=\mathrm{arg \min}_{i=1,\cdots,k}R_n(\overline{\theta}^{(i)})$. 
- The **empirical risk** of the **current solution** $\overline{\theta}^{(k)}$, $R_n(\overline{\theta}^{(k)})$, goes down in a **noisy** manner while $R_n(\overline{\theta}^{(i_k)})$ is **monotonically decreasing** by definition

If the algorithm is stopped at any point, we should report $\overline{\theta}^{(i_k)}$ (the best solution so far) rather than $\overline{\theta}^{(k)}$.

```ad-warning
Note that $\lim_{k\to\infty}R_n(\overline{\theta}^{(i_k)})$ is not necessarily $0$ as the points may not be **linearly separable**.
```

![[Pasted image 20230119143830.png|600]]

---

#### Convergence


---

### Hard Margin SVM
We want to have
- Boundary that classifies the training set **correctly**
- That is **maximally removed** from training examples closest to the decision boundary

This is an **optimization** problem with **constraints**.

```ad-important
**Definition 4.3**: Hard-margin Support Vector Machines (SVMs) maximize the distance between the **decision boundary** and points that are **closest** to the decision boundary.

$$\max_{\overline{\theta}, b} \min_{i} \gamma^{i}(\overline
\theta, b) \mathrm{\ \ \ \ subject\ to\ \ \ \ } y^{i}(\overline{\theta}\cdot x^{i}+b) \ge 1$$

```

^f87ed1

![[Pasted image 20230123093702.png|400]]

Here, $\gamma^i$ is defined to be the **distance** between the datapoint $x^i$ and the hyperplane.

```ad-note
$\gamma^i$ can be computed as follow:
$$\frac{y^{i}(\overline{\theta}\cdot x^{i}+b)}{||\overline{\theta}||}.$$

**Proof**:
$$d=\frac{\overline{\theta}\cdot(x^i-x_0)}{||\overline{\theta}||}$$

where $x_0$ is a point on the hyperplane $H$. Then,
$$=\frac{\overline{\theta}\cdot x^{i}- \overline{\theta}\cdot x^{0}}{||\overline{\theta}||}=\frac{\overline{\theta}\cdot x^{i}+b}{||\overline{\theta}||}$$

as $\overline{\theta}\cdot x^{0}+b = 0$ as defined by the decision boundary. We then multiply it by $y^{i}\in \set{1,-1}$ to account for the direction.
```

^def8e8

The points that are closest to our decision boundary in this case is called **support vectors** - they lie on the **support margin**. 

```ad-note
We can in fact simplify the general formulae to Hard-Margin SVM.

This is because 

$$\min \gamma^{i}(\overline{\theta}, b) = \min \frac{y^{i}(\overline{\theta}\cdot x^{i}+b)}{||\overline{\theta}||}$$

and we had set the constraint that $y^{i}(\overline{\theta}\cdot x^{i}+b) \ge 1$.
- If $\min (\overline{\theta}\cdot x^{i}+b)$ is not necessarily $1$, we **normalize**

Therefore, we are essentially trying to get at

$$\max_{\overline{\theta}} \frac{1}{||\overline{\theta}||}$$

which is equivalent to
$$\min_{\overline{\theta}} \frac{||\overline{\theta}^2||}{2}.$$
```

^ec1126

