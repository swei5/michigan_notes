[[2024-11-12]] #Valuation #Derivatives 

Now we've studied a discrete time model (Binomial Tree Model), we would like to set up a continuous time model to describe the evolution of a stock price $S$ over a period of time, say $1$ year. We first divide a calendar year into $N$ intervals with equal length: $$\tau_{N}:=\frac{1}{N}$$
Consider a **binomial model**, for the stock $S$, on the time grid $$\{0,\frac{1}{N},\frac{2}{N},\cdots,\frac{N}{N}\}=\{0,\tau_{N},2\tau_{N},\cdots, N\tau_{N}\}$$
We will denote the the **one-step return** over the time interval $[(m-1)\tau_{N},m\tau_{N}]$ by $K_{N}(m)$, for every $m=1,2,\cdots, N-1, N$.

Since we have assumed a binomial model, the sequence $(K_{N}(m))_{m=1,\cdots,N}$ is i.i.d with $$K_{N}(1)=\begin{cases}
u_{N} \text{ with probability }p \\
d_{N} \text{ with probability }(1-p) \\
\end{cases}$$ For simplicity we will assume that $p=1$.

Let us rewrite the binomial model in a more *convenient* form. For $t=\frac{m}{N}=m\tau_{N}$: $$\begin{align}
S_{N}(t)=S_{N}(m\tau_{N})&=S_{N}((m-1)\tau_{N})(1+K_{N}(m))\\
&= S_{N}((m-2)\tau_{N})(1+K_{N}(m-1))(1+K_{N}(m))\\
&= S_{N}(0) \prod\limits_{i=1}^{N}(1+K_{N}(j)) & S_{N}(0)  \text{ is deterministic}\\
&= S_{N}(0) \exp\left[\ln \left( \prod\limits_{i=1}^{N}(1+K_{N}(j)\right)\right] & S_{N}(0))\\
&= S_{N}(0) \exp\left[\sum\limits_{j=1}^{n}k_{N}(j)\right]
\end{align}$$ where $k_{N}(j)=\ln(1+K_{N}(j))$ is the **one step log return** of the stock.

We denote the **one-year return** by $K$, which satisfies $$1+K=\prod\limits_{i=1}^{N}(1+K_{N}(j)) $$ and the **one-year log return** by $k$, which satisfies $$k=\sum\limits_{j=1}^{n}k_{N}(j)$$

We denote the **continuously compounded interest rate** by $r$. Then, the per $\tau_{N}$ -period compounded interest rate $r_{N}$ satisfies $$e^{r}=(1+r_{N})^{N}$$

```ad-important
**Definition 20.1**: Volatility of a Stock

The **volatility** of a **STOCK** over $1$ year is defined to be the **standard deviation of the log return** of the stock price in $1$ year.

In addition, if we assume the volatility over $1$ year is $\sigma$. Then, the volatility over a period of length $\tau_{N}$ is $\sigma \sqrt{\tau_{N}}$.
```

