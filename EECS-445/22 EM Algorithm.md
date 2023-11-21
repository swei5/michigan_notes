[[2023-04-05]] #MLE #Probability #UnsupervisedLearning 

### Generative Model
Recall in [[19 Collaborative Filtering]] we briefly mention the concept of **generative model**: ![[19 Collaborative Filtering#^8b1286]]
We can apply similar ideas to probabilities and distributions. We can assume data are **generated i.i.d.** from an **unknown distribution** that has parameter $\overline{\theta}$.

So far, we have covered a few types of distributions, each corresponding to different dimensions.
- Binary data - Bernoulli
- Continuous data from $\mathbb{R}$ - [[20 MLE, Gaussians#^625fb3|Univariate Gaussian]]
- Continuous data from $\mathbb{R}^d$ - [[20 MLE, Gaussians#^1fcfef|Spherical Gaussian]], [[21 Gaussian Mixture Models#^f1277b|Gaussian Mixture]]

---

### Probability Basics

```ad-important
**Theorem 22.1**: Probability Rules

1. Product Rule $$p(X,Y)=p(X|Y)p(Y)$$
2. Sum Rule $$p(X)=\sum\limits_{Y} p(X,Y)$$
```

This is the reason we get $$\begin{align}p(S_{n})&=p(\overline{x}^{(i)},\overline{y}^{(i)})\\
&=\prod_{i=1}^{n} p(\overline{x}^{(i)}|y^{(i)})p(\overline{y}^{(i)})\end{align}$$
in [[21 Gaussian Mixture Models#^31dde4|GMM MLE]] in the first place.

---

### MLE for GMMs with Unknown Labels
Surprisingly, $\delta(j|i)$ is unknown in the real world! 

Let's revisit our probability density function as well as log-likelihood function for this distribution, **without** pre-knowledge of the labels, however. $$p(S_{n})=\prod_{i=1}^{n} \sum\limits_{j=1}^{k} \left(N(\overline{x}^{(i)}|\mu^{(j)},\sigma_{j}^{2})\gamma_{j}\right)$$$$\ln\left(P(S_{n})\right)= \sum\limits_{i=1}^{n} \ln\left(\sum\limits_{j=1}^{k} N(\overline{x}^{(i)}|\mu^{(j)},\sigma_{j}^{2})\gamma_{j}\right)$$
Given $k$ clusters, the above has model parameters $\overline{\theta}=[\gamma_{1},\cdots, \gamma_{k},\overline{\mu}^{(1)},\cdots, \overline{\mu}^{(k)},\sigma_{1}^{2}, \cdots, \sigma_{k}^{2}]$, where each mixture component is a **Spherical Gaussian** and $\gamma_{j}$ are the **mixing coefficients**.

---

#### Expectation Maximization Algorithm
1. Initialize model parameters
2. Iterate until convergence, for each point
	- **E-step**: use current estimate of mixture model to softly assign examples to clusters
	- **M-step**: re-estimate each cluster model separately based on the points assigned to it (similar to the *known label* case)

In each iteration, **likelihood** is guaranteed **NOT** to decrease.

![[Pasted image 20230414001819.png|600]]

##### E-Step
In **E-Step**, we fix $\overline{\theta}$, then **softly assign points** to clusters according to posterior probability $$p(j|i)=\frac{\gamma_{j}N(\overline{x}^{i}|\overline{\mu}_{j}, \sigma_{j}^{2})}{\sum_{t}\gamma_{t}N(\overline{x}^{i}|\overline{\mu}_{j}, \sigma_{j}^{2})}=\frac{\gamma_{j}P(\overline{x}^{(i)}|\overline{\mu}^{(j)},\sigma^{2}_{j})}{P(\overline{x}^{(i)}|\overline{\theta})}$$
This translates to *given a datapoint $\overline{x}^{(i)}$, what is the probability that cluster $j$ **generated it*** - analogous to $\delta(j|i)$. Note that $\sum_{j} p(j|i)=1$.

```ad-example
Example: Univariate Gaussian

![[Pasted image 20230407012902.png|600]]

In this case, since $P(1|1)$ is the most probable out of all 3 clusters for point $x^{(1)}$, we label $x^{(1)}$ with cluster 1.
```

##### M-Step
We optimize each cluster separately given $p(j|i)$ - the soft clusters we computed in E-Step. Note that this process is very similar to the **MLE Estimator** we get from labelled GMM. $$\begin{align}\hat{n}_{j}&=\sum\limits_{i=1}^{n} p(j|i)\\\hat{\gamma_{j}}&=\frac{\hat{n}_{j}}{n}\\\hat{\overline{\mu}}^{(j)}&=\frac{1}{\hat{n}_{j}}\sum\limits_{i=1}^{n} p(j|i)\overline{x}^{(i)}\\\hat{\sigma_{j}}^{2}&=\frac{1}{d\hat{n}_{j}} \sum\limits_{i=1}^{n} p(j|i) ||\overline{x}^{(i)}-\hat{\overline{\mu}}^{(j)}||^{2}\end{align}$$