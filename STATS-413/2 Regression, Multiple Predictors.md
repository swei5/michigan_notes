[[2024-08-28]] #Regression 

In general, our responses $y_i$ might be thought of as functions of $p > 1$ explanatory variables

We’d like to fit a function of the form:
$$\hat{y}_{i}=\hat{\beta}_{0}+\hat{\beta}_{1}x_{i1}+\hat{\beta}_{2}x_{i2}+\cdots+\hat{\beta}_{p}x_{ip}$$
Similar to how we solve the optimization problem with one predictor variable: ![[STATS-413/1 Intro#^302e2f|1 Intro]]
We may extend this to multiple dimensions, and hence we would come up with a similar optimization problem: 

The **least squares coefficients** $(\beta_{0},\beta_{1}, \cdots, \beta_{p})$ to minimize the sum of the squared errors (SSE):
$$(\hat{\beta_{0}},\hat{\beta}_{1}, \cdots, \hat{\beta}_{p})=\text{arg min}_{\beta_{0}, \beta_{1}, \cdots, \beta_{p}} \sum\limits_{i}^{n}(y_{i}-(\beta_{0}+\beta_{1}x_{i1}+\cdots + \beta_{p}x_{ip}))^{2}$$

### 