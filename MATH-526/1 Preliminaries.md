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

---
### Markov Chains
The fundamental idea is to characterize the evolution of systems with uncertainty. Essentially, our interest is to be able to **model changes** that are not known by certainty to us. Then, we can integrate those changes to obtain a description for the future of the system. We remark that almost all sufficiently complicated systems have an uncertain future. Thus, we need to model what we do not know, which is exactly the primary role of probability theory.

The fundamental question of interest is, given that the system is in a particular state, can you predict what will be the next state of the system. That is the information we want to integrate to obtain future distributions. The system might be formed by subatomic particles, galaxies, banks, animals, etc. and one can associate as many state variable as needed, such as position, momentum, balance sheet, connections etc. which characterizes the system well enough that allows better predictions of their potential future states.

**Markov Chains** (MC) models systems where past states are irrelevant but **only the current state** of the system determines the probabilities for the next state. To further simplify, we will consider MCs that does not depend on time $n$ either.

```ad-important
**Definition 1.8**: Temporally Homogeneous Discrete Markov Chain

We say a stochastic process $X_{n}$ with state space $\mathcal{S}$ is a **temporally homogeneous discrete Markov Chain** with transition matrix $p: \mathcal{S} \times \mathcal{S} \to [0,1]$, if for any $n \in \mathbb{N}$ and $x,y,x_{1},\cdots, x_{n-1} \in \mathcal{S}$, $$\mathbb{P}(X_{n+1}=y|X_{n}=x, X_{n-1}=x_{n-1}, \cdots, X_{1}=x_{1}) = \mathbb{P}(X_{n+1}=y|X_{n}=x)=p(x,\tilde{x})$$ whenever conditional probabilities are defined: $\mathbb{P}(X_{n}=x, X_{n-1}=x_{n-1}, \cdots, X_{1}=x_{1})>0$$.

Moreover, any transition matrix $p: \mathcal{S} \times \mathcal{S} \to [0,1]$ satisfying $\sum_{y \in \mathcal{S}}p(x,y)=1$ is called **stochastic matrix**, and any stochastic matrix can be associated with a **Markov Chain**.
```

```ad-example
**Example**: Transition Matrix

Consider a gambling game. At each round, you win \$1 with probability $0.4$ and lose \$1 with probabiliy $0.6$. Game ends when you lose all your money, or reach a fixed value $N$. To model this as a MC, let $\mathcal{S}= \{0,1,\cdots, N\}$. The transition matrix is given by $$\begin{align}
p(x,x+1) &= 0.4 \\
p(x,x-1) &= 0.6\\
p(0,0)=1\\
p(N,N)=1 \end{align}$$ for $0<x<N$. The transition matrix for $N=3$ can be represented by $$p=\begin{bmatrix}1 & 0 & 0 & 0 \\ 0.4 & 0 & 0.6 & 0 \\ 0 & 0.6 & 0 & 0.4 \\ 0 & 0 & 0 & 1\end{bmatrix}$$
Note that for each row (previous state), the probabilities of transitioning into some other states in the state space $\mathcal{S}$ add up to $1$.
```

Another example is the [Ehrenfest Chain](https://en.wikipedia.org/wiki/Ehrenfest_model).
