[[2025-01-14]] #Probability 

### Markov Chains
The fundamental idea is to characterize the evolution of systems with uncertainty. Essentially, our interest is to be able to **model changes** that are not known by certainty to us. Then, we can integrate those changes to obtain a description for the future of the system. We remark that almost all sufficiently complicated systems have an uncertain future. Thus, we need to model what we do not know, which is exactly the primary role of probability theory.

The fundamental question of interest is, given that the system is in a particular state, can you predict what will be the next state of the system. That is the information we want to integrate to obtain future distributions. The system might be formed by subatomic particles, galaxies, banks, animals, etc. And one can associate as many state variable as needed, such as position, momentum, balance sheet, connections etc. Which characterizes the system well enough that allows better predictions of their potential future states.

**Markov Chains** (MC) models systems where past states are irrelevant but **only the current state** of the system determines the probabilities for the next state. To further simplify, we will consider MCs that does not depend on time $n$ either.

```ad-important
**Definition 2.1**: Temporally Homogeneous Discrete Markov Chain

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

Then the transition matrix is given by
```

Another example is the [Ehrenfest Chain](https://en.wikipedia.org/wiki/Ehrenfest_model).

```ad-example
**Example**: Inventory Chain

Let $X_{n}$ denote the stock in the inventory at the end of day $n$. At the beginning of the day, if the inventory is less or equal to $s$, we order enough to bring total stocks to $S$. Let $D_{n}$ be the demand throughout day $n$. The dynamics of $X_{n}$ are given by $$X_{n+1}=\begin{cases}
(X_n - D_{n+1})^{+} & \text{if } X_n > s \\
(S- D_{n+1})^{+} & \text{if } X_n < s
\end{cases}$$

Suppose now an electronic store sells a video game. Set $S=5$, $s=1$. Let the demand be $$\mathbb{P}(D_n = k) = \begin{cases}
0.3 & \text{if } k = 0 \\
0.4 & \text{if } k = 1 \\
0.2 & \text{if } k = 2 \\
0.1 & \text{if } k = 3 \\
\end{cases}$$

Then the transition matrix $p$ is given by

![[Pasted image 20250114101952.png|300]]

The reason why $p_{0,2}$ is $0.3$ is because the probability of the store ending with an inventory level of $0$ correspond to both scenarios of (1) $D_{n+1}=2$ (the store sells all its inventory) and (2) $D_{n+1}=3$ (the store sells all its inventory and forgoes the additional $1$ unit of demand), which sums up to a probability of $0.3$.

Now, suppose each unit solds for \$12 and storage cost is \$2. What is the long-run profit per day of this inventory policy? How do we choose $s$ and $S$ to maximize profits?
```

Suppose we want our model to take into account **NOT ONLY one previous** states but perhaps multiple, we can extend our Markov Chain and combine multiple states as **ONE** and enlarge the state space.

```ad-example
**Example**: Two-Stage Markov Chains

We can easily extend a Markov Chain such that $X_{n+1}$ depends on $X_{n}$, $X_{n-1}$. For example, consider a basketball player who makes a show with following probabilities: 
- $1/2$ if he has missed the last two times
- $2/3$ if he has missed one of his last two shots
- $3/4$ if he hits both shots

Notice that the probability of next shot is given by the previous two. Therefore, let the state space be $\mathcal{S} = \{HH, MM, HM, MH\}$. The transition matrix is given by 

![[Pasted image 20250114103428.png|300]]
```

---
### Multistep Transition Probabilities
In this section, we aim to show that $$p^m (x,y) = \mathbb{P}(X_{n+m}=y|X_n =x)$$ 
That is, the probability of getting from the state $x$ to $y$ in $m$ -steps is given by the $m$ -th power of the **transition matrix** $p$.

By induction, it holds for $m-1$ : $$\begin{align}
\mathbb{P}(X_{n+m}=y|X_n =x) &= \sum_{k \in \mathcal{S}} \mathbb{P}(X_{n+m}=y|X_{n+m-1}=k,X_n =x) \mathbb{P}(X_{n+m-1}=k|X_n =x)\\
&= \sum_{k \in \mathcal{S}} \mathbb{P}(X_{n+m}=y|X_{n+m-1}=k) p^{m-1}(x,k)\\
&= \sum_{k \in \mathcal{S}} p^{m-1}(x,k)p(k,y)\\
&= p^m (x,y)
\end{align}$$
```ad-important
**Definition 2.2**: Chapman-Kolmogorov Equation

In our setting, Chapman-Kolmogorov Equation is the matrix multiplication $$p^{m+n}(x,y) = \sum_{k \in \mathcal{S}} p^m (x,k)p^n (k,y)$$
```

```ad-example: 
**Example: Gambler's Ruin, Revisited**

Recall the example and its transition matrix. Set $N=4$. Using computers, we can easily compute the $20$-th power of the transition matrix, which gives us the probability distributions after $20$ step starting at each state.

![[Pasted image 20250114110511.png|350]]

Notice that with very high probability the game ends with either $0$ or $4$. Later, we will show that the limit $p^{n}$ as $n \to \infty$ exists.
```

---
### Strong Markov Property
We defined Markov chain to be independent of the past, given $X_{n}$ for some $n$. In this section, we will see that we can **randomize** this time.

```ad-important
**Definition 2.3**: Stopping Time 

We say $T: \Omega \to \mathbb{N} \cup \{\infty\}$ is a stopping time with respect to $X_{n}$, if $\{T=n\}$ is a function of $\{X_{0},\cdots, X_{n}\}$.
```

```ad-important
**Definition 2.4**: Strong Markov Property
```

