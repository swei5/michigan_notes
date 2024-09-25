[[2024-09-17]] #Portfolio #Valuation #Risks #CAPM 

### Efficient Frontier
Let $V_{i}, i=1,2$ be two risky assets, e.g., stock price, portfolio etc., with associated data $(\sigma_{i},\mu_{i}), i=1,2$. Then,
1. If $\mu_{1}=\mu_2$, we naturally prefer the asset with **lower risk**
2. If $\sigma_{1}= \sigma_2$, we naturally prefer the asset with **higher expected return**
3. If $\sigma_{1} \le \sigma_{2}$ and $\mu_{1} \le \mu_2$, i.e., high risk-high returns vs. low risk-low returns, then depends on the **profile** of the investor

```ad-important
**Definition 7.1**: Dominant Portfolio, Efficient Frontier

We say that stock (security, or portfolio) 1 **dominates** (or is more preferable than) stock (security, or portfolio) if $\mu_{1} \ge \mu_2$ and $\sigma_{1} \le \sigma_2$.

A portfolio is called **efficient** if it is **NOT dominated** by any other portfolio in the attainable set. The set of all efficient portfolios is called the **efficient frontier**.
```

![[Pasted image 20240923144619.png|300]]

Graphically in Markowitz's Bullet, this is represented by the red line in bold. In other words, this is the region where the portfolio has
- Highest expected return among all attainable portfolios with the same risk 
- Lowest risk among all attainable portfolios with the same expected returns

For our convenience, we will denote the weights of the portfolios lying on the efficient frontier by $w_{EF}$ instead of $w_{\mu_{V}}^{\min}$, for $\mu_{V} \ge \mu_{MVP}$.

The efficient frontier is the set of points whose weights $w_{EF}$ are given by $$w_{EF}=A\mu+B$$ for $\mu > \mu_{MVP}$ and $A=\frac{(am-cu)C^{-1}}{ad-bc}$ and $B=\frac{(du-bm)C^{-1}}{ad-bc}$ referenced [[6 Portfolio Management II#^bdc70b|here]].

From now on we extend the market of the $n$ **risky assets** by introducing a **risk-free asset** with return $R$.
- Moreover, we assume $R < \mu_{MVP}$ (otherwise arbitrage opportunity)

---
### Market Portfolio

```ad-important
**Definition 7.2**: Market Portfolio

Recall the graph of the set of portfolios that we can construct based on two assets; one risk free and one risky. Then,
1. It is immediate that the portfolio $V$ should **lie on the efficient frontier**
	- If $V$ does not lie on the efficient frontier, **there exists** another portfolio that provides **either a higher return for the same level of risk** or **lower risk for the same level of return**; in that case, we could replace $V$ with a more efficient portfolio
3. We should choose the portfolio $V$ so that the half-line starting at $(0, R)$ and passing through $V$ is **tangent to the efficient frontier**
	- The tangent line represents the highest Sharpe ratio

In other words, we can uniquely identify a portfolio $M$, which will be given the special name **Market Portfolio**.

![[Pasted image 20240925133018.png|300]]
```

```ad-important
**Definition 7.3**: Weights of Market Portfolio

Under the assumption that $R < \mu_{MVP}$ and if $C$ is invertible, then the market portfolio $M$ **exists** and its weights are given by $$w=\frac{(m-Ru)C^{-1}}{(m-Ru)C^{-1}u^{T}}$$
```

```ad-note
**Proof of 7.3**

We again have a maximization problem: $$\max\limits_{w} \frac{mw^{T}-R}{\sqrt{wCw^{T }}} \text{ subject to } uw^{T}=1$$

To this end we define the Lagrangian $$\mathcal{L}(w, \lambda):= \frac{mw^{T}-R}{\sqrt{wCw^{T }}} -\lambda(uw^{T}-1)$$

By following the proper steps, we derive $$\begin{align}\nabla \mathcal{L}=0 &\iff \frac{m[wCw^{T}]^{\frac{1}{2}}-(mw^{T}-R)[wCw^{T}]^{-\frac{1}{2}}wC}{wCw^{T}} - \lambda u = 0 \\ &\iff m-\lambda[wCw^{T}]^{\frac{1}{2}}u = \frac{mw^{T}-R}{wCw^{T}}wC & (1) \\ &\implies \left(m-\lambda[wCw^{T}]^{\frac{1}{2}}u\right)w^{T} = mw^{T}-R &\text{ Multiplying } w^{T}
\\ &\implies mw^{T} - \lambda[wCw^{T}]^{\frac{1}{2}} = mw^{T}-R & uw^{T}=1
\\ &\implies \lambda = \frac{R}{[wCw^{T}]^{\frac{1}{2}}}
\end{align}$$

Subbing in $\lambda = \frac{R}{[wCw^{T}]^{\frac{1}{2}}}$ back to (1), we have $$\begin{align}\frac{mw^{T}-R}{wCw^{T}}w&=(m-Ru)C^{-1} & (2) \end{align}$$

For notational simplicity, let us define $\gamma := \frac{mw^{T}-R}{wCw^{T}}$ - observe that $\gamma$ depends on $w$. Multiplying $u^{T}$ on both sides and we get $$\gamma = (m-Ru)C^{-1}u^{T}$$ and hence we arrive at $$w=\frac{(m-Ru)C^{-1}}{(m-Ru)C^{-1}u^{T}}$$

At this point, in addition, we have to make sure that $\gamma \ne 0$. Recall that $$\begin{align} 
\mu_{MVP} = w_{MVP}m^{T}&= \frac{uC^{-1}m^{T}}{uC^{-1}u^{T}} & \text{ Definition } 6.4
\\ &= \frac{mC^{-1}u^{T}}{uC^{-1}u^{T}} 
\\ &> R & \text{ Assumption }
\\ &\iff mC^{-1}u^{T} > RuC^{-1}u^{T}
\\ &\iff mC^{-1}u^{T} - RuC^{-1}u^{T} > 0
\\ &\iff \gamma > 0
\end{align}$$
```

```ad-important
**Definition 7.4**: Capital Market Line

The half-line that starts at the risk-free asset and runs through the market portfolio $M$ will be said the Capital Market Line (CML) and it satisfies the equation: $$\mu = R +\frac{\mu_{M}-R}{\sigma_{M}}\sigma$$ where $\frac{\mu_{M}-R}{\sigma_{M}}$ is the **risk premium**.
```

