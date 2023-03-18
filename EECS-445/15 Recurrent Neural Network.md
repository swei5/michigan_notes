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
