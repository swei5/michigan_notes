[[2023-01-09]]

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
		
	- $\vec{x}^i$ represents datapoint $i$ such that $\vec{x}^i \in \mathbb{R}^2 \ s.t. \ \vec{x}^i \in [0,1]\times[0,1]$. This set is known as the **feature space**.
			![[Pasted image 20230110110411.png]]
		
		In this example, $\vec{x}^4=[0.2,0.4]^T$. And $\vec{x}^4_1=0.2$.

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
A hyperplane in $\mathbb{R}^d$ can be specified by parameter vector $\overline{\theta} \in \mathbb{R}^d$ and offset $b\in\mathbb{R}$.

It is the **set** of points $\overline{x}\in\mathbb{R}$ such that $\overline{\theta} \cdot \overline{x}+b=0$.

$$\overline{\theta} \cdot \overline{x}=\sum_{i=1}^{d} \theta_ix_i$$

```ad-note
$\overline{\theta}$ is orthogonal to the hyperplane $\overline{\theta}\cdot \overline{x}=0$.
```

```ad-example
In $\mathbb{R}^2$, this is simply $\theta_1 x_1+\theta_2 x_2+b=0$. Like a line. In $\mathbb{R}^2$ this is the **set** of lines.
```


