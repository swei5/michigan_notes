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
&\approx \frac{1}{2}(W_{t_{n}}^{2}-W_{t_{0}}^{2})- \frac{1}{2}\left(\sum dt\right)
\end{align}$$
Thus, $$W_{t}^{2}=t+2 \int_{0}^{t}W_{s}dW_{s} \implies dW_{t}^{2}=dt+2W_{t}dW_{t}$$

```ad-important
**Definition 21.1**: Itô's Formula

For every $f(t,x) \in C^{1,2} (\mathbb{R}^{+}, \mathbb{R})$, i.e., continuously differentiable in the time variable $t$ and twice continuously differentiable in the space variable $x$, the following **Itô's formula** holds: $$df(t,W_{t})= \frac{\partial f}{\partial t}(t,W_{t})dt + \frac{\partial f}{\partial x}(t, W_{t})dW_{t} + \frac{1}{2} \frac{\partial^{2}f}{\partial x^{2}}(t,W_{t})dt$$

The first and the third terms correspond to the familiar Riemann integrals seen above.

The second term corresponds to the Stochastic Integral w.r.t. the Brownian Motion.
```
