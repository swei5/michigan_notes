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


```