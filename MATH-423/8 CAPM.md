[[2024-09-19]] #Portfolio #Valuation #Risks #CAPM 

Since the data for the risky assets, i.e., $(\mu_{i}, \sigma_{i})$ for $i= 1,\dots, n$, are the same for every investor, then everyone would choose to **invest on the Market Portfolio** (and the rest on the risk free asset).

In other words, the market will be *in equilibrium* and the **relative proportions of risky assets** held in each portfolio will be the **same** for all investors.

Equivalently, the weights $w_{M}=[w_{M,1}, \dots, w_{M,n}]$ represent the relative volume of the values of shares of the stocks with respect to the whole market. Then, $$w_{i}=\frac{n_{i}S_{i}}{\sum\limits_{i=1}^{n} n_{i}S_{i}}$$ where the denominator represents the total market cap.

---
### CAPM
Given a portfolio $V$, we want to understand how the return $K_{V}$ will react to trends affecting the whole market, i.e., the return $K_{M}$ of the market portfolio.

```ad-important
**Definition 8.1**: Beta

The beta factor of a portfolio/security $V$ is defined by $$\beta_{V}=\frac{\text{Cov}(K_{V},K_{M})}{\sigma^{2}_{M}}$$
```

This is essentially a regression problem. For each scenario $\omega$, we may plot the pair $K_{M}(\omega), K_{V}(\omega)$ onto a plane. In general, all points are randomly scattered, so will not lie on a same line.

![[Pasted image 20240925152634.png|300]]
We want to find a best line, or equivalently a pair of $(\beta, \alpha)$, that minimizes the RSS: $$f= \mathbb{E}[(K_{V}-(\beta K_{M}+\alpha))^{2}]$$Taking partial derivatives of $\beta, \alpha$ with respect to $f$ and setting them to zero finds us the minimizer: $$\begin{cases} \frac{\partial f}{\partial \beta} = 0 \implies \beta\mathbb{E}[K^{2}_{M}]-\mathbb{E}[K_{V}K_{M}]+\alpha\mathbb{E}[K_{M}] = 0 \\
\\ \frac{\partial f}{\partial \alpha} = 0 \implies \alpha-\mathbb{E}[K_{V}] + \beta\mathbb{E}[K_{M}]=0 \end{cases}$$ Solving simultaneously yields $$\begin{cases} \beta = \frac{\mathbb{E}[K_{V}K_{M}]-\mathbb{E}[K_{V}]\mathbb{E}[K_{M}]}{\mathbb{E}[K_{M}^{2}]-\mathbb{E}[E_{M}]^{2}}= \frac{\text{Cov}(K_{V}, K_{M}) }{\sigma_{M}^{2}}\\ \alpha = \mathbb{E}[K_{V}]-\beta_V \mathbb{E}[K_{M}]=\mu_V -\beta_{V}\mu_{M}\end{cases}$$
Note that this is the same conclusion we drew from [[1 Simple Regression#^70b481|statistics]] prior.

The line we eventually find $$y=\beta_{V}x+\alpha_V$$ is called the line of best fit, or regression line statistically. Rewriting $\alpha$ , we also get $$ \mu_V=\beta_V\mu_{M}+\alpha_{V}$$

```ad-important
**Definition 8.2**: Covariance of Two Portfolios

Let two portfolios $V_1$,$V_2$ have associated weights $w_{1}:=[w_{1,1},\dots, w_{1,n}]$ and $w_{2}:=[w_{2,1},\dots, w_{2,n}]$. Then, $$\text{Cov}(K_{V1},K_{V2})=w_{1}Cw_{2}^T$$ for $K:=[K_{1},\dots, K_{n}]$ is a row vector of random returns of risky assets.
```

This is true since $$\begin{align}\text{Cov}(K_{V1},K_{V2})&=\text{Cov}(w_{1}K^{T},w_{2}K^{T}) \\ &= \text{Cov}\left(\sum\limits_{i=1}^{n} w_{1,i} K_{i}, \sum\limits_{j=1}^{n} w_{1,j} K_{j}\right) \\ &= \sum\limits_{i=1}^{n} \sum\limits_{j=1}^{n} w_{1,i} w_{2,j} \text{Cov}(K_{i},K_{j}) \\&=w_{1}Cw_{2}^{T} \end{align}$$
This gives us the toolset to derive the following.

```ad-important
**Definition 8.3**: CAPM

In other words, the **expected return** $\mu_{V}$ for a feasible portfolio $V$ is given by the CAPM $$\mu_{V}=R+\beta_{V}(\mu_{M}-R)$$
```

```ad-note
**Proof of Definition 8.3**

We know that the **market portfolio** exists and its weights are $$w=\frac{(m-Ru)C^{-1}}{(m-Ru)C^{-1}u^{T}}$$ from [[7 Portfolio Management III#^061edf|Definition 7.3]].

Then, $$\begin{align}
\beta_{V}= \frac{\text{Cov}(K_{V}, K_{M}) }{\sigma_{M}^{2}} &= \frac{w_{V}Cw_{M}^{T}}{w_{M}Cw_{M}^{T}} \\ &= \frac{w_{V}C[(m-Ru)C^{-1}]^{T}}{w_{M}C[(m-Ru)C^{-1}]^{T}} \\ &= \frac{w_{V}(m-Ru)^{T}}{w_{M}(m-Ru)^{T}} \\ &= \frac{\mu_{V}-Rw_{V}u^{T}}{\mu_{M}-Rw_{M}u^{T}} \\ &= \frac{\mu_{V}-R}{\mu_{M}-R}
\end{align}$$

Plugging $\beta_{V}$ back into 8.3 verifies the equality.
```

---
### Security Market Line
It is clear from the CAPM that on the $(\beta,\mu)$ −plane all portfolios lie on the **straight line** described by CAPM $$\mu_{V}=R+\beta_{V}(\mu_{M}-R)$$
The graph of this function in the $(\beta,\mu)$ -plane is called the **Security Market Line** (SML).

![[Pasted image 20240926180038.png|500]]

```ad-note
It is easy to determine the the SML since we know two points $(0,R)$ and $(1, \mu_{M})$.

Despite some securities/portfolios have small returns and large risks, they remain attractive to the investors. The reason is that these securities have **negative beta**, or equivalently a **negative covariance**. This means that the prices of these securities tend to move on the opposite direction to the market.
```

#### Value at Risk (VaR)
Another term related to risk reduction is the Value at Risk. Assume that in time 0 we borrow $S(0)$ to buy a share of stock, so our portfolio is $(1, -S(0))$ for stocks and bonds. The probability that our loss in 1 year, i.e. $(S(0)e^{r}-S(1))$ is less than some **Value at Risk** (VaR) amount can be expressed as $$\mathbb{P}(S(0)e^{r}-S(1)<\text{VaR})$$
In general, we are more interested in constructing a **confidence interval**, and hence it takes the form $$\mathbb{P}(\text{loss} < \text{VaR}_{\alpha}) =\alpha$$ where $\alpha$ is the confidence level usually set to $95\%$ or $99\%$.

```ad-example
**Example: Computing VaR**

![[Pasted image 20240926182607.png|500]]

![[Pasted image 20240926182616.png|500]]
```