# Activation Functions

## What are activation functions

An activation function is a mathematical gate attached to each neuron in a neural network. It determines whether the neuron should be "activated" (fired) or not based on the weighted sum of its inputs plus a bias term.

another way to think about it: A perceptron’s decision is just a number. But sometimes, we need a clear “yes” or “no” answer. That’s where activation functions come in. They act like a switch, turning the perceptron’s output into something useful. Activation functions determine whether a neuron should be “activated” (answer: yes) or not (answer : no) based on the weighted sum of inputs.

## How activation functions work

Inside a single neuron, two steps occur sequentially:

1. **Linear Transformation**: The neuron computes the weighted sum of its inputs and adds a bias:
    $$
    z = \sum_{i=1}^{n} (w_i \cdot x_i) + b
    $$

2. **Non-linear Activation**: The output $z$ is passed through the activation function $f(z)$:
    $$a = f(z)$$
    where $a$ is the final activation output is sent to the next layer

## Common Activation Functions

### Binary Step Function

The step function is the simplest activation function, historically used in early perceptrons. It outputs a binary decision depending on whether the input crosses a specific threshold (usually $0$).

$$
f(z) = \begin{cases} 0 & \text{if } z < 0 \\ 1 & \text{if } z \ge 0 \end{cases}
$$

- Range: $\{0, 1\}$

- **Use Case**: Early binary classifiers and logical perceptrons.

- **Drawback**: Its derivative is zero everywhere (except at zero, where it is undefined). This means gradient descent cannot work, because the gradient provides no signal on how to update weights during backpropagation. It cannot handle multi-class problems or output probabilities.

### Sigmoid Function

The sigmoid function maps any real-valued number into a smooth range between $0$ and $1$.

$$
\sigma(z) = \frac{1}{1 + e^{-z}}
$$

- **Range**: $(0, 1)$

- **Use Case**: Historically used in the output layer of binary classification problems to represent probabilities.

- **Drawback**: Prone to the vanishing gradient problem. As $z$ becomes very large or very small, the derivative approaches zero, halting gradient descent updates in deep networks.

### Tanh (Hyperbolic Tangent) Function

The tanh function is a scaled and shifted version of the sigmoid function, mapping inputs to a range between $-1$ and $1$.

- Range: $(-1, 1)$

- **Use Case**: Hidden layers of neural networks. Because its output is zero-centered, optimization is often easier compared to sigmoid.

- **Drawback**: It still suffers from the vanishing gradient problem for extreme positive or negative inputs.

### ReLU (Rectified Linear Unit)

ReLU is the default activation function for hidden layers in modern deep learning architectures. It outputs the input directly if it is positive; otherwise, it outputs zero.

$$
f(z) = \max(0, z)
$$

- **Range**: $[0, \infty)$

- **Use Case**: Hidden layers across Convolutional Neural Networks (CNNs) and Deep Feedforward Networks.

- **Advantages**: Computationally efficient and avoids vanishing gradients for positive inputs.

- **Drawback: Dying ReLU problem**—if a large gradient flows through a neuron, its weights can update such that it permanently outputs zero for all future inputs.
### Leaky ReLU

An evolution of ReLU designed to fix the dying ReLU problem by introducing a small, non-zero slope for negative inputs.

$$
f(z) = \max(\alpha z, z)
$$

(where $\alpha$ is a small constant like $0.01$)
- **Range**: $(-\infty, \infty)$
- **Use Case**: Deep networks where standard ReLU neurons are dying prematurely.

### Softmax Function

Unlike element-wise activation functions, Softmax takes a vector of raw scores (logits) and normalizes them into a probability distribution summing to $1$.

$$
\sigma(z)_i = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}
$$

- **Range**: $(0, 1)$ (with the sum across all classes equal to $1$)

- **Use Case**: Exclusively used in the **output layer of multi-class classification models**.

## Step-by-step Numerical Examples

Let's walk through concrete calculations for a single neuron using both the **Binary Step Function** and the **ReLU** activation function.

Given:

- **Inputs** ($x$): $x_1 = 2.0$, $x_2 = -1.5$
- **Weights** ($w$): $w_1 = 0.5$, $w_2 = 1.2$
- **Bias** ($b$): $-0.4$

### Step one: compute the Linear combination ($z$)

$$z = (w_1 \cdot x_1) + (w_2 \cdot x_2) + b$$
$$z = (0.5 \cdot 2.0) + (1.2 \cdot -1.5) + (-0.4)$$
$$z = 1.0 - 1.8 - 0.4$$
$$z = -1.2$$

### example A: Applying the Binary Step Function

$$f(z) = \begin{cases} 0 & \text{if } z < 0 \\ 1 & \text{if } z \ge 0 \end{cases}$$

Since $z = -1.2$, which is less than $0$:

$$f(-1.2) = 0$$

**Result**: The neuron fires a strict $0$ (off). Because the input failed to meet the threshold, the step function completely discards the magnitude of the negative value.

### Example B: Applying the ReLU Function

$$\text{ReLU}(z) = \max(0, z)$$
$$\text{ReLU}(-1.2) = \max(0, -1.2) = 0$$

Like the step function, ReLU outputs $0$ here because the weighted sum is negative. However, if our inputs had resulted in $z = 1.5$, ReLU would output $1.5$, whereas a binary step function would artificially clamp it down to $1$. This capacity to preserve positive magnitudes is why ReLU replaced the step function in deep neural networks.

## comparison Table

| Activation Function | Mathematical Formula | Output Range | Key Advantage | Primary Bottleneck |
| :---: | :---: | :---: | :---: | :---: |
| Binary Step | $$f(z) = \begin{cases} 0 & \text{if } z < 0 \\ 1 & \text{if } z \ge 0 \end{cases}$$ | $\{0, 1\}$ | Simple threshold logic | Zero derivative everywhere; incompatible with backpropagation |
| Sigmoid | $\frac{1}{1 + e^{-z}}$ | $(0,1)$ | Outputs interpretable probabilities | Vanishing gradients, non-zero centered |
| Tanh | $\frac{e^z - e^{-z}}{e^z + e^{-z}}$ | $(-1,1)$ | Zero-centered, stronger gradients than sigmoid | Vanishing gradients | 
| ReLU | $\max(0, z)$ | $[0, \infty)$ | Fast computation, avoids vanishing gradient | Dying ReLU problem | 
| Leaky ReLU | $\max(\alpha z, z)$ | $(-\infty, \infty)$ | Prevents dead neurons for negative values | Extra hyperparameter ($\alpha$) to tune |
| Softmax | $\frac{e^{z_i}}{\sum e^{z_j}}$ | $(0, 1)$ | Converts logits into multi-class probabilities | Sensitive to extreme outliers |