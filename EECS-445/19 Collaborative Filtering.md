[[2023-03-27]] #UnsupervisedLearning #Filtering

### Motivation
Assume we have a utility matrix of size $m \times n$ for $n$ customers and $m$ movies. We have some partial knowledge about *some* customers' preferences of *some* movies only, how do we solve for the missing ratings?

```ad-question
Given $Y$ with empty cells, how can we construct **low rank** $\hat{Y}$ to approximate $Y$ and use $\hat{Y}$ to predict missing ratings?
```

---

### Matrix Factorization

```ad-important
**Theorem 19.1**: Low-rank Factorization

Let $\hat{Y} \in \mathbb{R}^{n\times m}$ and $\text{rank}(\hat{Y})=r$. Then, there is $U\in \mathbb{R}^{n \times r}$ and $V^{T}\in \mathbb{R}^{r \times m}$ such that $\hat{Y}=UV^T$.
```
