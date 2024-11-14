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

![[Pasted image 20241110204106.png|400]]

In comparison to polynomials (blue), spline (red) is better in accounting for curvature.

By far, the most common type of regression spline is a **cubic spline**.
- Additional degrees result in additional smoothness that is hard to discern by the naked eye, but can result in **instability**

Knot points $K$ are the primary way that complexity is controlled. It should be chosen with **out-of-sample** performance in mind. Typically (but not always), cubic regression splines outperform polynomial regression.
- Increasing complexity by increasing the **number of knots** (splines) results in **more STABLE** functions than simply increasing polynomial degree

#### Basis Functions
Both polynomial regression and regression splines are a general class of regression methods that utilize **basis functions**. The general idea is to have at hand (pre-specified) a **family of functions or transformations** that can be applied to a predictor $x$: $\{b_{1}(x), \cdots, b_{r}(x)\}$. 

For example, the polynomial regression of degree $r$ that takes the form $$y_{i}=\beta_{0}+\beta_{1}x_{i}+\beta_{2}x_{i}^{2}+\cdots+\beta_{r}x_{i}^{r}+\varepsilon$$ can be written as $$y_{i}=\beta_{0}+\beta_{1}b_{1}(x_{i})+\beta_{2}b_{2}(x_{i})+\cdots+\beta_{r}b_{r}(x_{i})+\varepsilon$$ where $$b_{j}(x_{i})=x_{i}^{j}$$
Once we choose the basis function you can just use OLS to estimate $\beta$.

```ad-example
**Example**: Regression Splines Represented Using Basis Functions

We can also represent a cubic spline with $K$ knots using the basis function representation: $$y_{i}=\beta_{0}+\beta_{1}b_{1}(x_{i})+\beta_{2}b_{2}(x_{i})+\cdots+\beta_{K+3}b_{K+3}(x_{i})+\varepsilon$$ where $b_{0}(x)=1, b_{1}(x)=x, b_{2}(x)=x^{2}, b_{3}(x)=x^{3}$ and $b_{j+3}(x)=(x-\xi_{j})^{3}_{+}$ for $j=1,\cdots, K$. 

This is (roughly) what’s occurring underneath the hood when using the `bs` command.
```

In sum, regression splines overcome one of the main limitations of polynomial regression - fitting local piecewise polynomials of **small degree**, rather than a single polynomial providing a global fit.

That said, however, regression splines can still exhibit instability at the **extremes of the distribution** of $x$. In other words, prediction equation has large variance when $x$ is the near the the minimum or maximum value for the variable - this is called a **boundary effect**, and is common with many nonlinear methods.

---
### Natural Splines
Natural splines are a slight modification of **CUBIC** regression splines that improve **stability at the boundary**.

If we have $K$ knots:
- Still piecewise cubic in all $K-1$ interior regions (between knot points)
- Also **piecewise cubic** between $\min(x)$ and the smallest knot point, and between the largest knot point and $\max(x)$
- Further enforce prediction function becomes **LINEAR** once it goes outside the range of $x=[\min(x), \max(x)]$

![[Pasted image 20241110211615.png|500]]

We also enforce the **continuity** of the piecewise functions and the first/second derivatives.
- This means the prediction function has to have a second derivative equal to zero at the **boundary**

This results in a total of $K+2$ parameters to choose from. Think $K+1 =p$ in regression terms, plus an intercept.

```ad-note
In simple terms, **forcing the function to be linear when extrapolating** is where natural splines differ from cubic regression splines.
```

The reason why natural splines improve stability at the boundary include
- Cubic function is more complex than a line
- Complexity in an estimated function results in additional variability
	- Enforcing linear behavior at the boundary introduces more stability, though potentially introducing more bias

In `R`, we use the `ns()` command to help fit natural splines. 

```r
library(splines)
lm(y~ns(x, knots = knotlocation, degree = d))
# Or, if we rather have the knots placed at quantiles of x
lm(y~ns(x, df = K+1)) # df = K+1 as we are ignoring the intercept
```

![[Pasted image 20241110212601.png|400]]

So far, our discussion has focused on a single predictor variable. In general, we'd be interested in fitting functions of the form $$y_{i}=f(x_{i1}, \cdots, x_{ip})+\varepsilon_{i}$$ where $f$ is unknown. We've discussed polynomial regression in [[17 Sample Splitting#Polynomials in Several Predictors|last lecture]] as a way to approximate this. However, this is quite difficult in general without making further assumptions about the functional form for $f(\cdot)$.

---
### Generalized Additive Models
When we have several predictors and want to achieve a non-linear fit by extending univariate methods, a natural way to extend the multiple linear regression model $$y_{i}=\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip}+\varepsilon_{i}$$ is to replace each linear part $\beta_{j}x_{ij}$ with $g_{j}(x_{ij})$, where $g_{j}$ is some **smooth nonlinear** function. We would then write the model as $$y_{i}=\beta_0+g_{1}(x_{i1})+\cdots+g_{p}(x_{ip})+\varepsilon_{i}$$
Here, we **MUST assume** **NO interactions** of any order between nonlinear functions of predictor variables, i.e. nothing of the form $h_{1}(x_{i1},x_{i2})$ and so on.

This model is an example of a **Generalized Additive Model** (GAM). It is called an additive model because we calculate a separate $g_{j}$ for each $x_{j}$ and then **add them all together**.

In general, we consider representing each $g_{j}(x_{j})$ through a **flexible basis expansion**.
- Let $g_{j}(\cdot)$ be polynomials of a specific degree in $x_{j}$
- Or, perhaps the most common approach is to let $g_{j}(\cdot)$ be a spline in $x_{j}$

An example in codes is as follows:

```r
gam.ns = lm(medv~ns(crim, df=5+1) + ns(indus, df=5+1)+ ns(rm, df=5+1) + ns(lstat, df=5+1), data=train.boston)
```

```ad-summary
**GAMs, Pros and Cons**
- **Pros**
	- By allowing one to fit a non-linear $g_{j}$ to each $x_{j}$ we can automatically model non-linear relationships that standard linear regression will miss
	- Fitting an additive model we can still examine the effect of each $x_{j}$ on $y$ individually while holding all the other $x$ fixed
	- Potentially more accurate predictions
- **Cons**
	- Restricted to be additive in $g_{j}(x_{j})$
	- Our fit is not completely non-linear
	- Interactions can’t automatically be modeled
```
