[[2023-03-29]] #MLE #Probability

### Background, IID
- Identically distributed
	- E.g for Bernoulli distribution, **EACH** $x^{(i)}=1$ is with a probability of $p$ and **EACH** $x^{(i)}=0$ is with a probability of $1-p$.
- Independently distributed
	- E.g. for Bernoulli distribution, $p (x^{(i)}, x^{(j)})=\text{Bern}(x^{(i)}; p)\text{Bern}(x^{(j)}; p)$

Combining our IID assumptions, we arrive at
$$p(S_{n})=\prod_{i=1}^{n} p(\overline{x}^{(i)})$$

And our goal is to find which $p$ maximizes such result, which is the essence of MLE.

Let's apply this concept in a Gaussian distribution.

---

### Gaussian Distribution

```ad-important
**Definition 20.1**: Univariate Gaussian Distribution

Data with the following distribution:
$$\mathcal{N}(x|\mu, \sigma^{2})=\frac{1}{\sqrt{2\pi \sigma^{2}}} \exp\left({-\frac{(x-\mu)^{2}}{2\sigma^{2}}}\right)$$
```

^625fb3

And our goal is to determine $\mu$ and $\sigma^2$ that maximizes $p$, where
$$p(S_{n})=\prod_{i=1}^{n}\mathcal{N}(x|\mu, \sigma^{2})$$

Taking partial derivative of $p$ with respect to both parameters, we get
$$\mu_{\text{MLE}}=\sum\limits_{i=1}^{n} \frac{x^{(i)}}{n}$$
$$\sigma^{2}_{\text{MLE}}=\sum\limits_{i=1}^{n} \frac{(x^{(i)}-\mu_{\text{MLE}})^{2}}{n}$$

Now, we want to venture into a more general form (multivariate distribution).

### Multivariate Gaussian Distribution

```ad-important
**Definition 20.1**: Multivariate Gaussian Distribution

Data with the following distribution:
$$\mathcal{N}(\overline{x}|\overline{\mu}, \Sigma)=\frac{1}{(2\pi)^{\frac{d}{2}} \det({\Sigma})^{\frac{1}{2}}} \exp\left({-\frac{1}{2}(\overline{x}-\overline{\mu})^{T}\Sigma^{-1}}(\overline{x}-\overline{\mu})\right)$$

where $\overline{x}, \overline{\mu}$ are $d\times 1$ column vector, and $\Sigma$ is a $d\times d$ covariance matrix. 
- $\Sigma_{ij}$ measures the **covariance** between $x_i$ and $x_j$.
	- Intuitively, the diagonal is just the **variance**

We have the following property:
$$\overline{\mu}=\mathbb{E}[\overline{x}].$$
$$\Sigma = \mathbb{E}[(\overline{x}-\overline{\mu})(\overline{x}-\overline{\mu})^{T}]$$
```

```ad-example
Example: **Spherical Gaussians**

This is a unique type of Gaussian Distribution where
$$\Sigma = \sigma^{2}\textbf{I}_d$$

The distribution has only **ONE** free parameter - $\sigma^2$.

![[Pasted image 20230401152652.png|400]]

$$p(S_n)=\prod_{i=1}^{n}p(\overline{x}^{(i)})=\prod_{i=1}^{n} \left(\frac{1}{(2\pi\sigma^{2})^{\frac{d}{2}}} \exp\left(-\frac{1}{2\sigma^{2}}||\overline{x}^{(i)}-\overline{u}||^{2}\right)\right)$$


```

^1fcfef

