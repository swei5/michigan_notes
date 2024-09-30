[[2024-09-26]]  #Regression

### Recap: Importance of LR Assumptions
Consider our test statistic for testing the null $H_{0}:\beta_{j}=\gamma_{0}$ with a two-sided alternative: ![[4 Hypothesis Tests#^cecd4f]]
Here **linearity** plays a role in asserting the **unbiasedness** of $\hat{\beta}$ and thus the center of the distribution. **Homoskedasticity** plays a role in establishing constant $\hat{\sigma}_{\epsilon}^{2}$ and therefore the standard error. **Normality** plays a role in forming the $T$ distribution.

In addition to our suite of regression diagnostics assessing linearity, homoskedasticity, and normality, we’ll introduce additional diagnostics to help us **flag potentially unusual / important observations** in our data set.
- **Outliers** - observations do not behave like the bulk of our data points
- **Influential points** - observations which unduly/substantially affect the prediction equation

A data point could be none, one, or both of these.

### Outliers
Informally, an outlier is an observation that differs substantially from the other points in the data set.
- Mistakes
- A rare, but possible, outcome in our population

Including an **additional predictor variable** can also reveal outliers that weren’t apparent before.