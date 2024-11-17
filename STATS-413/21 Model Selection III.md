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
```

---
### Inferences
Splitting the sample into training and test sets allows us to conduct valid hypothesis tests using the techniques we have learned.

However, we **CANNOT use the conventional output** from the training set without modification.
- Issue: we *sifted through* the training set to find a model that performed well

The training set serves an **exploratory** role.
- It allows us to search for a **potentially interesting model**

The test set serves a **confirmatory role**.
- We searched around for interesting associations on the training set; Now, we test them using data that **weren’t involved in the initial search**

