[[2024-10-24]] #Derivatives #Valuation 

### Martingale Property

```ad-question

![[Pasted image 20241025211116.png|400]]

In this example, after one time step, once it becomes known whether the stock price has gone up or down, what would be $\mathbb{E}^{\star}[S(2)]$?
```

Suppose $S(1)=120$. The tree will reduce (i.e. we can erase the lower branch) to the upper branch, which is itself a one-step tree. There are **two possible** values for $S(2)$. In this case, $$\mathbb{E}^{\star}[S(2)|S(1)=120]=120(1+r)$$
Note that this differs from the result seen in the example above because we're standing at $t=1$. Similarly, suppose $S(1)=80$. Then, $$\mathbb{E}^{\star}[S(2)|S(1)=80]=80(1+r)$$ Combining these two cases, we see that $$\mathbb{E}^{\star}[S(2)|S(1)]=S(1)(1+r)$$
We can generalize the above to any time step in the binomial tree model. 

```ad-important

```