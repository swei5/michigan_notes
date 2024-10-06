[[2024-08-29]] #Valuation 

### Interest Rates
We focus on three main types of interest rates in the discussion below.
#### Simple Interest 

```ad-important
**Definition 2.1**: Simple Interest

Principal is **fixed** - no interest earned will be added to it.
$$V(t)=P(1+rt)$$ where $P$ is the principal.

If we start at some point $s \ge 0$, then $V(t)=P(1+r(t-s))$
- Here, $1+r(t-s)$ is known as the **growth factor**
- Reversely, $(1+r(t-s))^{-1}$ is the **discount factor**
```

#### Periodic Compounding Interest 

```ad-important
**Definition 2.2**: Periodic Compounding Interest

We reinvest the interest earned.
$$V(t)=P\left(1+\frac{r}{m}\right)^{mt}$$
Here, $m$ is the **compounding frequency**, and we assume $m=365$ if interest rate compounded daily.
-  $(1+\frac{r}{m})^{(t-s)m}$ is the **growth factor**
-  $(1+\frac{r}{m})^{-(t-s) m}$ is the **discount factor**
```

An **Example** of this is [[2.2 Discounted Cash Flow II#Annuity, Perpetuity|annuity]] - a payment of fixed amount $C$ is made at equal time intervals (e.g. at the end of each year) such as mortgages or salary.

The $PV$ of $n$ -th payment is $$\frac{C}{(1+r)^{n}}$$
The sum of the payment is $C \cdot A(r,n)$, where
$$A (r, n)=\sum_{i=1}^{n} ={(1+r)^{-i}}=\frac{1-(1+r)^{-n}}{r}$$
Hence, $PV$ of the annuity is 
$$C \frac{1-(1+r)^{-n}}{r}$$
An annuity is a **perpetuity** if $C$ is continued to payout forever, i.e. $n \to \infty$:  $$PV=\frac{C}{r}$$
#### Continuous Compounding Interest

```ad-important
**Definition 2.3**: Continuous Compounding Interest

If we calculate interest more frequently such that we let $m \to \infty$, we have that
$$V_m(t)=P\left[\left(1+\frac{r}{m}\right)^{\frac{m}{r}}\right]^{rt}$$
Since we know that $\lim_{x\to \infty} \left (1+\frac{1}{x}\right)^{x} =e$, thus $$V(t):=\lim_{m\to \infty}V_{m}(t)=Pe^{rt}$$
This is known as **continuous compounding**, and gives higher future value than periodic compounding.
- The **growth factor** is $e^{r(t-s)}$
- The **discount factor** is $e^{-r(t-s)}$
```

The **return** is $K (s, t)=\frac{V(t)-V(s)}{V(s)}=e^{r(t-s)}-1$
The **logarithmic return** is $k (s, t)=\ln \frac{V(t)}{S(t)}=r(t-s)$
- This is **additive**, s.t. $k(s,t)+k(t,u)=k(s,u)$