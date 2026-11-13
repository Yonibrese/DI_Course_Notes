# Forward Propagation

## what is forward Propagation

Forward propagation is the foundational process in artificial neural networks where input data flows forward—from the input layer, through any number of hidden layers, and finally to the output layer—to generate a prediction.

Think of it as a factory assembly line: raw materials (data) enter at one end, pass through multiple stations where specific transformations occur, and a finished product (the model's prediction) emerges at the other end.

## How it works

At its core, forward propagation relies on two main mathematical operations performed at every single neuron:

1. Linear Transformation: Computes the weighted sum of its inputs plus a bias term [Precptron Guide](./Perceptron.md)

2. Non-Linear Activation Function: Passes $z$ through an activation function ($f$) to introduce non-linearity, enabling the network to learn complex patterns[Activation functions Guide](./activationFunction.md)

### Real world example

Let's walk through a concrete example: predicting whether a credit card transaction is fraudulent using a neural network.

#### inputs (input layer)

Suppose our network evaluates two normalized features from an incoming transaction:

- $x_1 = 0.85$ (Transaction amount relative to the user's normal spending—e.g., a unusually large purchase)

- $x_2 = 0.90$ (Geographic distance from the user's last known location)

#### Extracting Features (Hidden Layer)

In the hidden layer, neurons combine these inputs to detect higher-level behavioral patterns. Let's look at two hidden neurons:

- Neuron A (Detects "Unusual Spending Spike"): It places heavy weight on the transaction amount: $w_1 = 0.9, w_2 = 0.1$, with a bias of $-0.15$.
    $$
    z_A = (0.85 \times 0.9) + (0.90 \times 0.1) - 0.15 = 0.765 + 0.09 - 0.15 = 0.705
    $$
    Applying a ReLU activation function ($\max(0, z)$), the output remains $0.705$.

- Neuron B (Detects "Impossible Travel / Location Anomaly"): It balances both distance and amount to catch card cloning abroad: $w_1 = 0.4, w_2 = 0.8$, with a bias of $-0.2$.
    $$
    z_B = (0.85 \times 0.4) + (0.90 \times 0.8) - 0.2 = 0.34 + 0.72 - 0.2 = 0.86
    $$
    After ReLU activation ($\max(0, z)$), the output is $0.86$.

#### The Final Prediction (Output Layer)

The output layer takes the activations from Neuron A and Neuron B ($0.705$ and $0.86$), multiplies them by their respective output weights (say, $w_{out1} = 1.2$ and $w_{out2} = 1.5$), adds an output bias ($b_{out} = -0.5$), and applies a Sigmoid activation function to squash the final score into a probability between $0$ and $1$:

$$
z_{out} = (1.2 \times 0.705) + (1.5 \times 0.86) - 0.5 = 0.846 + 1.29 - 0.5 = 1.636
$$

$$\hat{y} = \sigma(1.636) = \frac{1}{1 + e^{-1.636}} \approx 0.84$$

The network outputs $0.84$, meaning there is an $84\%$ probability that the transaction is fraudulent—triggering an automated block or security alert.

### What Happens Next? 

Once forward propagation finishes and produces a prediction ($\hat{y}$), the network compares it against the actual outcome ($y$ — whether the transaction was truly fraud or legitimate) using a loss function to calculate the error. That error is then sent backward through the network during [**backpropagation**](./BackwardPropagation.md) to update the weights and biases, making the model sharper for future transactions.
