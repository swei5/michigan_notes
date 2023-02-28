[[2023-02-20]]  #NeuralNetwork #FeatureMap #SGD #SupervisedLearning 

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
- Instead of $\phi$, we will use $h$ - which stands for **hidden layers** of neutrons.

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

In simple terms, $j$ is the **index** of the **hidden neuron**, and $i$ is the coordinate in $\overline{x}$ in the previous layer that we map from.
```

```ad-example
For $S_n=\{\overline{x}^{(i)}, y^{(i)}\}_{i=1}^{100}$, what is the dimension of $\overline{h}^{(3)}$ if $\overline{z}^{(3)}$ has 5 hidden nodes?

First, note that $\dim(\overline{h}_{1}^{(3)})=100$, since $\dim(\overline{z}_{1}^{(3)})=100$ as $\overline{z}_{1}^{(3)}$ takes input from the previous 100 neurons and previously 100 raw data points.

Then, since there are 5 hidden neurons, meaning that $\dim(\overline{h}^{(3)})=5$, combining the first step we get $100\times 5$ for the dimension of $\overline{h}^{(3)}$.
```

To make predictions, each input feature is transformed and propagated forward to deeper layers until the **final neuron** is reached, in a process called **forward propagation**. By layering the outputs of these non-linear transformations, a neural network can capture complex nonlinear relationships between features.

![[Pasted image 20230227155836.png|500]]

In this example, the input of our neural network is
$$\mathbf{x}=[x_1,x_2,1]^T$$
and the output is
$$z^{(4)},\ \ s.t.\ \ z^{(4)}\in\mathbb{R}$$

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

The output of $h$ is strictly between $[0,1]$.

```ad-info
The range of $\tanh$ activation function is $[-1,1]$.
```

```ad-example
**Example 1**: Calculate $h_{1}^{(2)}$

$$z_{1}^{(2)}=w_{11}^{(1)}x_{1}+w_{12}^{(1)}x_{2}+w_{10}^{(1)}$$
$$h_{1}^{(2)}=\sigma(z_{1}^{2})$$

**Example 2**: Calculate $z^4$
$$z^{4}=w_{11}^{(3)}h_1^{(3)}+w_{12}^{(3)}h_2^{(3)}+w_{13}^{(3)}h_3^{(3)}+w_{10}^{(3)}$$
```

---

### Neural Networks: Architecture
- Number of hidden layers
	- NN with one hidden layer can model complex functions provided it has **enough neurons** (i.e., is wide)
		- In fact, 2-layer NN can approximate *any* function
	- Deeper nets can **converge faster** to a good solution

```ad-hint
We want to start with 1-2 hidden layers and ramp up until you we to overfit.
```

- Number of Neurons per Hidden Layer
	- Number of neurons in input & output determined by task **at hand**
	- A common practice is to use the same size for **ALL** hidden layers (one tuning parameter)

```ad-hint
We want to increase number of neurons until we start to overfit.
```

---

### Training Neural Networks
- Training Data
	- $S_n=\{\overline{x}^{(i)}, y^{(i)}\}_{i=1}^{n}$
		- $\overline{x}\in \mathbb{R}^d$
		- $y\in\{-1,1\}$

- Parameters
	- $\overline{\theta}=[w_{11}^{(1)},w_{12}^{(1)},\cdots,w_{ij}^{(k)}]^T$

- Loss Function
	- $J (\overline{\theta})=\frac{1}{n} \sum\limits_{i=1}^{n} \mathrm{Loss}(y,h(\overline{x}^{(i)};\overline{\theta}))$

#### Back Propagation
Brief idea: run stochastic gradient descent. 

We start with the **last layer**, go through each node in reverse order to figure out the contribution of **each node** to the loss.

Let us begin with a one-layer NN for illustration.

![[Pasted image 20230227175320.png|300]]

Assume that we go with the hinge loss. Then,
$$\frac{\partial f}{\partial w_i}=\frac{\partial f}{\partial z} \frac{\partial z}{\partial w_i}=-yx_i$$

We update $w_i$ if $\mathrm{loss}\ge 0$:
$$w_{i}^{(k+1)}=w_{i}^{(k)}+\eta_{k}yx_i$$
