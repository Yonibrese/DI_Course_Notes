# Layers

## what are Layers 

A single perceptron acts as a basic decision-maker, much like an individual making a simple yes/no choice based on weighted inputs. However, real-world problems are too complex for a single perceptron to handle effectively. That’s where layers of perceptrons come in.

Each layer in a neural network is essentially a group of perceptrons working together. Instead of making a single decision, they pass their outputs forward, allowing the network to learn increasingly complex features. The three main types of layers are:

- **Input Layer**:  Receives the raw feature vector ($x_1, x_2, \dots, x_n$). No mathematical transformations happen here; it simply passes the features forward.
  - The number of neurons equals the number of input features.

- **Hidden Layer**: The computational engine of the network. These layers sit between the input and output layers. Each layer consists of multiple neurons (or nodes) that perform linear transformations followed by non-linear activations. "Deep" learning gets its name from having multiple hidden layers stacked sequentially, allowing the network to learn complex, non-linear boundaries.

- **Output Layer**: Produces the final prediction ($\hat{y}$). For binary classification, this is typically a single neuron with a Sigmoid activation; for multi-class classification, it uses Softmax; for regression, it outputs a continuous linear value.

## How Hiddent layer works

Every neuron in a hidden or output layer performs two primary steps:

1. Linear Transformation: Computes the weighted sum of its inputs plus a bias term [Precptron Guide](./Perceptron.md)

2. Non-Linear Activation Function: Passes $z$ through an activation function ($f$) to introduce non-linearity, enabling the network to learn complex patterns[Activation functions Guide](./activationFunction.md)

## Concrete Example

Let's walk through a concrete example. Imagine building an ANN to predict whether a software deployment build will **Pass (1)** or **Fail (0)** based on two normalized features:

- $x_1$: Code test coverage ratio ($[0.0, 1.0]$)
- $x_2$: Lines of code changed ($[0.0, 1.0]$)

### Network Topology:

- Input Layer: 2 nodes ($x_1, x_2$)
- Hidden Layer: 2 neurons using ReLU activation
- Output Layer: 1 neuron using Sigmoid activation (probability of success)

### Step 1: Forward Propagation (Inference)

Suppose we have a specific commit where $x_1 = 0.8$ (80% coverage) and $x_2 = 0.2$ (small PR).

#### Hidden Layer (Neuron 1):

- Suppose weights are $w_{11} = 0.5, w_{12} = -0.3$ and bias $b_1 = 0.1$.
- $z_1 = (0.5 \times 0.8) + (-0.3 \times 0.2) + 0.1 = 0.4 - 0.06 + 0.1 = 0.44$
- Applying ReLU: $a_1 = \max(0, 0.44) = 0.44$

#### Hidden Layer (Neuron 2):

- Suppose weights are $w_{21} = -0.2, w_{22} = 0.9$ and bias $b_2 = -0.05$.
- $z_2 = (-0.2 \times 0.8) + (0.9 \times 0.2) - 0.05 = -0.16 + 0.18 - 0.05 = -0.03$
- Applying ReLU: $a_2 = \max(0, -0.03) = 0.0$ (neuron is turned off)

#### Output Layer:

- Takes hidden outputs $a_1 = 0.44, a_2 = 0.0$ with weights $w_{o1} = 1.2, w_{o2} = 0.8$ and bias $b_o = -0.1$.
- $z_{output} = (1.2 \times 0.44) + (0.8 \times 0.0) - 0.1 = 0.528 - 0.1 = 0.428$
- Applying Sigmoid: $\hat{y} = \frac{1}{1 + e^{-0.428}} \approx 0.605$ (60.5% predicted probability of passing).

### Step 2: Loss Calculation
If the actual label for this commit was $y = 1$ (Pass), we calculate the error using a loss function (e.g., Binary Cross-Entropy):

$$
\text{Loss} = -[y \log(\hat{y}) + (1-y)\log(1-\hat{y})]$$

### Backpropagation & Optimization

The model computes the gradient of the loss with respect to every weight and bias using the chain rule of calculus. An optimizer (like Adam or SGD) then updates the weights backward from the output layer to the input layer:

$$
W_{\text{new}} = W_{\text{old}} - \eta \frac{\partial \text{Loss}}{\partial W}
$$

(where $\eta$ is the learning rate). Repeating this across thousands of commits tunes the weights until predictions converge.

## Specialized Layer Types in Deep Learning

As deep learning evolved beyond basic fully connected ANNs (Dense layers), specialized layer architectures were invented to handle specific domains:

| Layer Type | Primary Use Case | How It Works |
| :---: | :---: | :---: |
| Convolutional Layers (CNNs) | Computer Vision (Images, Video) | Uses sliding kernel filters to extract spatial features (edges, textures, shapes) while preserving pixel topologies. |
| Recurrent / Attention Layers (RNNs / Transformers) | Natural Language Processing & Time Series | Captures sequential dependencies and contextual relationships across time steps or token sequences. |
| Pooling Layers | Downsampling in CNNs | Reduces spatial dimensions (e.g., Max Pooling) to control overfitting and computational cost. |
| Normalization Layers (Batch/Layer Norm) | Training Stability | Normalizes layer inputs to stabilize internal covariate shifts and speed up gradient descent convergence. |
| Dropout Layers | Regularization | Randomly zeroes out a percentage of neuron activations during training to prevent co-adaptation and overfitting. |
