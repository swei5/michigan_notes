[[2024-11-14]] #Regression

### Cross-Validation
The goal of cross-validation is to test the model’s ability to predict new data that were not used in estimating it, in order to flag problems like overfitting and to give an insight on how the model will **generalize to an independent dataset**. However, we’d like to do this in a way that isn’t overly sacrificial of training data.

```ad-important
**Definition 21.1**: $k$-fold Cross-Validation

We first partition the data into a training set and test set.
1. Given the training set, divide it into $k$ pieces (folds)
2. For fold $j$, use the other $k-1$ folds to **construct a model**, and **test that model on the held-out fold** $j$ (validation set) for a candidate hyperparameter value
3. Repeat this for each of the $k$ folds
4. For the candidate parameter value, compute the **average of the out-of-sample prediction error** estimates over the $k$ folds ($k$ validation sets)
5. Repeat for different values of the hyperparameter and choose the final parameter value that optimizes the model quality
6. Fit the predictive model to **all of training data** choosing the optimal parameter value for cross-validation step, and then **evaluate performance on the test set**
```

This uses the **entire training set to fit** the model, yet in choosing the tuning parameter it **tests model performance on previously unseen data**.

$n$ -fold cross-validation is often called **Leave-One-Out** cross-validation. For each of the $i$ observations, $i=1,\cdots,n$
1. Fit your predictive model using all observations **except** the $i$ th, call it $\hat{f}_{-i}$
2. See how well you predicted the $i$ th error, $(y_{i}-\hat{f}_{-i}(x_{i}))^{2}$
3. Average these squared deviations over the $n$ partitions

We can immediately see a few drawbacks from this:
- Computationally expensive - for each hyperparameter $\lambda$, we need to fit $n$ times
- High correlation between models in each fold due to **massive overlap** (all folds share $n-2$ observations in common)

Most common in practice are either $5$ -or $10$ -fold cross-validation. For the responses $y$ in the $j$ th left-out fold, calculate
1. $MSE_{j} = \frac{1}{n_{j}} \sum_{i=1}^{n_{j}}(y_{i}-\hat{f}_{-j}(x_{i}))^{2}$, where $\hat{f}_{-j}(\cdot)$ was trained using the other $k-1$ folds, without fold $j$ and $n_{j}$ is the number of observations in the $j$ th fold
2. Then, find $\bar{MSE}=\frac{1}{k} \sum_{i=1}^{k}MSE_{j}$

This is a good compromise between the leave-one-out approach and a validation set, and **not as computationally expensive** as leave-one-out.

```ad-summary
**Comparing Cross-Validation Options**
- LOOCV will be very nearly **unbiased**, since it’s based on training sets of size $n_{\text{train}}-1$
- 5 or 10 fold CV will have more bias, since each training set is a smaller size ($\frac{1}{5} n_{\text{train}}$ or $\frac{1}{10} n_{\text{train}}$)

BUt bias is not the whole story!
- Test rate estimates from LOOCV have more variability than those from $5$ or $10$ fold cross validation
- There’s a **bias-variance tradeoff**

Ultimately the tradeoff, along with computational considerations, leads 5 or 10 fold cross validation to be more popular in practice.
```

```ad-example
**Example**: Extended Diabetes Regression Model

![[Pasted image 20241116173652.png|400]]

Based on this assessment, we’d choose the model produced by forward selection with $\lambda$ chosen through cross-validation.
- Though CV gave same results as choosing based on $OSR^{2}$ (optimistic), but it is **NOT** always the case

Substantial improvement seen in $OSR^{2}$ by penalizing complexity.
```

---
### Inferences
Splitting the sample into training and test sets allows us to conduct valid hypothesis tests using the techniques we have learned.

However, we **CANNOT use the conventional output** from the training set without modification.
- Issue: we *sifted through* the training set to find a model that performed well

But, if we **decided ahead of time** on this algorithm **before looking at the test set**, we can use the the $p$-values from the **test set**!

The training set serves an **exploratory** role.
- It allows us to search for a **potentially interesting model**

The test set serves a **confirmatory role**.
- We searched around for interesting associations on the training set; Now, we test them using data that **weren’t involved in the initial search**

This works so long as performance on the **test set in no way informs the choice of model**. If we have competing models and want to choose one based on out-of-sample performance on a held out data set, we really need **another validation set.**
- Choose model by reference to validation
- Do inference on the as of yet untouched test set

```ad-note
If we ultimately want **high power for the inference in the test set**, we’d want its sample size to be **large**. 
- However, if we make the sample size in the training set too small, then we might not have enough data to explore interesting patterns to begin with

For prediction, more data is used for training, since ultimately it is the algorithm we create by means of the training set that will be employed in the future.
```

---
### Regularized Regression
Rather than running ordinary least squares to estimate $\beta$: $$\hat{\beta}=\arg \min \limits_\tilde{\beta}\sum\limits_{i=1}^{n}\left(y_{i}-\left(\tilde{\beta_0}+\sum\limits_{j=1}^{p}\tilde{\beta}_{j}x_{ij}\right)\right)^{2}$$ we’ve considered methods that penalize complexity, which we quantified as the number of predictor variables $m$ in the chosen model.
- Again, $m=\sum_{j=1}^{p}\mathbb{1}(\hat{\beta}_{j} \ne 0)$

One way to penalize model complexity is through considering **best subset selection**, through an optimization problem of the form $$\hat{\beta}=\arg \min \limits_\tilde{\beta}\sum\limits_{i=1}^{n}\left(y_{i}-\left(\tilde{\beta_0}+\sum\limits_{j=1}^{p}\tilde{\beta}_{j}x_{ij}\right)\right)^{2}+\lambda \sum\limits_{j=1}^{p} \mathbb{1}(\tilde{\beta} \ne 0)$$ whereby we try to balance **in-sample performance** (first term) while weighing the desire to **avoid overfitting** (second term).

```ad-note
- [[19 Model Selection I#^a6739d|Mallows' Cp]] is equivalent to the above approach with $\lambda = 2$
- AIC asymptotically equivalent to the above approach with $\lambda = 2$
- BIC asymptotically equivalent to the above approach with $\lambda = \ln(n)$

A better approach to the above is to choose $\lambda$ via cross-validation.
```

Due to computational limitations (the need to enumerate all possible all possible models), we generally cannot calculate $\hat{\beta}$ above.
- Number of possible candidate models: $2^{p}$

Our remedy thus far are to consider iterative model building approaches that are heuristic approaches to the above optimization problem.
- [[19 Model Selection I#^7398ef|Forward stepwise]]: start with only intercept, iteratively increase model size
- [[19 Model Selection I#^bdb940|Backwards elimination]]: start with the full model, iteratively decrease model size

An alternative approach is rather thinking of greedy heuristics to best subset (forward stepwise, backwards elimination), we could instead consider changing the form of our complexity penalty: $$\hat{\beta}=\arg \min \limits_\tilde{\beta}\sum\limits_{i=1}^{n}\left(y_{i}-\left(\tilde{\beta_0}+\sum\limits_{j=1}^{p}\tilde{\beta}_{j}x_{ij}\right)\right)^{2}+\lambda \sum\limits_{j=1}^{p} |\tilde{\beta_{j}}|^{0}$$ where we define $$|\tilde{\beta}_{j}|^{0}=\begin{cases}
0 & \tilde{\beta}_{j}=0 \\
1 & \text{otherwise}
\end{cases}$$
In general, this method takes the form: $$\hat{\beta}=\arg \min \limits_\tilde{\beta}\sum\limits_{i=1}^{n}\left(y_{i}-\left(\tilde{\beta_0}+\sum\limits_{j=1}^{p}\tilde{\beta}_{j}x_{ij}\right)\right)^{2}+\lambda \sum\limits_{j=1}^{p} |\tilde{\beta_{j}}|^{q}, q >0$$
When $q=1$, this is called a **Lasso Regression**; when $q=2$, it is called a **[[8 Regression, Regularization#^e607e4|Ridge Regression]]**. These are the most commonly used regularized regression.

Importantly, for $q\ge 1$, the objective function is **convex** in $\tilde{\beta}$
- Implication: can be solved to **optimality efficiently**
- Rather than resorting to heuristics for best-subset selection, this approach changes the form of the complexity penalty in a way that is **computationally tractable**

```ad-note
We don’t penalize the **intercept** (this just finds the right vertical level for the line of best fit).
```

As $q$ tends towards zero $|\tilde{\beta}_{1}|^{q}+|\tilde{\beta}_{2}|^{q}=1$, behaves more like $|\tilde{\beta}_{1}|^{0}+|\tilde{\beta}_{2}|^{0}$
- For $q \ge 1$, note that the corresponding regions are convex sets
- For $q>1$, boundaries of region are smooth as they intersect the axes

![[Pasted image 20241116202050.png|400]]

For $q\ge 1$, it is computationally feasible; otherwise it is computationally intractable.