[[2023-03-06]] #NeuralNetwork #SupervisedLearning #Optimization #CNN

### Intro
- Represent images as input - pixels $X$
- Pixels take a value in $\set{0,1}$
	- In real-world, wants to be in $[0,255]$

We want to capture
- Exploitability in structure in an image
	- Spacial locality matters, otherwise no way to recognize
- Translation invariance
	- Doesn't matter **WHERE** our target object occurs in the image

If we use the feedforward neural networks, we will missing out on these structures.
- Convolutional neural networks (CNN) work better! CNNs have different types of layers that **MAY NOT** be fully connected like we've seen in feedforward NN.
	- Convolutional blocks
		- Convolutional layers
		- ReLU
		- Max Pooling
		- Fully connected layer

---

### Convolutional Layer

Let's start with a simple 1D example.

```ad-example
![[Pasted image 20230310222254.png|400]]

Our goal is to detect **abnormal signals**.

In this graph, for signals that fall within the red block (range), we assign it a value of 0, otherwise 1.

We then apply a **filter** and apply it to the image, which produces the **convolved features**:

![[Pasted image 20230310222340.png|400]]

![[Pasted image 20230310222356.png|400]]

After convolution, we then apply ReLU (non-linearity) to our convolved features. Note that $\text{ReLU}(z)=\max(0,z)$.

![[Pasted image 20230310222553.png|400]]

When we compare our output to the graph, we note that stand-alone 1 corresponds to a $[0,1,0]$ sequence - an abnormal signal in the middle of two normal signals, indicating a spike.
```

However, if we selected a different image where the spike occurs at the end that it is not flanked by 0's, we find ourselves unable to detect it no more.

![[Pasted image 20230310223352.png|400]]

This tells us that we don't take information on the **edges** into account **as importantly** as information in the **middle**.
- We lost information!

#### Padding
To counteract this, we add additional values (0s in **zero padding**), enabling us to capture more information on the edges.

```ad-example
![[Pasted image 20230310223638.png|400]]

After applying the padding, we are once again able to detect the spike.
```

#### Filters
Filter values $w\in\mathbb{R}$. The size of the filter is a hyperparameter that we deliberately choose.
- We can have multiple filters in one single layer in CNN
- Each filter produces a **NEW** **feature map**
	- Filters in higher layers get to learn more complicated ideas

There are a number of hyperparameters we need to design:
- Number of filters
- Stride length
- Filter size
- Zero-padding
- Parameter sharing (same **weights** across all depths)

---

#### Size of Convolved Features
Let's use a 2D example to illustrate. Assume that
- Input: $I_{w}\times I_{h}$
- Filter: $F\times F$
- Stride: $S$
- Output: $O_{w}\times O_{h}$ where:
	- $O_{w}=\frac{I_{w}-F}{S}+1$
	- $O_{h}=\frac{I_{h}-F}{S}+1$

---

### Max Pooling
It is common to insert **pooling layers** between convolutional layers in a CNN in order to **reduce dimensionality** which has the dual benefit of **reducing overfitting** as well as **reducing computational cost**. 
- Helps introduce some spatial invariants
- Down sampling effects
- Consolidates what we've learned so far

```ad-example
![[Pasted image 20230310230313.png|400]]

In our example, the **depth** of the layer remains the same, but size **shrinks**.

A 3D example looks very similar (same depth, different size):
![[Pasted image 20230310230452.png|400]]
```

---

### Architecture
After we pooled our feature sets, we *flatten them* and build a fully connected networks. Then, we pass the nodes to what's called a **soft-max** function:
$$\hat{y}_{j}=\frac{\exp(z_{j})}{\sum\limits_{k=1}^{K}\exp(z_{k})}$$

```ad-important
**Definition 14.1**: Soft-max function

A soft-max function transforms an input vector of size $k$ to a probability distribution over $k$ possible outcomes.
```

It is helpful in helping us **encode uncertainty** in the classification and helps the **loss function** to penalize the model according to the uncertainty.
- This is typically used with **categorical cross entropy** (Loss function).


Visually, the architecture looks like

![[Pasted image 20230311170206.png|600]]