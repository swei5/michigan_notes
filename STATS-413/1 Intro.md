[[2024-08-26]] #Regression

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

### Linear Association
The points in the scatterplot appear to be clustered about a line when a relationship is **linear**.
- Stronger relationship means **TIGHTER cluster** about the line (elliptical shape cloud along the line), and vice versa
- Positive and Negative relationships possible

A corresponding quantitative measure of the linear relationship is a **linear equation**.

#### $z$ -Score
For an observation $x_i$ of a variable $x$, the z-score of $x_i$, $z (x_i)$, answers the following question:
- How many standard deviations above (+) or below (-) the mean was the observed value $x_i$?

Inversely, we can write observation $x_{i}$ for the $i$ th individual as the mean, plus some number times the standard deviation:
$$x_{i}=\overline{x}+z(x_{i})\sigma(x)$$
Where
$$z(x_{i})=\frac{x_{i}-\overline{x}}{\sigma(x)}$$
If we form $z$ -scores of any continuous variable, $x$, then we have:
- $\overline{z(x)}=0$
- $\sigma(z(x))=1$

Now, suppose I have a variable $x$, and I change units by $x^\star = a + bx$:
- If $b>0$: 
	- $z(x^\star)=z(x)$
- If $b<0$:
	- $z(x^\star)=-z(x)$

That is, the magnitude of $z$-scores are not affected by **additive and multiplicative shifts** (but the sign flips if $b$ negative).

#### Correlation
We define correlation by taking $z$ -scores of the $y$ and $x$ variables separately.