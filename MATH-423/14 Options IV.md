[[2024-10-22]] #Valuation #Derivatives 

### Time Value of Options

```ad-important
**Definition 14.1**: Moneyness

For European/American options with strike price $X$ and expiry date $T$ , if the price of the underlying asset at time $t$ is $S(t)$, then we will say that the call/put option is in/at/out of the money if

![[Pasted image 20241022131302.png|400]]
```

One may use the terms **deep in the money** and **deep out of the money** to mean considerably large difference between the two sides in the respective inequalities.

An American option in the money will bring a positive payoff if exercised immediately, whereas an option out of the money will not.

```ad-note
We use the same terms for European options, though their meaning is different: Even if the option is currently in the money, it may no longer be so on the exercise date. A European option in the money is more like a **promising asset**.
```

```ad-important
**Definition 14.2**: Intrinsic Value

At time $t \le T$ , the **intrinsic value** of a European/American option is defined by $$\begin{align}
\text{Call: } (S(t)-X)^{+} \\
\text{Put: } (X-S(t))^{+}
\end{align}$$
```

If an option is in the money, the intrinsic value is positive. If an option is out of the money or at the money, the intrinsic value is zero.

The price of an option at expiry $T$ **coincides** with the intrinsic value.

```ad-note
The price of an American option prior to expiry time **may be greater than** the intrinsic value because of the possibility of future gains but **CANNOT be smaller** than the intrinsic value.
- Because we can exercise an American option right away if in the money

The price of a European option prior to the exercise time may be greater or smaller than the intrinsic value.
```

```ad-important
**Definition 14.3**: Time Value of Options

The time value of an option (at time $t$) is the **difference** between the price (at time $t$) of the option and its intrinsic value (at time $t$).

![[Pasted image 20241022132422.png|500]]
```

Time value can be understood as the value of an option **arising from the time left to maturity**.

The time value of a European call option **can NEVER be negative**. Indeed, recall the [[12 Options II#^b5a9b4|bounds]] for the premium and observe that $$(S(t)-X)^{+}\le \left(S(t)-Xe^{-r(T-t)}\right)^{+}\le C_{E}(t)$$
However, when a European put option is **deep in the money**, the time value may be negative.

![[Pasted image 20241022132936.png|500]]
![[Pasted image 20241022132949.png|500]]

```ad-important
**Definition 14.4**: Maxima of Time Value

For any European/American call/put option with strike price $X$, its time value attains its **maximum** at $S=X$.
```

```ad-example
**Example**: Executing American Put Option Early

![[Pasted image 20241022133624.png|500]]

Note that (a) represents the **lower bound** of the American put option. So in an optimal scenario we should exercise in $[t,T)$.

```

---
### Binomial Tree Model
Recall the Binomial tree Model in Lecture 4: ![[4 Risky Assets#^29bdb3]]
And hence one-step return is $$K(n):=K(n-1,n)=\frac{S(n)-S(n-1)}{S(n-1)}$$
We assume that one-step returns $K (n), n=1,2,...$ are i.i.d. such that for each $n$, $$K(n)=\begin{cases}
u \text{ with probability }p \\
d \text{ with probability }(1-p) \\
\end{cases}$$ where $-1<d<u$ and $p\in (0,1)$. Further, we assume that $d <r <u$. See proof [[4 Risky Assets#^1a0562|here]].

The probability of the stock price $S(n)$ taking a particular value is $$\begin{align}
&\mathbb{P}(S(n)==S(0)(1+u)^{i}(1+d)^{n-i})\\
&= \binom{n}{i}p^{i}(1-p)^{n-i}
\end{align}$$

![[Pasted image 20241022135011.png|400]]

Although $S (n)$ is random, we could calculate (and are usually interested in calculating) the **expected price** $\mathbb{E}(S(n))$ and compare it with the risk-free investment $A(n)=A(0)(1+r)^{n}$.

```ad-important
**Definition 14.5**: Expected Value of Stock Price in Binomial Tree Model

$$\begin{align}
\mathbb{E}(S(n))&=S(0)\mathbb{E}(1+K(0,n))\\
&= S(0)(1+\mathbb{E}(K(1)))(1+\mathbb{E}(K(1)))\cdots (1+\mathbb{E}(K(n))) &\text{ S is iid}\\
&= S(0)(1+\mathbb{E}(K(1)))^{n} 
\end{align}$$
```

The comparison reduces to comparing $r$ and $\mathbb{E}(K(1))=pu+(1-p)d$.

```ad-important
**Definition 14.6**: Risk-Neutral Probability

The risk-neutral probability $p^\star$, is such that $$r=p^{\star}u+(1-p^{\star})d=\mathbb{E}^{\star}(K(1))$$

Or, $p^{\star}=r-d/u-d$ and is always in $[0,1]$ given our assumptions that $-1<d<u$ and $d <r <u$.
```

^f99ea9

The probability measure $\mathbb{P}^{\star}$, under which the stock price goes up with probability $p^\star$ and goes down with probability $1-p^{\star}$ independently at each node, is called **risk-neutral probability measure**.

The expectation under $\mathbb{P}^{\star}$ is called the **risk-neutral expectation** and is denoted by $\mathbb{E}^{\star}$.

```ad-note
$p^{\star}$ **may NOT reflect** the real/actual/physical market probability. The risk-neutral probability lives in a risk-neutral world that is not affected by real-world expectations. It is introduced for the **purpose of pricing derivative securities** such that we forbid any potential A.O..
```

Since the risk-neutral probability measure is just a special choice of $p=p^\star$, $$\mathbb{E}^{\star}(S(n))=S(0)(1+r)^{n}$$