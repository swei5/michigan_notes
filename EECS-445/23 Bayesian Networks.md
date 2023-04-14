[[2023-04-10]] #BayesianNetwork #UnsupervisedLearning 

### Model Selection: Bayesian Information Criterion
Sometimes the **log-likelihood** score doesn't offer a comprehensive evaluation of the model performance. In such cases, we want to consider other measures.

```ad-important
**Definition 23.1**: Bayesian Information Criterion
Defined as $$BIC(D; \overline{\theta})=l(D;\overline{\theta})-\frac{\#\text{param}}{2}\log(n)$$

where $\frac{\#\text{param}}{2}$ measures the **model complexity**, and $n$ is the number of training data
```

Here we’d want to **maximize** the BIC.
- If BIC is defined as the negative above, then we want to minimize

![[Pasted image 20230414002414.png|500]]

---

### Bayesian Networks
We want to use **probabilistic model** because we can compute
- Marginal probability
- Conditional probability

```ad-important
**Definition 23.2**: Bayesian Network

A **Bayesian network** is a **probabilistic graphical model** that represents a set of **variables** and their **conditional dependencies** via a directed acyclic graph (DAG).
```

We can illustrate this idea using a simple example.

```ad-example
Bayesian Network of Coin-flipping

![[Pasted image 20230414014303.png|600]]

Given this example, the factorization based on the graph is $P(x_{1}, x_{2}, x_{3})=P(x_{1})P(x_{2})P(x_{3}|x_{1}, x_{2})$

Note that this is actually $P(x_{1})P(x_{2}|x_{1})P(x_{3}|x_{1}, x_{2})$; however, since $X_{1}$ and $X_{2}$ are independent events, we ignore the conditional.
```

Let's look at this problem in a general case.

```ad-important
**Definition 23.3**: Chain Rule of Probability
$$P(X_{1},\cdots, X_{d})=P(X_{1}|X_{2},\cdots, X_{d})P(X_{2}|X_{3},\cdots, X_{d})\cdots P(X_{d-1}|X_{d})P(X_{d})$$
```

Bayesian Networks encode such **conditional independencies**.

Thus, for a given graph, the **joint distribution** can be written as a product of the conditional probability of each **variable** given its **parents**.

```ad-important
**Definition 23.4**: Bayesian Network Factorization
For variables $X_{1,}$
```