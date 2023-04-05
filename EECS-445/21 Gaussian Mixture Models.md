[[2023-04-03]] #Gaussians #Mixture

### Recap, Spherical Gaussian
Recall previous discussions on Spherical Gaussian Distribution being a special type of Multivariate Gaussian Distribution: ![[20 MLE, Gaussians#^1fcfef]]

---

### MLE Derivation
To **maximize** $p(S_n)$, we apply the **log MLE** method. Let $$\begin{align}l(S_{n};\overline{\mu}, \sigma^{2})=\ln(p(S_{n}))&=\ln\left(\prod_{i=1}^{n} \left(\frac{1}{(2\pi\sigma^{2})^{\frac{d}{2}}} \exp\left(-\frac{1}{2\sigma^{2}}||\overline{x}^{(i)}-\overline{u}||^{2}\right)\right)\right)\\
&=\sum\limits_{i=1}^{n}\ln\left(\frac{1}{(2\pi\sigma^{2})^{\frac{d}{2}}} \exp\left(-\frac{1}{2\sigma^{2}}||\overline{x}^{(i)}-\overline{u}||^{2}\right)\right)\\
&=\sum\limits_{i=1}^{n}\left(\ln\left(\frac{1}{(2\pi\sigma^{2})^{\frac{d}{2}}}\right)-\frac{1}{2\sigma^{2}}||\overline{x}^{(i)}-\overline{u}||^{2}\right)\\
&=\sum\limits_{i=1}^{n}\left(-\frac{d}{2}\ln(2\pi\sigma^{2})-\frac{1}{2\sigma^{2}}||\overline{x}^{(i)}-\overline{u}||^{2}\right)
\end{align}$$
Then, we take gradient with respect to $\mu$ to find our MLE for $\overline{\mu}$: $$
\begin{align}\nabla_{\overline{\mu}}l&=-\frac{1}{2\sigma^{2}}\sum\limits_{i=1}^{n}\nabla_{\overline{\mu}}\left(||\overline{x}^{(i)}-\overline{u}||^{2}\right)\\
&=\frac{1}{\sigma^{2}}\sum\limits_{i=1}^{n}(\overline{x}^{(i)}-\overline{\mu})
\end{align}$$
Setting this to zero gives $$\overline{\mu}_{\text{MLE}}=\frac{\sum_{i=1}^{n}\overline{x}^{(i)}}{n}$$
Doing similar, but more complex things with respect to $\sigma^{2}$, we get the following $$\sigma^{2}_{\text{MLE}}=\frac{\sum_{i=1}^{n}||\overline{x}^{(i)}-\overline{\mu}_{\text{MLE}}||^{2}}{nd}$$

```ad-info
**Derivation of MLE for variance**

![[Pasted image 20230404013720.png|500]]

```

---

### Mixture Distributions
Sometimes, a single Gaussian is not a good fit for this dataset.

![[Pasted image 20230404013810.png|400]]

In this model each datapoint $\overline{x}^{(i)}$ is assumed to be **generated** from a mixture of $k$ distributions.

Let us consider the case where data labels are **known**; that being said, we know the cluster with which the data point is associated. First, we define an indicator function $$\begin{equation}\delta(j|i) =
    \begin{cases}
      1 \text{ if }x_{i}\in \text{cluster }j\\
      0 \text{ otherwise}\\
    \end{cases} \end{equation}$$
```ad-important
**Definition 21.1**: MLE for GMMs with known labels

We can write our probability density function as $$\begin{align}p(S_{n})&=p(\overline{x}^{(i)},\overline{y}^{(i)})\\
&=\prod_{i=1}^{n} p(\overline{x}^{(i)}|\overline{y}^{(i)})p(\overline{y}^{(i)})\\
&=\prod_{i=1}^{n} \sum\limits_{j=1}^{n} \delta(j|i) \left(N(\overline{x}^{(i)}|\mu^{(j)},\sigma_{j}^{2})\gamma_{j}\right)
\end{align}$$

And define that
$$\hat{n}_{j}=\sum\limits_{i=1}^{n} \delta(j|i)$$

is the number of points assigned to cluster $j$.

$$\gamma_{j}=\frac{\hat{n}_{j}}{n}$$

is the fraction of points assigned to cluster $j$.
```

From the above, we can find the mean as well as variance of our GMMs: $$\begin{align}\overline{\mu}^{(j)}&=\frac{1}{\hat{n}_{j}}\sum\limits_{i=1}^{n} \delta(j|i)\overline{x}^{(i)}\\
\sigma_{j}^{2}&=\frac{1}{d\hat{n}_{j}}
\end{align}$$

