# Perceptron

## What is a Preceptron

A perceptron is the foundational building block of deep learning and artificial neural networks. it is a binary linear classifier designed to take multiple continuous or binary inputs, weigh their importance, and output a single binary decision ($0$ or $1$).

## Componets of a Preceptron

A perceptron consists of four core components:

1. Inputs ($x_1, x_2, \dots, x_n$): The features or numerical data fed into the model.

2. Weights ($w_1, w_2, \dots, w_n$): Parameters that determine the importance or influence of each input on the final decision. Higher weights mean stronger influence.

3. Bias ($b$): An adjustable offset value that shifts the decision boundary. It allows the model to fire even when all inputs are zero, controlling how easily the perceptron activates.

4. Activation Function: A thresholding function that converts the aggregated linear combination into a discrete output. Classically, this is a Heaviside step function.

## The Mathematical Formulation

The perceptron computes a weighted sum of its inputs, adds the bias, and passes the result through an activation function:

$$
z = \sum_{i=1}^{n} w_i x_i + b = \mathbf{w} \cdot \mathbf{x} + b
$$

The output $y$ is determined by the step function:

$$
y = \begin{cases} 1 & \text{if } z \ge 0 \\  0 & \text{if } z < 0 \end{cases}
$$

### Concrete Example:

To see how a perceptron works in practice, let's look at how it learns or models a logical AND gate. An AND gate outputs $1$ only if both inputs are $1$; otherwise, it outputs $0$.

#### Truth Table

- Input $x_1 = 0, x_2 = 0 \implies \text{Output} = 0$

- Input $x_1 = 0, x_2 = 1 \implies \text{Output} = 0$

- Input $x_1 = 1, x_2 = 0 \implies \text{Output} = 0$

- Input $x_1 = 1, x_2 = 1 \implies \text{Output} = 1$

#### Setting Weights and Bias

Let's manually assign weights $w_1 = 1$, $w_2 = 1$, and a bias $b = -1.5$.

The linear equation becomes:

$$
z = (1 \cdot x_1) + (1 \cdot x_2) - 1.5
$$

#### Evaluating Each Scenario:

1. when $x_1 = 0, x_2 = 0$ (when both are `False`)
    $$
    z=(1 \cdot x_1)+(1 \cdot x_2)-1.5=-1.5
    $$
    since $-1.5 \lt 0$, the activation function outputs $0$

2. when $x_1=0, x_2=1$ ($x_2$ is `True`)
    $$
    z = (1 \cdot 0) + (1 \cdot 1) -1.5 = -0.5
    $$
    since $-0.5 \lt 0$ the activation function outputs $0$

3. When $x_1=1, x_2=0$ ($x_1$ is `True`)
    $$
    z = (1 \cdot 1) + (1 \cdot 0) - 1.5 = -0.5
    $$
    since $-0.5 \lt 0$ the activation function outputs $0$

4. when $x_1=1, x_2=2$ ($x_1$ and $x_2$ are `True`)
    $$
    z= (1 \cdot 1) + (1 \cdot 1) -1.5 = +0.5
    $$
    since $+0.5 \gte 0$ the activation function outputs $1$

The perceptron successfully classifies the logical AND operation by drawing a linear decision boundary separating the positive case $(1,1)$ from the negative cases.

## Python Implementation using NumPy

Here is a clean implementation of a single perceptron inference and update rule in Python:

```python
import numpy as np

class Perceptron:
    def __init__(self, input_dim, lr=0.1):
        # Initialize weights and bias to small random values or zeros
        self.weights = np.zeros(input_dim)
        self.bias = 0.0
        self.lr = lr

    def activation(self, z):
        return 1 if z >= 0 else 0

    def predict(self, x):
        z = np.dot(x, self.weights) + self.bias
        return self.activation(z)

    def train(self, X, y, epochs=10):
        for epoch in range(epochs):
            for xi, target in zip(X, y):
                prediction = self.predict(xi)
                error = target - prediction
                # Perceptron learning rule update: w = w + lr * error * x
                if error != 0:
                    self.weights += self.lr * error * xi
                    self.bias += self.lr * error

# Example usage: Training an AND gate
X_and = np.array([[0, 0], [0, 1], [1, 0], [1, 1]])
y_and = np.array([0, 0, 0, 1])

perceptron = Perceptron(input_dim=2)
perceptron.train(X_and, y_and, epochs=5)

print("Learned Weights:", perceptron.weights)
print("Learned Bias:", perceptron.bias)
print("Predictions:", [perceptron.predict(x) for x in X_and])
```