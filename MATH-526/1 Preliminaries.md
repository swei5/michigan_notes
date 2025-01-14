[[2025-01-09]] #Probability 

Up until later on in the course, where we will introduce the notions in the measure theory, we will rely on the construction of probability theory for the **countable state spaces**. For this purpose, we will recall the core definitions.

Let $\Omega$ be a countable set, endowed with a mapping $p \to [0,1]$ satisfying $\sum_{\omega \in \Omega} p(\omega)=1$. We introduce the probability distribution $\mathbb{P}$ from subsets of $\Omega$ to $[0,1]$ as $$\mathbb{P}(E):=\sum\limits_{\omega \in E}p(\omega), E \subset \Omega$$
```ad-important
**Definition 1.1**: Conditional Probability
$$\mathbb{P}(A|B):= \frac{1}{\mathbb{P}(B)} \mathbb{P}(A \cap B)$$
```

```ad-important
**Definition 1.2**: Bayes’ Rule

$$\mathbb{P}(A|B) \mathbb{P}(B) = \mathbb{P}(B|A) \mathbb{P}(A) $$
```

```ad-important
**Definition 1.3**: Law of total probability

$$\mathbb{P}(A) = \sum\limits_{i} \mathbb{P}(A \cap B_{i}) = \sum\limits_{i} \mathbb{P}(A|B_{i}) \mathbb{P}(B_{i}) $$ where $\cup_{i} B_{i} = \Omega$ and $B_{i}$'s are **disjoint**. It is also useful in computations to note that $$\mathbb{P}(A|C) = \sum\limits_{i} \mathbb{P}(A \cap B_{i} | C) = \sum\limits_{i} \mathbb{P}(A | B_{i} \cap C) \mathbb{P}(B_{i}|C) $$ if $B_{i}$'s are disjoint and $C \subset \cup_{i} B_{i}$.
```

```ad-important
**Definition 1.4**: Independence

We say $\{A_{i}\}_{i=1}^{n}$ are mutually independent if for any $1 < j_{1} < \cdots < j_{k} < n$ $$\mathbb{P} \left(\bigcap\limits_{\ell =1}^{k} A_{j \ell}\right) = \prod\limits_{\ell = 1}^{k} \mathbb{P}(A_{j \ell})  $$
```

```ad-important
**Definition 1.5**: Random Variable (RV)

We call a mapping $X: \Omega \to \mathbb{R}$ a random variable (RV). Let $R_{X} \subset \mathbb{R}$ be the range of $X$, which is also countable. Notice that any RV induces a distribution on $R_{X}$, which we call the **law** of $X$: $$\mathcal{L}_{X}(E):= \mathbb{P}(\{\omega \in \Omega: X(\omega) \in E\}) := \mathbb{P}(X^{-1}(E)), E \subset R_{X} $$
```

```ad-important
**Definition 1.6**: Expectation

Expected of $X$ is defined as $$\mathbb{E}[X] := \sum\limits_{\omega \in \Omega} X(\omega) \mathbb{P}(\omega) = \sum\limits_{x \in R_{X}} x \mathcal{L}_{X}(x) $$

We can also define the expected value given an event, and decompose an expectation as follows, $$\mathbb{E}[X] = \sum\limits_{i} \mathbb{E}[X|B_{i}] \mathbb{P}(B_{i}):= \sum\limits_{i} \left(\sum\limits_{\omega \in \Omega} X(\omega) \mathbb{P}(\omega | B_{i}) \right) \mathbb{P}(B_{i}) $$ where $\cup_{i} B_{i} = \Omega$ and $B_{i}$'s are **disjoint**.
```

```ad-example
**Example**: Law of $X$, Expectation

Let us go over a simple example of tossing two coins. Let $\Omega = \{HH, TT, HT, TH\}$, with equal probabilities ($\mathbb{P}$ is a uniform distribution). Define $X(HH)=1$ and $0$ otherwise. Then $\mathcal{L}_{X}(1)= \frac{1}{4}$ and $\mathcal{L}_{X}(0) = \frac{3}{4}$. 

The expected is $$1 \cdot \mathcal{L}_{X}(1) + 0 \cdot \mathcal{L}_{X}(0)$$
```

```ad-important
**Definition 1.7**: Stochastic Process

We call $\{X_{n}\}_{n \in \mathbb{N}}$ a stochastic process with state space $\mathcal{S}$, if for all $n \in \mathbb{N}$, $X_{n}$ is a RV with values in $\mathcal{S}$, i.e. $X_{n} : \Omega \to \mathcal{S}$. We refer to $n \mapsto X_{n}(\omega)$ as a **path** of $X$.
```

Notice that even if the state space has only two elements, there are $2^{n}$ many potential paths of $(X_{1}, \cdots ,X_{n})$.  If the common probability space $\Omega$ has only few elements, then one cannot have rich dynamics. Typically, we ignore $\Omega$ and concentrate on the state space itself.
