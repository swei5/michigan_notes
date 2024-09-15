[[2024-09-12]] #Regression 

### Categorical Variables

```ad-important
**Definition 6.1**: Categorical Variables

Categorical (also called nominal or qualitative) variables are ones in which the ‘values’ = are **labels for group memberships**. These values are often (but not always) coded as words.

Categorical variables that can only take on **two** distinct values are called **binary variables**.
```

If we have a categorical variable that takes on $G$ values, `R` creates $G − 1$ binary variables (also called dummy variables) and includes them as regressors.

```ad-example
**Regression with Categorical Variables, Example**

![[Pasted image 20240914192056.png|500]]
```

