# Backward Popagation

## What is backward Proagation?

**Backpropagation** (short for "backward propagation of errors") is the foundational algorithm used to train artificial neural networks. If the architecture of a neural network is its anatomy, backpropagation is its nervous system—allowing the network to learn from its mistakes by adjusting its internal parameters (weights and biases) to minimize errors.

## How Backpropagation Works (Step-by-Step)

The backpropagation process relies on calculus—specifically the chain rule—and consists of three main stages:

1. **Calculate the Loss (Error)**: Compare the network's final prediction ($\hat{y}$) against the actual ground-truth label ($y$) using a loss function (e.g., Mean Squared Error or Binary Cross-Entropy). This gives a single numerical score representing total error. 

2. **Compute Gradients (The Chain Rule)**: Move backward from the output layer to the input layer. At each neuron, calculate the partial derivative of the loss with respect to its weights and biases. This tells us how much each weight contributed to the final error.

3. **Update the Weights (Gradient Descent)**: Adjust each weight and bias in the opposite direction of its gradient, scaled by a hyperparameter called the learning rate ($\alpha$):
    $$
    w_{\text{new}} = w_{\text{old}} - \alpha \frac{\partial L}{\partial w}
    $$

## Real world example

This is continued from the examle in the [forward propagation guide](ForwardPropagation.md)

- Our Prediction ($\hat{y}$): $0.84$ ($84\%$ chance of fraud)

- The Actual Outcome ($y$): This transaction was actually fraudulent, meaning the true label is $1$.

### Calculating the Loss (The Error)

Using a simple Mean Squared Error (MSE) loss function, $L = \frac{1}{2}(\hat{y} - y)^2$:

$$
L = \frac{1}{2}(0.84 - 1.0)^2 = \frac{1}{2}(-0.16)^2 = 0.0128
$$

The loss is $0.0128$. Our goal during backpropagation is to change the weights so this loss shrinks toward zero.

### Backward Pass through the Output Layer

We first calculate how sensitive the loss is to changes in our output weights ($w_{out1}$ connected to Neuron A, and $w_{out2}$ connected to Neuron B). Using the chain rule:

- Error derivative at output: $(\hat{y} - y) = 0.84 - 1.0 = -0.16$

- To increase the final prediction ($\hat{y}$) closer to $1.0$, we need to increase the weights that connect to active hidden neurons.

Suppose our learning rate ($\alpha$) is set to $0.1$. Backpropagation calculates the updates:

- Updating $w_{out1}$: If its gradient indicates it should increase, we update it:
    $$w_{out1\_new} = 1.2 + (0.1 \times \text{adjustment factor}) \approx 1.28$$

- Updating $w_{out2}$: Similarly updated to strengthen its contribution.

## The Training Loop: Forward + Backward

By repeating this two-step loop millions of times across a dataset of credit card transactions:

1. Forward Pass makes a guess.

2. Backward Pass measures the error and shifts the weights.

The neural network gradually transforms from making random guesses into a high-precision security system capable of catching sophisticated fraud patterns in milliseconds.


