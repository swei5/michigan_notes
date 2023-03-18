[[2023-03-13]] #SupervisedLearning #NeuralNetwork #RNN

### Motivation
1. Efficiently address varying input **size**
	- Answer simple and complicated questions at once
1. Incorporate **dependencies** over sequences/time
	- Auto-fill
1. Allow **shared parameters** over time
	- Captures recurring patterns in language or whatever input

---

### Recurrent Neural Network

```ad-important

**Definition 15.1**: Recurrent Neural Network

**Recurrent neural networks** (RNNs) can be used to model sequences of arbitrary length. 

Consider an input sequence $[x_1,x_2,...,x_T]$, where $x_t$ is the **data** at timestep $t$, and binary **output** $y$. For each timestep $t$, we pass $x_t$ and a **hidden state** $h_{(t−1)}$ into the RNN.

![[Pasted image 20230318132732.png|600]]
```

The RNN takes these inputs, applies **weights**, **biases**, and a non-linear **activation function**, and outputs a new **hidden state** $h_t$. The hidden state will then be used as an input for timestep $h _{(t+1)}$ in addition to $x_{(t+1)}$:
$$y_t=g(w_{x}^{(t)}x_{t}+w_{h}^{(t)}h_{(t-1)}+w_{0}^{t})$$
where
$$h_t=f(u_{x}^{(t)}x_{t}+u_{h}^{(t)}h_{(t-1)}+u_{0}^{(t)})$$
We call this class of neural networks “*recurrent*” because we **feed its output back** in as input. Unrolled (image on the right above), we can see more clearly how RNNs can model sequences.

If we want our parameters to be **shared** (see motivation above), we can set the weights in each layer to be the same.
$$w_{x}^{(1)}=w_{x}^{(2)}=\cdots=w_{x}^{(t)}$$

In this case,
$$y_t=g(w_{x}x_{t}+w_{h}h_{(t-1)}+w_{0})$$
$$h_t=f(u_{x}x_{t}+u_{h}h_{(t-1)}+u_{0})$$

---

### RNNs: Types of Tasks
- Many-to-one
	- Example: sentiment analysis
		![[Pasted image 20230318133522.png|350]]
- One-to-many
	- Example: music/image generation (input as a single image and output as a video, which consists multiple frames of an image)
		![[Pasted image 20230318133647.png|350]]
- Many-to-many (same length)
	- Example: video captioning
		![[Pasted image 20230318133723.png|350]]
- Many-to-many (different length)
	- Example: translation
		![[Pasted image 20230318133758.png|450]]
		- Where the part $x_1,\cdots,x_t$ is the **encoder**, and $y_{1,\cdots}y_s$ is the **decoder**

#### Challenges
- Typical RNN might “*forget*” information as the sequence grows **longer**
	- Vanishing gradient backprop through time (BPTT)

**Solution**:
- Gated cells: **control** **information** is used to update the cell state
- Gated Recurrent Units (**GRU**s) and Long, short term memory cells (**LSTM**s) include “gates” to control the flow of information

##### LSTM Unit
