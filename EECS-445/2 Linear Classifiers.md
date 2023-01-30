[[2023-01-09]] #SupervisedLearning #LinearClassifier #Perceptron #Hyperplane #EmpiricalRisk

### Linear Binary Classifier
This is **supervised** **learning** as the data are already pre-labeled.
- Basically, a yes/no question

```ad-example
Classify whether if a review is helpful or not helpful. We have a set of reviews that have **relevant features** and a **label**, and we want to predict the label of the review that is **unlabeled**. 

We assume all features given are **relevant** in this case.  

Features:
- star rating
- length of review

Labels:
- helpful $(+1)$ /unhelpful $(-1)$
```


#### Classification as ML Problem
1. Feature Vector
	- Features are **statistics** or **attributes** that describe the data. Here, we represent them in terms of vectors: $$\vec{x}\in \mathbb{R}^d \to \mathbb{R} \times \mathbb{R}\times \cdots \times \mathbb{R}$$
		where $d$ is the dimension of our data set. In our case, $d=2$.
		
		![[Pasted image 20230110110411.png]]
	

```ad-example
In this example, $\vec{x}^i$ represents datapoint $i$ such that $\vec{x}^i \in \mathbb{R}^2 \ s.t. \ \vec{x}^i \in [0,1]\times[0,1]$. This set is known as the **feature space**.

Specifically, $\vec{x}^4=[0.2,0.4]^T$. And $\vec{x}^4_1=0.2$.
```

2. Labels
	- In general, we refer to labels as $y^i$ such that $y^i \in \set{1,-1}$. This set is known as the **label space**.

3. Training dataset
	- We pair up our feature vector and labels to get a pair: $(\vec{x}^i,y^i)$ with $i \in [0,n]$.

---

#### Geometric Visualization
We can plot our example on a 2-D plane.

**Goal**: Find a line that splits the space ($\mathbb{R}^2$) into positive and negative regions - **linear decision boundary**. 

![[Pasted image 20230110111505.png|400]]

##### Why Linear Boundary
Let $\mathcal{H}$ be the set of classifiers under consideration.
- $\mathcal{H}$ is the set of all hyperplanes.
- Too many choices is not always a good thing
	- May be **overfitting** and may not **generalize** well

**Solution**
Constrain possible $\mathcal{H}$, but cannot be **too constrained** either cause it leads to **under-fitting**

This problem is called **model selection**.
- Linear models are more powerful than they may appear

---

#### Hyperplanes
A hyperplane in $\mathbb{R}^d$ can be specified by parameter vector $\overline{\theta} \in \mathbb{R}^d$ and offset $b\in\mathbb{R}$. ^ba658c

It is the **set** of points $\overline{x}\in\mathbb{R}$ such that $\overline{\theta} \cdot \overline{x}+b=0$.

$$\overline{\theta} \cdot \overline{x}=\sum_{i=1}^{d} \theta_ix_i$$

```ad-note
$\overline{\theta}$ is orthogonal to the hyperplane $\overline{\theta}\cdot \overline{x}=0$.
```

```ad-example
In $\mathbb{R}^2$, this is simply $\theta_1 x_1+\theta_2 x_2+b=0$. Like a line. In $\mathbb{R}^2$ this is the **set** of lines.
```

![[Pasted image 20230110151053.png|400]]

From the graph above, we note the pattern that every point (to the right/above) of the boundary has a dot product $>0$, and every point (to the left/below) of the boundary has a dot product $<0$.

Mathematically, this is because

$$\overline{\theta}\cdot\overline{x}=\sum_{i=1}^{d}\theta_ix_i=||\overline{\theta}||_2 ||\overline{x}||_2 \cos{\alpha}$$

Where $||\overline{\theta}||$ is the L2 norm of $\overline{\theta}$.

We have three cases.
1. Case 1: $0\degree \le\alpha<90\degree$ or $270\degree <\alpha\le360\degree$
	- $\overline{\theta}\cdot\overline{x}>0$

2. Case 2: $90\degree < \alpha < 270\degree$
	- $\overline{\theta}\cdot\overline{x}<0$

3. Case 3: $\alpha \in \set{90\degree,270\degree}$
	- $\overline{\theta}\cdot\overline{x}=0$, $\overline{x}$ is **ON** the hyperplane

Thus, once we established $\overline{\theta}$, we can classify a datapoint using the **sign** of $\overline{\theta}\cdot\overline{x}$.

---

#### Linear Classifiers

##### Assumptions
1. Constrain $\mathcal{H}$ to be the set of all hyperplanes that **go through** the origin.
2. Constrain problem to datasets that are **linearly separable**. ^91d7b9
	- There exists $\overline{\theta}\in\mathbb{R}^d$ such that $y^i(\overline{\theta}\cdot\overline{x}^i)>0$ for $i=1,\cdots,n$.
		- The positive sign means that the classification is correct, i.e. $y^i$ is on the **right side** of the boundary (both positive or both negative).


```ad-important
**Definition 2.1**: The linear classifier is defined as
$$h(\overline{x};\overline{\theta})=\mathrm{sign}(\overline{\theta}\cdot\overline{x})$$

The choice of $\overline{\theta}$ determines
1. Orientation of the hyperplane
2. The **predicted** class label
```

^b65093

##### Selecting $\overline{\theta}$

We want to find $\overline{\theta}$ that works well on **training dataset** $S=\set{(\overline{x}^i,\overline{y}^i)}_{i=1}^{n}$.

```ad-important
**Definition 2.2**: The training error is defined as
$$E_n(\overline{\theta})=\frac{1}{n}\sum_{i=1}^{n} \left([[\overline{y}^i \ne h(\overline{x};\overline{\theta})]]\right)$$

The predicate function returns 1 when $p$ (predicate) is true, and 0 otherwise.
```

^78fb13

Thus the function above returns the **count** of the datapoints that were misclassified.

Our goal is therefore to **minimize** training error.

```ad-important
**Definition 2.3**: The empirical risk is defined as
$$R_n(\overline{\theta})=\frac{1}{n}\sum_{i=1}^{n} \left((\overline{y}^i\cdot(\overline{\theta}\cdot \overline{x})\right)$$
```

^52880f

##### Algorithm

![[Pasted image 20230110154810.png|500]]