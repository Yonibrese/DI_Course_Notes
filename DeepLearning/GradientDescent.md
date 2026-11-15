# Gradient Descent

## What is Gradient Descent

At its core, **Gradient Descent** is an iterative optimization algorithm used to find the minimum of a function. In machine learning and deep learning, it is the engine behind training models: it takes a model with random initial parameters (weights and biases), measures how wrong its predictions are using a loss function, and systematically adjusts those parameters to make the predictions as accurate as possible.

## How it works

1. Start with random weights and biases.

2. Calculate the loss (how wrong the predictions are) with the current weights and biases.

    - This uses the Loss function (seen in our [loss function guide](LossFunctions.md))

3. Adjust the weights and biases to reduce the loss by calculate the gradient of the loss with respect to each parameter.

4. we adjust the weights and biases with the following fomula
    $$
    w_{\text{new}} = w_{\text{old}} - \alpha \frac{\partial Loss}{\partial w}$$

    - $\alpha$ learning rate: is a hyperparameter that controls how large of a step we take on each iteration

        - **Too high**: You might overshoot the bottom entirely, bouncing back and forth or even diverging.

        - **Too low**: Your steps are microscopic, meaning training will take forever or get stuck.

5. Repeat steps 2-4 until the loss is minimized.

## The Three Main Flavors of Gradient Descent

| Variant | Data Used per Step | Pros | Cons |
| :---: | :---: | :---: | :---: |
| Batch Gradient Descent | Entire dataset | Exact, smooth path toward the minimum | Extremely slow or memory-prohibitive for massive datasets. |
| Stochastic Gradient Descent (SGD) | Single random data point | Fast updates; can escape shallow local minima due to noise. | High variance; loss bounces around erratically instead of a smooth descent. |
| Mini-Batch Gradient Descent | Small batches (e.g., 32, 64, 128 samples) | The industry standard; balances computational efficiency with stable convergence. | Requires tuning batch size |

