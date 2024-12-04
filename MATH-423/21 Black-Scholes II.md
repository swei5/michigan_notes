[[2024-11-14]] #Valuation #Derivatives 

### SDE and Stochastic Processes
Last time we looked at the dynamics of the stock price $S$: ![[20 Black-Scholes I#^9c7323]]
The solution of the SDEs are stochastic processes: $$S_{t}=S_{0} \exp(\mu t+\sigma W_{t})$$ with a slight modification in the notations we're using thus far. 

In many cases we are given a process $Y_{t}$ without given its explicit form, but rather as a function of a known process (e.g. Brownian Motion). We need its **explicit form** to know more about the process, e.g., the distribution.

Now, assume $Y_{t}=f(t,W_{t})$, then $$Y_{t}=Y_{0}+\int_{0}^{t}dY_{s}=Y_{0}+\int_{0}^{t}df(s,W_{s})$$
We are going to learn how to compute $df(s,W_{s})$ for a given function $f$. 

Recall that a Brownian motion $W$ has the following properties: ![[20 Black-Scholes I#^50b971]]
Having in mind the simulations of the scaled random walk, we expect to see the paths of the Brownian Motion to be *very rough* or to *oscillate very abruptly* even in very small time intervals.

Mathematically speaking, two properties that jointly describe this behavior are the following
- The paths of Brownian Motion are **nowhere differentiable** and 
- The paths of Brownian Motion are **infinite variation**: $$\lim_{\max_{i}(t_{i+1}-t_{i})\to0} \sum\limits_{0=t_{0}<t_{1}<\cdots<t_{n}=T} |W_{t_{i+1}}-W_{t_{i}}|=+\infty$$
These properties do not allow us
- To apply the usual differential rules, i.e. $$df(t,x)=\frac{\partial f}{\partial t}(t,x)dt+ \frac{\partial f}{\partial x}(t,x) dx$$
- To construct the integral w.r.t. the Brownian Motion

Let us first try to calculate $\int_{0}^{t} W_{s}dW_{s}$: $$\begin{align}
\int_{0}^{t} W_{s}dW_{s} &= \lim_{n \to \infty} \sum\limits_{i=1}^{n} W_{t_{i-1}}(W_{t_{i}}-W_{t_{i-1}})\\

&= \lim_{n \to \infty} \frac{1}{2} \left(\sum\limits_{i=1}^{n} W_{t_{i}}^{2}-W_{t_{i-1}}^{2}\right) + \frac{1}{2}  \left(\sum\limits_{i=1}^{n} (W_{t_{i}}-W_{t_{i-1}})^{2}\right)  \\
&\approx \frac{1}{2}(W_{t_{n}}^{2}-W_{t_{0}}^{2})- \frac{1}{2}\left(\sum dt\right) & \text{ from quadratic variation}
\end{align}$$
Thus, $$W_{t}^{2}=t+2 \int_{0}^{t}W_{s}dW_{s} \implies dW_{t}^{2}=dt+2W_{t}dW_{t}$$

---
### Itô's Formula

```ad-important
**Definition 21.1**: Itô's Formula

For every $f(t,x) \in C^{1,2} (\mathbb{R}^{+}, \mathbb{R})$, i.e., continuously differentiable in the time variable $t$ and twice continuously differentiable in the space variable $x$, the following **Itô's formula** holds: $$df(t,W_{t})= \frac{\partial f}{\partial t}(t,W_{t})dt + \frac{\partial f}{\partial x}(t, W_{t})dW_{t} + \frac{1}{2} \frac{\partial^{2}f}{\partial x^{2}}(t,W_{t})dt$$

The first and the third terms correspond to the familiar Riemann integrals seen above.

The second term corresponds to the Stochastic Integral w.r.t. the Brownian Motion.
```

The third term is called the **Itô's correction**, which arises because Brownian motion $W_{t}$ has non-zero quadratic variation ($dW^{2}_{t}=dt+2W_{t}dW_{t}$). The correction is deterministic because it comes from the second derivative and represents how the variance (the *roughness* of $W_{t}$) impacts $f$.

```ad-example
**Example**: Applying Itô to $\int_{0}^{t} W_{s}dW_{s}$

Let $Y_{t}=W_{t}^{2}$. Then, $$\begin{align}
dY_{t}&= \frac{\partial }{\partial t}(W_{t}^{2})dt + \frac{\partial }{\partial W_{t}}(W_{t}^{2})dW_{t} + \frac{1}{2} \frac{\partial^{2}}{\partial W_{t}^{2}}(W_{t}^{2})dt\\
&= 0 + 2W_{t}dW_{t} +1 dt
\end{align}$$

Thus, $$\begin{align}
W_{t}^{2}&=\int_{0}^{t} 2W_{s}dW_{s} + \int_{0}^{t} 1 ds\\
&\implies \int_{0}^{t} W_{s}dW_{s} = \frac{1}{2}W_{t}^{2}+ \frac{1}{2}
\end{align} $$

The exact solution we got from the earlier computation.

Note that in differentiating, we treat $W_{t}$ as fixed w.r.t. $t$.
```

```ad-important
**Definition 21.2**: Martingality of Stochastic Integrals

Assume that the function $g(t,x)$ does not grow faster than the **exponential function**. Then, the process of stochastic integrals **with respect to the Brownian Motion**, i.e. $$\int_{0}^{t} g(s,W_{s})dW_{s}, t \in \mathbb{R}^{+}$$

is an $\mathbb{F}^{W}$-martingale.

This implies that
- Its expected value at any time $t$ equals its **initial value** (which is typically zero)
- It exhibits no *drift* — its future value is purely determined by the **stochastic noise**
```

^a962f7

Here the filtration $\mathbb{F}^{W}:=(\mathcal{F}_{t}^{W})_{t\in \mathbb{R}^{+}}$ means the **information we can collect** from the Brownian Motion up to time $t$. 

In other words, we have that $$\mathbb{E}\left[\int_{0}^{t} g(s,W_{s})dW_{s}\right]=\int_{0}^{0}g(s,W_{s})dW_{s}=0, t \in \mathbb{R}^{+}$$
```ad-example
**Example**: Solving for $\mathbb{E}(W_{t}^{4})$ using Martingality

We want to solve for $\mathbb{E}(W_{t}^{4})$. Applying Itô's formula, we have $$d(W_{t}^{4})=4W_{t}^{3} dW_{t}+12W_{t}^{2}dt$$

Since the expectation of the stochastic integral is zero, this is $$\begin{align}
W_{t}^{4}&= \int_{0}^{t}4W_{s}^{3} dW_{s} + \int_{0}^{t} 12W_{s}^{2}ds\\
\mathbb{E}(W_{t}^{4}) &= \mathbb{E}\left(\int_{0}^{t}4W_{s}^{3} dW_{s}\right) + \mathbb{E}\left(\int_{0}^{t} 12W_{s}^{2}ds\right) \\
&= 12 \int_{0}^{t} \mathbb{E}(W_{s}^{2})ds\\
&= 12 \int_{0}^{t}\mathbb{E}\left(\int_{0}^{t} 2W_{k}dW_{k} + \int_{0}^{t} 1 dk\right)ds\\
&= 12 \int_{0}^{t}\mathbb{E}\left(\int_{0}^{t} 1 dk\right) ds\\
&= 12 \frac{t^{2}}{4}\\
&= 3t^{2}
\end{align}$$

We would've arrived at the same conclusion by leveraging properties of moments of a normal random variable ($W_{t}$).
```

Recall that in Lecture 20 we have re-written the binomial model as ![[20 Black-Scholes I#^3cfe2f]]
We can adapt this to the stochastic function we've studied throughout $f(t,w_{N}(t))$, where $f (t, x):= S (0)\exp(\mu t+\sigma x)$ for $t \in \mathbb{R}^{+}$ and $x \in \mathbb{R}$. Here, $\mu$ is the mean value of the **log returns** under the measure $\mathbb{P}$.

We can derive the limit-model (Black-Scholes) in the form of $$dS(t)= S(t)\left(\mu+ \frac{1}{2}\sigma^{2}\right)dt + \sigma S(t)dW(t)$$
```ad-example
**Example**: Applying Itô in B-S 

Applying Itô's formula to $f (t, x):= S (0)\exp(\mu t+\sigma x)$, we have $$\begin{align}\\
df(t,W_{t})&= \frac{\partial f}{\partial t}(t,W_{t})dt + \frac{\partial f}{\partial W_{t}}(t,W_{t})dW_{t} + \frac{1}{2} \frac{\partial^{2} f}{\partial W_{t}^{2}}(t,W_{t})dt\\
&= S(0)\mu \exp(\mu t + \sigma W_{t}) dt + S(0)\sigma \exp(\mu t + \sigma W_{t}) dW_{t} + \frac{1}{2} S(0) \exp(\mu t + \sigma W_{t}) \sigma^{2} dt\\
&= S(0) \exp(\mu t + \sigma W_{t}) \left[\left(\mu + \frac{1}{2}\sigma^{2}\right)dt + \sigma dW_{t}\right]
\end{align}$$

It can thus be proven that $$S(t)=S(0) \exp(\mu t+\sigma W(t))$$ is the unique solution the problem.
```

^7a3abc

#### Itô's Formula for Process $X$
When the underlying process is not a Brownian motion, but a **diffusion process** $X$, we can still use Ito’s formula to write down the dynamics of $f(t, X_{t})$.

```ad-important
**Definition 21.3**: Itô's Formula, Extended

Assume the process $X$ has the following dynamics: $$dX_{t}=\mu_{t} dt+\sigma_{t}dW_{t}, X_{0}=x_{0}$$

Then, for every $f(t,x) \in C^{1,2}(\mathbb{R}^{+}, \mathbb{R})$, i.e., continuously differentiable in the time variable $t$ and twice continuously differentiable in the space variable $x$, the following Itô's formula holds: $$\begin{align}df(t,X_{t})&= \frac{\partial f}{\partial t}dt + \frac{\partial f}{\partial x}dX_{t} + \frac{1}{2} \frac{\partial ^{2} f}{\partial x^{2}} dX_{t}^{2}\\
&= \frac{\partial f}{\partial t}dt + \frac{\partial f}{\partial x}(\mu_{t} dt+\sigma_{t}dW_{t})+\frac{1}{2} \frac{\partial ^{2} f}{\partial x^{2}}(\mu_{t} dt+\sigma_{t}dW_{t})\\
&= \left(\frac{\partial f(t,X_{t})}{\partial t} + \mu_{t} \frac{\partial f(t,X_{t})}{\partial x} + \frac{1}{2} \sigma_{t}^{2} \frac{\partial^{2} f(t,X_{t})}{\partial x^{2}}\right)dt + \sigma_{t} \frac{\partial f(t,X_{t})}{\partial x} dW_{t}
\end{align}$$ since as $dt \to 0, dt^{2}$ and $dt dW_{t}$ will tend to zero faster than $dt$ and $dW_{t}^{2}$ and $(dX_{t})^{2}=dt$ is calculated using the following rule:
1. $(dt)^{2}=0$
2. $dt dW_{t} = 0$
3. $(dW_{t})^{2}=dt$
```

---
### Extras
- [Variation of Brownian Motion](https://uregina.ca/~kozdron/Teaching/Regina/862Winter06/Handouts/quad_var_cor.pdf)
- [Ito Integrals](https://math.nyu.edu/~goodman/teaching/DerivSec10/notes/week6.pdf)
- [Derivation of BSM](https://dergipark.org.tr/en/download/article-file/1838280)

```ad-note
**Black-Scholes Equation (B-S PDE)**

If $S$ is a stock price that follows an Itô's Process, then the value of an option, $f$, of $S$ is quantified by the following equation: $$\frac{\partial f}{\partial t}+r S \frac{\partial f}{\partial S}+ \frac{1}{2} \frac{\partial^{2} f}{\partial x^{2}}\sigma^{2}S^{2}=rf$$

To show this, suppose $f$ is a twice differentiable function of the price of a call option or other derivative contingent on $S$. Since $S$ follows an Itô's Process, $S$ has a Itô's differential equation of $$dS=\mu Sdt + \sigma S dz$$

Using Itô's Lemma we can write $df$ as $$df=\left(\mu S \frac{\partial f}{\partial S}+\frac{\partial f}{\partial t} + \frac{1}{2} \frac{\partial ^{2} f}{\partial S^{2}} \sigma^{2}S^{2}\right)dt + \frac{\partial f}{\partial S}\sigma S dz$$

Notice how $\mu$ and $\sigma$ are no longer contingent on $t$, but are contingent on $S$. We can write the above discretely over a time period $\Delta t$ as $$\Delta S = \mu S \Delta t+ \sigma S \Delta z$$ and $$\Delta f = \left(\mu S \frac{\partial f}{\partial S}+\frac{\partial f}{\partial t} + \frac{1}{2} \frac{\partial ^{2} f}{\partial S^{2}} \sigma^{2}S^{2}\right)\Delta t+ \frac{\partial f}{\partial S}\sigma S \Delta z$$

Now $S$ and $f$ both follow the same Itô's Process. Thus, if we select a portfolio, we can eliminate the Itô's Process and effectively price an option. The portfolio we select will consist of short $1$ derivative and long $\frac{\delta f}{\delta S}$ shares of stock. It will become clear shortly why we select this portfolio.

We define $Pi$ as the value of our portfolio. By definition, $$\Pi = -f + \frac{\partial f}{\partial S} S$$, which takes on the discrete version of $$\Delta \Pi = -\Delta f + \frac{\partial f}{\partial S} \Delta S$$

Subbing in $\Delta f$ and $\Delta S$ that were calculated above, we get $$\Delta \Pi = -\frac{\partial f}{\partial t} \Delta t - \frac{1}{2} \frac{\partial^{2} f}{\partial S^{2}} \sigma^{2} S^{2} \Delta t$$ Factoring out a $\Delta t$, $$\Delta \Pi = \left(-\frac{\partial f}{\partial t}- \frac{1}{2} \frac{\partial^{2} f}{\partial S^{2}} \sigma^{2} S^{2}\right)\Delta t$$ 

Without a $\Delta z$ term (stochastic variable), this portfolio is effectively **riskless** during the time period $\Delta t$. Since there are **no arbitrage opportunities**, security trading is continuous and all securities **share the same short term constant interest rate**, our portfolio we have created will earn instantaneous rates of return over the time period $\Delta t$. Thus we can write $\Delta \Pi$ as $$\Delta \Pi = r\Pi \Delta t$$ 
Then by combining the above we have $$\left(-\frac{\partial f}{\partial t}- \frac{1}{2} \frac{\partial^{2} f}{\partial S^{2}} \sigma^{2} S^{2}\right)\Delta t= r\left(-f + \frac{\partial f}{\partial S} S\right)\Delta t$$
```
