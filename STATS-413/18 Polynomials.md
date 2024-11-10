[[2024-11-05]] #Regression #R 

### Piecewise Polynomials
As a means of capturing nonlinear trends, polynomials suffer from a few important deficiencies
- Only way to fit increasingly complex functions is to increase the polynomial degree
- Individual observations **affect the prediction function globally**
- Prone to being influenced by **outliers**
- Potentials for instability
- Behavior at the boundary highly unstable

Each data point **affects the fit globally**. Polynomial needs to trade-off performance in the **center and the extremes**, even with a large degree.

![[Pasted image 20241109211002.png|400]]

Sometimes the nonlinear pattern can change substantially **across different regions** of $x$. Should this occur, a single polynomial fit would likely be inadequate:
- Low polynomial degree = bias may remain
- High polynomial degree = too much variance

Rather than simply fitting a single polynomial that provides predictions at all points $x$, we might instead consider **fitting different polynomials (each of low degree) for different ranges** of $x$. For instance: $$y_{i}=\begin{cases}
\beta_{01}+\beta_{11}x+\beta_{21}x^{2}+\beta_{31}x^{3}+\varepsilon_{i} & x \le \xi_{1} \\
\beta_{02}+\beta_{12}x+\beta_{22}x^{2}+\beta_{32}x^{3}+\varepsilon_{i} & \xi_{1} <x\le \xi_{2} \\
\beta_{03}+\beta_{13}x+\beta_{23}x^{2}+\beta_{33}x^{3}+\varepsilon_{i} & \xi_{2} <x\le \xi_{3} \\
\beta_{04}+\beta_{14}x+\beta_{24}x^{2}+\beta_{34}x^{3}+\varepsilon_{i} & x > \xi_3
\end{cases}$$
The drawbacks of piecewise polynomials are readily apparent once we look at the prediction equation: 
- **Discontinuity** at the boundaries of the regions we have defined, at $\{\xi_{k}\}$
- But we generally imagine the function we are estimating is continuous over the domain of $x$

While piecewise polynomials are rarely deployed in practice, they motivate a family of commonly used methods known as **splines**.
- Based on piecewise polynomials, while imposing constraints to ensure continuity and smoothness

---
### Regression Splines
Instead of fitting a high dimensional polynomial over the entire range of $x$, a regression spline works by fitting different **low dimensional polynomials** over different regions of $x$ (just like the piecewise polynomial).

The $K$ points where the coefficients change, $\xi_{1},\cdots, \xi_{K}$, are called the **knots** (also referred to as breakpoints) of the spline.
- The **more knots** that are used, the more **flexible** the spline is
- The **higher the polynomial** degree for each region of $x$, the more **flexible** the spline
- Flexible is not always better!

Unlike with piecewise polynomials, splines are constructed using piecewise polynomials of degree $d$ such that the resulting fit is smooth at the knots $\xi_{1},\cdots, \xi_{K}$. Suppose we fit a regression spline using a degree $d$ polynomial, then at each knot $\xi_k$, the function will be **continuous** at $x=\xi_k$. 

In addition, **ALL** derivatives $f^{(1)}, \cdots, f^{(d-1)}$ will also be continuous functions at $x=\xi_{k}$ for $k=1, \cdots, K$. Continuity of derivatives provides **additional smoothness**.

![[Pasted image 20241109220705.png|400]]

#### Free Parameters
For a cubic spline with $K=3$ knots, it may appear as though there are $4 \times 4$ free parameters to choose.
- However, there are $K=3$ knots, resulting in $4$ regions
- Each region gets its **own polynomial** - this involves an intercept, plus coefficients on the linear, square, and cubed terms: $d+1=4$ parameters per region

However, our **continuity and smoothness** requirements impose constraints:
- For each knot, there are $d$ constraints - one for continuity, $d-1$ for smoothness
- Total constraints across knots: $K \times d$, or $3 \times 3$ in this example

```ad-important
**Definition 18.1**: Free Parameters for Regression Splines

More generally, for a degree $d$ spline with $K$ knots, the number of parameters we get to choose are $$\begin{align}
df &= (K+1)(d+1)-Kd\\
&= K+d+1
\end{align}$$
```

Think of $K + d$ as the analogy to $p$ in the usual regression context, with an **additional term for the intercept**.

The location of the knots $\xi_{1},\cdots, \xi_{K}$ can be pre-specified by the user if there are known areas in the domain of $x$ that represent **different regimes** due to the context of the problem.

If we don’t have such knowledge, and simply desire to fit a spline with $K$ knots, two natural defaults arise in practice.
- **Equi-spaced** knot points $\xi_{k}$, so that the knot points are the same distance apart
- Knot points based on quantiles of $x$, so that each of the $K + 1$ resulting regions contains the same fraction of observations in the data (this is **most common**, and is built into `R`)

In `R`, functions for splines are available in the splines package. We use the `bs` function to help fit regression splines. For a degree $d$ regression spline with $K$ knot locations stored in an object called `knotlocation`.

```r
library(splines)
lm(y~bs(x, knots = knotlocation, degree = d)) # We will define knotlocation

# Or, if we rather have the knots placed at quantiles of x
lm(y~bs(x, df = d+K, degree = d)) # df = d+K as we are ignoring the intercept
```

