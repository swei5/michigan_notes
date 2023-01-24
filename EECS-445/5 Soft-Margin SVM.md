[[2023-01-23]] #SVM #Optimization

### Soft-Margin SVM
Supports data that are **NOT** linearly separable.

```ad-important
**Definition 5.1**: The Soft-Margin SVM is defined as the **Hard-Margin** SVM with a slack to the constraints.
$$\min_{\overline{\theta}, b, \overline{\xi}} \frac{||\overline{\theta}||^2}{2}+C\sum\limits_{i=1}^{n}\xi_i$$
$$\mathrm{subject\ to\ \ \ \ } y^{i}(\overline{\theta}\cdot x^{i}+b) \ge 1-\xi_{i}\mathrm{\ \ \ \ for} i \in \{1,\cdots, n\}, \xi_i\ge0$$

Here, $C$ is a hyperparameter.
```

In other words, we allow the **decision boundary** to violate the constraint.

Soft-margin SVM has several advantages.
- Can handle data that are **NOT** linearly separable
	- Note that **ALL** data that are mis-classified or on the margin boundary are **support vectors**.
- Reduce effect of outliers and less chances of overfitting

#### Hyperparameter

```ad-note
Soft-Margin SVM is a special case for Hard-Margin SVM.
- Intuitively, as $C$ increases, penalty/misclassification increases
- However, as $C\to \infty$, penalty/misclassification decreases, and this is the hard margin SVM
```
