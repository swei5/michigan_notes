[[2023-02-20]]  #NeuralNetwork

### Recap: Feature Representation
1. Explicit Feature Mapping
	- Polynomial kernels with feature mapping: ![[6 Dual Formulation of SVM#^816d20]]
		- $f (\overline{x};\overline{\theta})=\theta_{0}x^{0}+\theta_{1}x^{1}+\cdots+\theta_{p}x^{p}$

	- Radial Basis Function: ![[7 SVM, Kernel Trick#^f606f6]]

2. Implicit (learned) Feature Mapping
	- Decision Trees: ![[9 Decision Tree#^213987]]
		- which is equivalent to $\sum\limits_{m=1}^{M} \theta_{m} \phi_{m}(\overline{x})$, where $\theta_{m}$ is the majority label, and $\phi_m(\star)$ is the implicit mapping we've done

	- Boosting: ![[11 Ensembles (Boosting)#^c54656]]
		- which is equivalent to $\sum\limits_{m=1}^{M} \alpha_{m}\phi_{m}(\overline{x})$

---
### Feedforward Neural Network
A very flexible feature mappings.
- Instead of $\phi$, we will use $h$ - which stands for **hidden neurons**

How do we make our learned $h$ flexible?
- Introduces **weights** into $h$
- Introduce **non-linearities** in $h$
- Combine non-linear $h$ s

```ad-important
**Definition 13.1**: Feed-forward linear neural network

A directed, acyclic graph composed of layers of nodes called **neurons**, where the $j$th neuron in layer $\ell$ will

1. Compute a **linear combination** of inputs from neurons $i$ in the previous layer:
$$z_{j}^{(\ell)}=\sum\limits_{i} w_{ji}^{(\ell)} h_{i}^{\ell -1}$$

2. Transform the linear combination using an **activation function** $g$:
$$h^{(\ell)}=g(z_{j}^{(\ell)})$$
```

To make predictions, each input feature is transformed and propagated forward to deeper layers until the **final neuron** is reached, in a process called **forward propagation**. By layering the outputs of these non-linear transformations, a neural network can capture complex nonlinear relationships between features.

![[Pasted image 20230227155836.png|500]]

#### Activation Functions

```ad-important
**Definition 13.2**: Activation Functions

Non-linear functions are used to perform feature mappings that **better capture nonlinear relationships** between features, and so the choice of activation function is a critical hyperparameter to choose when designing neural network architectures.
```

There are a number of important properties of activation functions:
1. **Nonlinear**
	- If a linear activation function is used, then despite how many layers are added, the final output is still a linear combination of the input features - no better than a simpler linear model.
2. **Easy to compute**
	- Training and evaluating neural networks are generally computationally expensive,  so activation functions should have a fast compute speed to minimize time spent in each layer.
3. **Sub-differentiable**
	- In order to perform gradient descent, we must be able to take the derivative (or a subderivative) of an activation function.

Commonly used activation functions include:
![[Pasted image 20230227161921.png|500]]

Let us assume in this lecture we would use the **sigmoid activation function**.
$$h(z)=\frac{1}{1+e^{-z}}$$

```ad-example
**Example 1**: Calculate $h_{1}^{(2)}$

$$z_{1}^{(2)}=w_{11}^{(1)}x_{1}+w_{12}^{(1)}x_{2}+w_{10}^{(1)}$$
$$h_{1}^{(2)}=\sigma(z_{1}^{2})$$

```
