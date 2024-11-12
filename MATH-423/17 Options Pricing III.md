[[2024-10-31]] #Valuation #Derivatives 

### $N$ -step Binomial Tree Model
For notational convenience, we enumerate the nodes of the binomial tree as follows: 

![[Pasted image 20241031130325.png|500]]

Here, we use $(n, j), j= 0,1,\cdots, n$ to denote at time $n$, from the top to the bottom of the tree.

In the following we will determine the price of the European derivative on the nodes at the exercise date $N$ , and then **backwardly at EVERY node** before the exercise date $N$.

In other words, we will answer the question for each $n=N, N-1, \cdots, 0$, *what is the value of the European derivative when the **price of the underlying asset** at time $n$ is $S(n)$?* 

We denote the value of the derivative security at node $(n,j)$ by $D(n,j)$. Recall that the European Option Price is a [[16 Options Pricing II#^5326c3|martingale]] under the binomial tree model: $$D(n,j)=\mathbb{E}^{\star}\left[\frac{D(n+1)}{1+r}|S(n)=S(n,j)\right]$$
Even better, the martingale property allows us to simplify the computation, avoid the backward evaluation, and directly calculate the derivative price. For example, in a $3$ -step binomial tree model, $$\begin{align}
D(0,0)&=\mathbb{E}^{\star}\left[\frac{f(S(3))}{(1+r)^{3}}|S(0)\right]=\mathbb{E}^{\star}\left[\frac{f(S(3))}{(1+r)^{3}}\right]\\
&= \frac{1}{(1+r)^{3}}[(p^{\star})^{3}f(S^{uuu})+3(p^{\star})^{2}(1-p^{\star})f(S^{uud})+3(p^{\star})(1-p^{\star})^{2}f(S^{udd})+(1-p^{\star})^{3}f(S^{ddd})]
\end{align}$$
```ad-important
**Definition 17.1**: Cox-Ross-Rubinstein

We can generalize the above calculation to pricing a European derivative security with payoff function $f$ and exercise date $N$ as follows: $$\begin{align}
D(0,0) &= \mathbb{E}^{\star}\left[\frac{f(S(N))}{(1+r)^{N}}|S(0)\right]=\mathbb{E}^{\star}\left[\frac{f(S(N))}{(1+r)^{N}}\right]\\
&= \binom{N}{i}\frac{1}{(1+r)^{N}} \sum\limits_{i=0}^{N} (p^{\star})^{N-i}(1-p^{\star})^{i} f(S(0)(1+u)^{N-i}(1+d)^{i})
\end{align}$$
where the index $i$ denotes the numbers of *downs* of the paths and $\binom{N}{i}=\frac{N!}{i!(N-i)!}$ denotes the total number of paths with exactly $i$ downs (therefore exactly $N-i$ ups).

The formula (CRR) is called **Cox-Ross-Rubinstein formula**. An alternative name for the binomial tree model is CRR model.
```

^b11d79

---
### $N$ -step, Binomial Tree Model, American Options
For the American call option, if the underlying stock pays no dividend, [[13 Options III#^ca55d2|then we know]] that $C_{A}=C_{E}$ could be calculated using the CRR formula. 

But for general American options we cannot derive a formula as simple as CRR, because we have to consider the **extra right to exercise early**. In particular, when “moving backwards” we need to compare at each node $(n,j)$: 
- The payoff of **exercising** at this node $f(S(n,j))$
- The value of **waiting** at this node: $$\begin{align}
w(n,j)&=\mathbb{E}^{\star}\left[\frac{D(n+1)}{1+r}|S(n)=S(n,j)\right]\\
&= p^{\star} \frac{D(n+1,j)}{1+r}+ (1-p^{\star}) \frac{D(n+1,j+1)}{1+r}
\end{align}$$
Then take the larger one with the corresponding action: $$D(n,j)=\max(f(S(n,j)),w(n,j))$$
