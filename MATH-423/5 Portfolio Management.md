[[2024-09-10]] #Portfolio #Valuation 

### Portfolio Returns
Assume we have a simple case: two stocks and two time spots.

![[Pasted image 20240910130414 1.png|400]]

We define the **weights** of a stock $i$ (in the portfolio) by
$$w_{i}=\frac{x_{i}S_{i}(0)}{V(0)}$$
Note that $V(0)$ is the initial wealth of the portfolio and $w_{1}+w_{2}=1$. $w_{1}, w_{2} \in \mathbb{R}$ - can be zero and negative.

```ad-important
**Definition 5.1**: Portfolio Returns

For a portfolio consisting of two securities, 
$$K_{V}=w_{1}K_{1}+w_{2}K_{2}$$

where $K_{i} := \frac{S_{i}(1)-S_{i}(0)}{S_{i}(0)}, i=1,2$ are the returns of the  portfolio and of stock $i$ over time period $[0,1]$.

For log return, we have: 
$$e^{kv}=w_{1}e^{k_{1}}+w_{2}e^{k_{2}}$$

where $k_{V}:= \ln \frac{V(1)}{V(0)}$ and $k_{i}:= \ln \frac{S_{i}(1)}{S_{i}(0)}, i=1,2$ are the returns of the  portfolio and of stock $i$ over time period $[0,1]$.
```

```ad-important
**Definition 5.2**: Expected Returns

For an asset $X$, the expected return is defined by $\mu_{X} := \mathbb{E}[K_{X}]$.  
- E.g. $\mu_{i} := \mathbb{E}[K_i]$ for stock $i = 1, 2$, $\mu_{V} := \mathbb{E}[K_{V} ]$ for the portfolio $V$  

The risk is defined as the **standard deviation** of the return:  
$$\sigma_{V} := \sqrt{\text{Var}(K_{V})}$$
is the risk of the portfolio; and
$$\sigma_{i} := \sqrt{\text{Var}(K_{i})}$$
is the risk of stock $i, i = 1, 2$.  

For two assets $X$, $Y$, we define their **correlation coefficient**, denoted by $\rho_{XY}$ , as the correlation between $K_{X}$ and $K_{Y}$.
```

---
### Portfolio Risks
Recall the definition of covariance ![[8.1 Portfolio Theory#^8e3717]]
And how it relates to the correlation between the two assets ![[8.1 Portfolio Theory#^983767]]
Further, if $|\rho_{AB}|=1$, then the returns $K_{A},K_{B}$ are **affinely related**, i.e. there exists $\alpha, \beta$ such that
$$K_{A}=\alpha K_{B}+\beta$$
And that
$$\text{Sign}(\alpha)=\text{Sign}(\rho_{AB})$$


We may write the expected return and risks of a portfolio as ![[8.1 Portfolio Theory#^b85381]]
For a portfolio consisting of two securities,
$$\mu_{V}=w_1\mu_1+w_2\mu_2$$
And
$$\begin{align}\sigma_{V}^{2}&=w_{1}^{2}\sigma_{1}^{2}+w_{2}^{2}\sigma_{2}^{2}+2w_{1}w_{2}\text{Cov}(K_{1},K_{2})\\&=w_{1}^{2}\sigma_{1}^{2}+w_{2}^{2}\sigma_{2}^{2}+2w_{1}w_{2}\rho_{12}\sigma_{1}\sigma_{2}\end{align}$$

```ad-note
If short sales are not allowed, then $\sigma_{V} \le \max(\sigma_{1},\sigma_{2})$.

No short sales means $x_{1},x_{2}\ge 0$ namely $w_{1},w_{2} \ge 0$. Assume WLOG, that $\sigma_{1}\ge \sigma_{2}$ or equivalently $\sigma_{1} =\max(\sigma_{1},\sigma_{2})$.

$$\begin{align}
(K_{1},K_{2})&=w_{1}^{2}\sigma_{1}^{2}+w_{2}^{2}\sigma_{2}^{2}+2w_{1}w_{2}\rho_{12}\sigma_{1}\sigma_{2}
\\ &\le w_{1}^{2}\sigma_{1}^{2}+w_{2}^{2}\sigma_{2}^{2}+2w_{1}w_{2}\sigma_{1}\sigma_{2}
\\ &= (w_{1}\sigma_{1}+w_{2}\sigma_{2})^{2}
\\ &\le (w_{1}\sigma_{1}+w_{2}\sigma_{1})^{2}
\\ &= \sigma_{1}^{2} = \max(\sigma_{1},\sigma_{2})
\end{align}$$
```

---
### Risks Reduction
Note that $\rho_{12} \in [−1, 1]$. First consider the risk reduction in two extreme cases.
1. If $\rho_{12}=1$, then $\rho_{V}=|w_{1}\sigma_1+w_{2}\sigma_{2}|$. In particular, when $\sigma_{1} \ne \sigma_2$:
$$\sigma_{V=0}\iff w_1=\frac{-\sigma_2}{\sigma_{1}-\sigma_{2}} \text{ and } w_{2}=\frac{\sigma_{1}}{\sigma_{1}-\sigma_{2}}$$
Here either $w_1$ or $w_2$ is negative, implying that **short sales must happen**.

2. If $\rho_{12}=-1$, then $\rho_{V}=|w_{1}\sigma_1-w_{2}\sigma_{2}|$. In particular