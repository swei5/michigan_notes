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
Recall the Binomial tree Model in Lecture 4: ![[4 Risky Assets#^6a7af0]]
And hence one-step return is $$K(n):=K(n-1,n)=\frac{S(n)-S(n-1)}{S(n-1)}$$
We assume that one-step returns $K (n), n=1,2,...$ are i.i.d. such that for each $n$, $$K(n)=\begin{cases}
u \text{ with probability }p \\
d \text{ with probability }(1-p) \\
\end{cases}$$
