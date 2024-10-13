[[2023-04-10]] #Probability

### Bayesian Networks
We want to use **probabilistic model** because we can compute
- Marginal probability
- Conditional probability

```ad-important
**Definition 23.1**: Bayesian Network

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
**Definition 23.2**: Chain Rule of Probability
$$P(X_{1},\cdots, X_{d})=P(X_{1}|X_{2},\cdots, X_{d})P(X_{2}|X_{3},\cdots, X_{d})\cdots P(X_{d-1}|X_{d})P(X_{d})$$
```

^a9246f

Bayesian Networks encode such **conditional independencies**.

Thus, for a given graph, the **joint distribution** can be written as a product of the conditional probability of each **variable** given its **parents**.

```ad-important
**Definition 23.3**: Bayesian Network Factorization
For variables $X_{1},\cdots, X_{d}$, and the parents of variable $X_{i}$ represented by $pa_{i}$, $$P(X_{1},\cdots, X_{d})=\prod_{i=1}^{d} P(X_{i}|X_{pa_{i}})$$
```

^ea9d39

This allows us to simplify the conditional dependencies we've seen in [[#^a9246f|Definition 23.3]], which also allows us to capture dependencies that makes it easier to learn and infer. Illustrated by the example below

```ad-example
The notations of red and green texts denote the same probability.
![[Pasted image 20230414152033.png|600]]
```

#### Marginal and Conditional Independence
Note that Bayesian Networks encode **independencies**. There are two types of independences we want to explore.

```ad-important
**Definition 23.4**: Marginal Independence, Conditional Independence

Marginal Independence is defined as $$P(X_{1},X_{2})=P(X_{1})P(X_{2})$$

Conditional Independence is defined as $$P(X_{1}, X_{2}|X_{3})=P(X_{1}|X_{3})P(X_{2}|X_{3})$$

Denoted by $$X_{1}\perp X_{2}|X_{3}.$$

Alternatively, $P(X_{1}|X_{2},X_{3})=P(X_{1}|X_{3})$.
```

Note that for **conditional independence**, this is simply saying, if we are given $X_3$ and $X_1$, the distribution of $X_2$ is certain ($X_2$ doesn't affect $X_1$) since $X_1$ and $X_2$ are conditionally independent.

```ad-example
Let us show an example of marginal independence.

![[Pasted image 20230414155101.png|550]]
```

---

#### $d$ -Separation: Inferring Independence
To find out if nodes are marginally or conditionally independent in a Bayesian Network.

1. Keep only *ancestral* graph of the **variables of interest**.
	- Include the variables of interests themselves
	- Include the arrows (direction)
2. Connect nodes with **common child** and change graph to **undirected**
3. Then, we can use the following rules to infer independence properties.
	- If there is **NO Path** between variables of interest, then they are **marginally independent**
	- If **ALL paths** between variables of interest go through **a particular node**, then the variables are **independent given that node**
		- Intuitively can say that that **node** *blocks* the influence from the first variable to the second

```ad-note
For $X_{1}\perp X_{2}|\set{X_{3},X_4}$, **EACH** path has to go through at least **ONE** of the nodes in $\set{X_{3},X_4}$.
```

```ad-example
Question: Compute $P(B=T|A=T)$

![[Pasted image 20230414184511.png|600]]

Answer:
![[Pasted image 20230414184536.png|600]]
```

### Learning Bayesian Networks
There are two main problems.
1. Estimate **parameters** given **graph structure**
2. Search over **possible** **graph structures**

#### Parameter Estimation
Suppose we have $d$ discrete random variables of interests $x_{1},\cdots, x_{d}$ and each $x_{i}\in V_{i}$ where $V_{i} = \set{v_{1},v_{2},\cdots, v_{k_{i}}}$. Suppose we have $n$ complete observations for each variable $x_i$ such that $S_{n}=\set{[x_{1}^{(t)},\cdots, x_{d}^{(t)}]}_{t=1}^{n}$.

Then, from [[#^ea9d39|definition 23.4]] we can derive the log-likelihood function: 
$$l(\theta;S_{n}, G)=\ln \prod_{t=1}^{n} \prod_{i=1}^{d} \theta_{i}(x_{i}^{(t)}|x_{pa_{i}}^{(t)})=\sum\limits_{t=1}^{n} \sum\limits_{i=1}^{d} \ln \theta_{i}(x_{i}^{(t)}|x_{pa_{i}}^{(t)}) $$
where $G_{i}$ is the graph structure we are currently using.

```ad-important
**Definition 23.5**: MLE for of Parameter Estimation

The corresponding MLE to the problem above is $$\hat{\theta}_{i}(x_{i}=v_{i}|x_{pa_{i}}=v_{pa_{i}}) = \frac{n(x_{i}=v_{i}, x_{pa_{i}}=v_{pa_{i}})}{\sum_{v_{i}^{\prime}}n(x_{i}=v^{\prime}_{i}, x_{pa_{i}}=v_{pa_{i}})}$$

where $n(x_{i},x_{pa_{i}})$ is the number of observations in $S_{n}$ where $X_{i}=x_{i}$ and $X_{pa_{i}}=x_{pa_{i}}$.
```

^684350

#### Learning Graph Structure
The main idea here is to choose the graph structure that **maximizes log likelihood**.
- However, $G_{0}$ will almost always result in higher log-likelihood
	- This is because there is intuitively, a higher **number of parameters**

This is why we have BIC (a way to add penalty to number of parameters):

```ad-info
A **textual proof** of the intuition:

For any probability $P(X_{i}|X_{pa_{i}})$, our model could learn $P(X_{i}|X_{pa_{i}})=P(X_{i}|S \subset X_{pa_{i}})$. That is, for any specified parents of $X_{i}$, our model could just ignore some of those parents and learn that $X_{i}$ was conditionally independent of them.

Thus, a model with fewer connections can be learned on a model with more connections. This means that a model with more connections can learn anything and more than a model with fewer dependencies. However, adding more dependencies doesn’t necessarily mean we are capturing the correct trends in our data.
```

To get the number of parameters in a Bayesian Network, we have $$k=\sum\limits_{i=1}^{m} (r_{i}-1) \prod
_{j \in pa_{i}} r_{j}$$
suppose each random variable $X_{i}$ can take on $r_{i}$ possible values.

---

### Model Selection: Bayesian Information Criterion
Sometimes the **log-likelihood** score doesn't offer a comprehensive evaluation of the model performance as more parameters tend to increase log likelihood. In such cases, we want to consider other measures.

```ad-important
**Definition 23.6**: Bayesian Information Criterion
Defined as $$BIC(D; \overline{\theta})=l(D;\overline{\theta})-\frac{\#\text{param}}{2}\log(n)$$

where $\frac{\#\text{param}}{2}$ measures the **model complexity**, and $n$ is the number of training data
```

Here we’d want to **maximize** the BIC.
- If BIC is defined as the negative above, then we want to minimize

![[Pasted image 20230414002414.png|500]]
