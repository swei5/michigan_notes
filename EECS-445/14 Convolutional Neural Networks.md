[[2023-03-06]] #NeuralNetwork #SupervisedLearning #Optimization 

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
- Convolutional neural networks (CNN) work better!
	- Convolutional blocks
		- Convolutional layers
		- ReLU
		- Max Pooling
		- Fully connected layer

