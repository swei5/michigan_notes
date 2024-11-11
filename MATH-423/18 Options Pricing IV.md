[[2024-11-05]] #Valuation #Derivatives 

#### $N$ -step Binomial Tree Model (i.d.)
Prior to this lecture, we've considered a $N$ -step binomial tree model where one-step returns $K (n), n=1,2,...$ are **i.i.d.** such that for each $n$, $$K(n)=\begin{cases}
u \text{ with probability }p \\
d \text{ with probability }(1-p) \\
\end{cases}$$ where $-1<d<u$ and $p\in (0,1)$. 

Let us consider now a (non-standard) binomial model for the stock prices $S$, where the one-step returns are **NOT identically distributed** (i.d.).

In other words, the risk-neutral probability $p^{\star}$ (stock goes up) might be **DIFFERENT** at each node. Let's therefore use a slightly different notation to demonstrate. Let 
- $S$ be the price of the stock at the root of the one-step sub-tree
- $S^{\uparrow}$ be the price of the stock when it *moves up* in the next step
- $S^{\downarrow}$ be the price of the stock when it *moves down* in the next step

This implies $$p^{\star}=\frac{S(1+r)-S^{\downarrow}}{S^{\uparrow}-S^{\downarrow}}$$ which is a generation case for the [[14 Options IV#^f99ea9|calculation of the risk-neutral probability]].

```ad-example
**Example**: European option pricing (non i.d. case)

![[Pasted image 20241111142131.png|500]]

![[Pasted image 20241111142140.png|500]]

![[Pasted image 20241111142155.png|500]]
```

```ad-example
**Example**: American option pricing (non i.d. case)

![[Pasted image 20241111142352.png|500]]

![[Pasted image 20241111142409.png|500]]
```
