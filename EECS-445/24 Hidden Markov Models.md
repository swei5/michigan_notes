[[2023-04-12]] #Markov #Probability 

### Markov Process
A method to model **sequential data**. Before this, we need to introduce the concept of **transition probabilities**.

In simple English, in determining the **next state** we **DON'T** care what the previous states were only what the **current state** is. The other states give **NO** additional information. In other words, $z_{t+1}$ is chosen according to the probability distribution associated with $z_{t}$.

```ad-example
In this example, let's suppose $\forall z_{t}, z_{t} \in \set{h_{1}, h_{2}}$, where $h_{i}$ is a given state (outcome) of $z$.

![[Pasted image 20230415213839.png|500]]
```

### Hidden Markov Model
Observations are correlated by **hidden states**.

```ad-important
**Definition 24.1**: Hidden Markov Model

Hidden Markov Models are a particular type of graphical model in which we assume that the data we observe come from a **sequence of hidden states**.

An HMM can be represented by the following model:

![[Pasted image 20230415214304.png|600]]

where $X$ are observed data, and $Z$ are hidden discrete random variables. We typically know the observations in the bottom row and want to estimate the corresponding states in the top row.

It encodes two **independence properties**:
1. Markov process(hiddenstates):future depends on past via the present
2. Each **observation** is **independent** of all other observations and states given the **associated state** (Each observation is associated with a state)
```

#### HMM Set-up
Hidden Markov Models are neatly consisted of three main components - **transition probabilities**, **emission probabilities**, and **starting state probabilities**.

```ad-important
**Definition 24.2**: HMM Set-up

We can define an HMM as a tuple $(H, O, \theta)$, where
- $H$ is the set of possible **hidden states**, $|H| = M$
- $O$ is the set of possible **observations**, $|O| = N$
- $\theta=\set{\pi, A, B}$, where
	- $A$ is an $M\times M$ matrix where $A(h_i,h_j) = P(z_{t+1} = h_j | z_t = h_i)$ is the probability of **transitioning** from state $h_i$ to $h_j$
	- $B$ is an $M \times N$ matrix where $B(h_i,o_l) = P(x_t = o_l | z_t = h_i)$ is the probability of **emitting** symbol $o_l$ from state $h_i$.
	- $\pi$ is an $M \times 1$ vector where $\pi (h_i) = P (z_1 = h_i)$ is the probability of **starting** in state $h_i$.

Given this, the probability of a sequence of observations and states in an HMM is: $$P(x_{1}, \cdots, x_{T}, z_{1},\cdots, z_{T}; \theta)=\pi (z_{1})\prod_{t=1}^{T-1} A(z_{t}, z_{t+1}) \prod_{t=1}^{T} B(z_{t},x_{t})$$
```

Note that $A, B, \pi$ are matrices, and $A(i,j)$ is simply indexing.

However, as per usual, we often **DO NOT** have access to hidden states, and we will have to make **inferences**.

---

#### Most Likely Hidden States
Suppose that we knew the model parameters $\theta$ for an HMM and observed an output sequence $x_1, \cdots, x_T$. Intuitively, we want to find a sequence of hidden states $z_{1}, \cdots, z_{T}$ that was most likely to generate this output sequence: $$\text{arg}\max_{z_{1}, \cdots, z_{T}}p(\mathbf{x}, \mathbf{z};\theta)$$Some practice questions are generous enough that we are only to calculate a limited number of combinations using brute force: 

```ad-example
![[Pasted image 20230415222101.png|600]]
![[Pasted image 20230415222111.png|600]]
```

However, note that this is a bad idea general given the time complexity of $O(M^T)$.

#### MLE with Complete Data
In other cases, we are given with complete **hidden states** and **observations**, and we need to infer the parameters.

This is very similar to what we've seen in [[23 Bayesian Networks#^684350|Bayesian Networks]].

```ad-important
**Definition 24.3**: MLE for Hidden Markov Model

The MLE for $\theta$ are $$\begin{align} \pi(v)&=\frac{n(z_{1}=v)}{\sum_{v' \in H} n(z_{1}=v')}\\
A(z,z')&= \frac{n(z\to z')}{\sum_{v \in H} n(z\to v)}\\
B(z,x)&= \frac{n(z\to x)}{\sum_{x' \in O} n(z\to x')}
\end{align}$$
```

---

### Viterbi Algorithm
Luckily, there is an efficient $O(TM^2)$ dynamic programming algorithm called the **Viterbi Algorithm** that solves this problem for us.

```ad-important
**Definition 24.4**: Viterbi Algorithm

We first define $$r(z_{1},\cdots, z_{k})=\prod_{t=1}^{k}A(z_{t-1},z_{t})\prod_{t=1}^{k} B(z_{t},x_{t})$$
where $A(z_{0},z_{1})=\pi(z_{1})$ and $$S(k,v)$$
is the set of **all sequences** of length $k$ that end in $z_k=v$, and $$\Psi(k,v)=\max_{S(k,v)} r(z_{1},\cdots,z_{k})$$
is the **most probable** sequence of length $k$ that ends with $z_k = v$.

With that, $\Psi(k,v)$ has the following recursive property: $$\Psi(k,v)=\max_{u\in H}\{\Psi(k-1,u)A(u,v)B(v,x_{k}) \}$$

with the base case $$\Psi(1,v)=\pi(v)B(v,x_{1})$$
```

Note that this is saying that the **most likely sequence** of length $k$ that ends with $z_k$ is simply the sequence which maximizes the probability of the best sequence of length $k − 1$ ending in **some state** $u$ times the **probability of transitioning** from $u$ to $v$ times the **probability of emitting** symbol $x_k$ in state $v$.

