[[2024-09-05]] #Stocks #Valuation 

### Dynamics of Stock Prices
We denote the stock price at time $t$ by $S(t)$ for any $t \ge 0$. Mathematically, $S(t)$ is defined as a positive random variable on a probability space $\Omega$ consisting of all feasible price movement scenarios $\omega \in \Omega$.
- $S(0)$ is known to the market

To be more precise, we shall write $S(t, \omega)$ to denote the stock price at time $t$. Now suppose time $t$ runs in a **discrete manner**: $t=n\tau$ where $n=0,1,2,\dots$ and $\tau$ is a **fixed time stamp**. We can thus write $S(t)=S(n\tau)$ .

---
### Returns
For two integers $n<m$, we substitute $K(n,m)$ for $K(n\tau, m\tau ;\omega)$ as the return over the time interval $[n\tau, m\tau]$ when the scenario $\omega$ is realized: $$K(n,m):=K(n\tau, m\tau, \omega)=\frac{S(m\tau, \omega)-S(n\tau, \omega)}{S(n\tau, \omega)}=\frac{S(m)-S(n)}{S(n)}$$
The return is a **random variable** as well; rearranging this equality gives
$$S(m)=S(n)(1+K(n,m))$$
Here we see that the **growth factor** is $1+K(n,m)$. ^6a7af0

Analogously, we can write the log return over the same time interval:
$$k(n,m):= \ln \frac{S(m)}{S(n)}$$
Rearranging this gives $$S(m)=S(n)e^{k(n,m)}$$
Like coupon being paid in bonds, when a dividend is paid, the stock price **DROPS** by the same amount. 

Since the dividend is decided **prior to paying date**, the drop of stock price is already reflected in $S(n)$. Thus, $S (n)+\text{div}(n)$ reflects the stock price at time $n$ if dividend **were NOT paid**. Hence, the one-step return and log return are 
$$K(n):=K(n-1,n) = \frac{S(n)+\text{div}(n)-S(n-1)}{S(n-1)}$$
And
$$k(n):=k(n-1,n)=\ln\frac{S(n)+\text{div}(n)}{S(n-1)}$$
As we need to account for the dividends earned during time $[n-1,n]$.

If no dividends are paid, then we have the following for one-step returns and log returns:
$$1+K(n,m)=(1+K(n+1))(1+K(n+2))\cdots (1+K(m))$$
$$k(n,m)=k(n+1)+k(n+2)+\cdots+k(m)$$
$$\mathbb{E}(k(n,m))= \mathbb{E}(k(n+1))+\mathbb{E}(k(n+2))+\cdots +\mathbb{E}(k(m))$$

Moreover, if one-step returns $K(n+1),...,K(m)$ are **independent**, then
$$1+\mathbb{E}(K(n,m))=(1+\mathbb{E}(K(n+1)))(1+\mathbb{E}(K(n+2)))\cdots (1+\mathbb{E}(K(m)))$$

```ad-note
**Short Proof of the equalities above**

![[Pasted image 20240906115452.png|500]]
```

---
### Binomial Tree Model
For stock prices, we extend One-Step Binomial Model to multiple steps so as to obtain a binomial tree structure for stock prices.

Assume one-step returns $K (n), n=1,2,...$ are i.i.d. such that for each $n$,
- $K(n)=u$ with probability $p$
- $K(n)=d$ with probability $1-p$ 
Where $-1<d<u$ and $p \in (0,1)$.
- This is true since $K (n)=\frac{S (n)-S (n-1)}{S (n-1)}>\frac{-S(n-1)}{S(n-1)}=-1$

In addition, we assume that one step return $r$ on a **risk-free investment** (bonds or cash held in a bank account) is the **same** at each time step and $d < r < u$. Why? Recall ![[1 Financial Market#^73727a]]
Assume for simplicity that $S(0) = 1$. The process for stock prices looks a tree: each node except the leaves has **two branches**. Starting from the root, each time going up (or down), the stock price obtains a factor $1 + u$ (or $1 + d$).
![[Pasted image 20240905141439.png|400]]

In general,
$$S(n)=S(0)(1+u)^{i}(1+d)^{n-1}$$ for $i=0,1,...,n$, i.e. $S(n)$ can take $n+1$ different values - the obtained tree is called the **recombining tree**.

The probability of $S (n)$ such that $S(n)=S(0)(1+u)^{i}(1+d)^{n-1}$ is
$$\mathbb{P}(S(n))=C(n,i) p^{i} (1-p)^{n-1}$$
In other words, $S \sim B(n,p)$.