[[2024-08-26]] #Regression #R

Our main goal is to use **data** to infer relationships between a single response variable and one or more other predictor variables.
- Then, to improve predictions of our response variable by leveraging information contained in the predictors (functions)

In regression analysis, we seek to predict values of the **response variable** (outcome variable, $y$) as a function of one or more **predictor variables** (covariates, $x$). When performing a regression, we say that we **regress** the values of the $y$ variable on the values of the $x$ variables.

There are three primary questions to be addressed in regression:
- **Point estimation**: investigate quantitative questions about the **parameters of a regression model**
	- E.g. the line of the best fit
- **Statistical inference**: **quantify uncertainty** about our estimates, and assess what the data can tell us about population parameters
	- Due to the limitation of the scope of data we have, we never have full access to the entire population 
	- E.g. confidence interval, hypothesis test
- **Prediction**: predict the value of the response
	- Point estimates, or estimating an *interval* to capture the variability

### Regression Analysis
Out goal is to **infer population-level relationships** between a response variable (denoted by $Y$) and **one or more** other explanatory variables (denoted by $X$) from data.

Data are formed in value tuples like $(x_1, y_1),\dots, (x_n, y_n)$ of $(X, Y)$ observed on each of $n$ units or cases. Here,
- $x_{i} \in \mathbb{R}^p$ is the predictor variable
- $y_{i}\in \mathbb{R}$ is the response variable
- $n$ is sample size

In tabular forms, this looks like

![[Pasted image 20240827220759.png|400]]

---
### Linear Association
The points in the scatterplot appear to be clustered about a line when a relationship is **linear**.
- Stronger relationship means **TIGHTER cluster** about the line (elliptical shape cloud along the line), and vice versa
- Positive and Negative relationships possible

A corresponding quantitative measure of the linear relationship is a **linear equation**.

#### $z$ -Score
For an observation $x_i$ of a variable $x$, the z-score of $x_i$, $z (x_i)$, answers the following question:
- How many standard deviations above (+) or below (-) the mean was the observed value $x_i$?

Inversely, we can write observation $x_{i}$ for the $i$ th individual as the mean, plus some number times the standard deviation:
$$x_{i}=\overline{x}+z(x_{i})\text{ stdev}(x)$$
Where
$$z(x_{i})=\frac{x_{i}-\overline{x}}{\text{stdev}(x)}$$
If we form $z$ -scores of any continuous variable, $x$, then we have:
- $\overline{z(x)}=0$
- $\text{stdev}(z(x))=1$

Now, suppose I have a variable $x$, and I change units by $x^\star = a + bx$:
- If $b>0$: 
	- $z(x^\star)=z(x)$
- If $b<0$:
	- $z(x^\star)=-z(x)$

That is, the magnitude of $z$-scores are not affected by **additive and multiplicative shifts** (but the sign flips if $b$ negative).

#### Correlation
We define correlation by taking $z$ -scores of the $y$ and $x$ variables separately. Then, the correlation coefficient $y,x$ is
$$r(x,y)=\frac{1}{n-1} \sum\limits_{i=1}^{n}z(x_{i})z(y_{i})$$
In other words, the correlation is the **average** of the products of the $z$ -scores.

```ad-info
**Properties of Correlation**

- $-1 \le r(x,y) \le 1$
- **Unit free** - consequence of taking product of z-scores
- $r(x,y)=r(y,x)$
- Larger $|r(x,y)|$, stronger the correlation

**Caveats of Correlation**
- Only tells us about overall **linear** relationship
- Heavily influenced by **outliers**
	- Have to plot to check for outliers and check for a nonlinear relationship
```

#### Regression Effect 
Given two correlated variables, if one variable is observed to be extreme, then you can expect the other variable to also be extreme – *just not as extreme*. 

```ad-important
**Definition 1.1**: $z$-Score Regression 

Suppose we know that two variables have a linear relationship with correlation $r$. Suppose we want to predict the $y$-value of an individual whose $x$-value had a $z$-score of $z(x)$. Our, prediction for the $z$-score of the $y$ value, $z(\hat{y})$, can be found as:
$$z(\hat{y}) = r\cdot z(x)$$
```

In other words, the **amount of regression** towards the mean is decided by the coefficient of correlation.

^38eb8a

```ad-important
**Definition 1.2**: Regression Equation

If we rewrite [[#^38eb8a|Definition 1.1]], we have that
$$\begin{align}z(\hat{y})&=r \cdot z(x)\\ \frac{\hat{y}-\overline{y}}{\text{stdev}(y)}&= r\cdot \frac{x-\overline{x}}{\text{stdev}({x})}\\ \hat{y} &= r \frac{\text{stdev}({y})}{\text{stdev}(x)}x+\overline{y}-r\frac{\text{stdev}({y})}{\text{stdev}(x)}\overline{x} \\ \hat{y}&=\hat{\beta}_{0}+\hat{\beta}_{1}x \end{align}$$
Where $\hat{\beta}_{1}=r \frac{\text{stdev}({y})}{\text{stdev}(x)}$ and $\hat{\beta}_{0}=\overline{y}-\hat{\beta}_{1}\overline{x}$. 

Here, $\hat{\beta}_{1}$ is the **slope** of the regression line and $\hat{\beta}_{0}$ is the **intercept**.
```

^70b481

This line is commonly referred to as **the least squares line**, or the **least squares regression equation**.

There are infinitely many equations of the form $\tilde{\beta_0}+\tilde{\beta_1}x$ we could have chosen... This boils down to an optimization problem.

For case $i , i = 1, 2, \dots, n$, let the $i$ th **residual** term be the difference between the observed response variable and the predicted value for the $i$ th individual for a given value of $(\tilde{\beta_0},\tilde{\beta_{1}})$:
$$e_{i}=y_{i}-(\tilde{\beta_0}+\tilde{\beta_1}x)$$
This is the **vertical** difference between the observation and line at $x_{i}$ with the choices of intercept and slope $(\beta_{0},\beta_{1})$. We would like $\varepsilon_{i}$ to be as small as possible for better prediction.

The **least squares regression** line chooses $(\tilde{\beta_0},\tilde{\beta_1})$ to minimize the **sum of the squared errors** (SSE):
$$(\hat{\beta_{0}},\hat{\beta}_{1})=\text{arg min}_{\tilde{\beta_0}, \tilde{\beta_1}} \sum\limits_{i}^{n}(y_{i}-(\tilde{\beta_0}+\tilde{\beta_1}x_{i}))^{2}$$
The reason for the squaring errors are for: ^302e2f
- Ease of optimization (taking derivatives)
- Optimality properties of resulting estimators under additional modeling assumptions

`R` computes the least squares equation for us using the `lm` command - which has the same optimal solutions of $\hat{\beta}_{0}, \hat{\beta}_{1}$ derived from $z$ -score regression intuition.

This shows that the **least squares optimization problem** produces a prediction equation that correctly **accounts for the regression effect**.

---
### Covered `R` Functions 

```r
# To read from a csv file
fatherson = read.csv("fatherson.csv")

# To read from columns in a df
father = fatherson$Father.Height

# Histogram
hist(father, main= "Histogram of Father's Heights", xlab = "Fathers")

# Boxplot
boxplot(father, son, names = c("Fathers", "Sons"))

# Scatter plot
plot(father, son, main = "Father's and Son's Heights", xlab = "Father's Height (in)", ylab = "Son's Height (in)", pch=16)

# Mean, Stdev, Length, Sum
mean(zfather)
sd(zfather)
n = length(son)
sum(zfather*zson)

# Correlation 
cor(zfather, zson)

#show scatterplot with regression line included
plot(father, son, pch = 16)
abline(sonreg)
```