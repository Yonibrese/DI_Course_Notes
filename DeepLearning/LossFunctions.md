# Loss functions

## What are loss functions?

Loss functions are the mathematical compass of machine learning and deep learning models. They quantify the penalty or error between a model's predictions ($\hat{y}$) and the true target values ($y$). During training, optimization algorithms (like gradient descent) use the gradients of these loss functions to update model weights and minimize error.

The goal of training a neural network is to minimize this error

>[!note]
>The loss functions are also used in normal machine learning as a part of the evaluation step in training your ML models

## Common Loss functions

### Regression Loss Functions

Regression models predict continuous numerical values. The primary choices balance sensitivity to outliers and mathematical smoothness. (Linear Regression)

#### Mean Squared Error (MSE) / L2 Loss

- **Intuition**: Computes the average of the squared differences between predicted and actual values. Because the errors are squared, large errors are penalized disproportionately heavily, making the model sensitive to outliers.

- **Mathematical Formula**:

    $$
    MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2
    $$

- **Variable Breakdown**:

    - $n$: Total number of data points (samples) in the dataset or batch.

    - $i$: The index of the current sample being evaluated (from $1$ to $n$).

    - $y_i$: The true target value (ground truth) for the $i$-th sample.

    - $\hat{y}_i$: The predicted numerical value produced by the model for the $i$-th sample.

- **Use Case**: Standard regression tasks where large errors are particularly undesirable (e.g., house price prediction).

- **Code Example (NumPy)**

    ```python
    import numpy as np

    y_true = np.array([3.0, -0.5, 2.0, 7.0])
    y_pred = np.array([2.5, 0.0, 2.0, 8.0])

    mse = np.mean((y_true - y_pred) ** 2)
    print(f"MSE: {mse}")
    ```

>[!note]
>In most cases we can extract the results from the training model object but here we have a basic Python example on how to calculate these errors

#### Mean Absolute Error (MAE) / L1 Loss

- **Intuition**: Computes the average of the absolute differences between predictions and targets. Unlike MSE, errors are weighted linearly, making MAE robust to outliers. However, its gradient is constant, which can cause slow or unstable convergence near the minimum.

- **Mathematical Formula**:

    $$
    MAE = \frac{1}{n} \sum_{i=1}^{n} \vert{}y_i - \hat{y}_i\vert{}
    $$

- **Variable Breakdown**:

    - $n$: Total number of data points (samples) in the dataset or batch.

    - $i$: The index of the current sample being evaluated (from $1$ to $n$).

    - $y_i$: The true target value for the $i$-th sample.

    - $\hat{y}_i$: The predicted numerical value produced by the model for the $i$-th sample.

    - $\vert{} \cdot \vert{}$: The absolute value operator, ensuring all error magnitudes are positive regardless of whether the prediction was too high or too low.

- **Use Case**: Regression problems with significant noise or outliers where you do not want extreme errors to dominate training.

- **Code Example (NumPy)**:

    ```python

    mae = np.mean(np.abs(y_true - y_pred))
    print(f"MAE: {mae}")

    ```

#### Huber Loss

- **Intuition**: Combines the best properties of MSE and MAE. It acts like MSE when the error is small (ensuring smooth convergence around zero) and like MAE when the error is large (limiting the influence of outliers).

- **Mathematical Formula**:

    $$L_\delta(y, \hat{y}) = \begin{cases} \frac{1}{2}(y - \hat{y})^2 & \text{for } \vert{}y - \hat{y}\vert{} \le \delta \\  \delta \left(\vert{}y - \hat{y}\vert{} - \frac{1}{2}\delta\right) & \text{otherwise} \end{cases}$$

- **Variable Breakdown**:

    - $y$: The true target value for a single sample.

    - $\hat{y}$: The predicted numerical value for that sample.

    - $\delta$ (delta): A hyperparameter threshold that defines the boundary where the loss transitions from quadratic (MSE) to linear (MAE).

    - $\vert{}y - \hat{y}\vert{}$: The absolute residual (error magnitude) between the ground truth and prediction.

- **Use Case**: Robust regression tasks prone to outliers (e.g., robust linear regression).

- **Code Example (PyTorch)**:

    ```python
    import torch
    import torch.nn as nn

    loss_fn = nn.HuberLoss(delta=1.0)
    output = loss_fn(torch.tensor([2.5, 0.0, 2.0, 8.0]), torch.tensor([3.0, -0.5, 2.0, 7.0]))
    print(f"Huber Loss: {output.item()}")
    ```

### Classification Loss Functions

Classification models map inputs to discrete class labels or probabilities.

#### Binary Cross-Entropy (BCE) / Log Loss

- **Intuition**: Measures the performance of a classification model whose output is a probability value between $0$ and $1$. As the predicted probability diverges from the actual label, the penalty increases logarithmically.

- **Mathematical Formula**:

    $$
    BCE = -\frac{1}{n} \sum_{i=1}^{n} \left[ y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i) \right]
    $$

- **Variable Breakdown**:

    - $n$: Total number of samples in the dataset or batch.

    - $i$: The index of the current sample being evaluated.

    - $y_i$: The true binary label for the $i$-th sample, which is strictly either $0$ (negative class) or $1$ (positive class).

    - $\hat{y}_i$: The predicted probability (ranging between $0$ and $1$) that the $i$-th sample belongs to the positive class ($1$).

    - $\log$: The natural logarithm (base $e$).

- **Use Case**: Binary classification problems (e.g., spam detection, medical diagnosis).

- **Code Example (PyTorch)**:

    ```python
    import torch.nn.functional as F

    # Predicted probabilities and true binary labels
    y_pred = torch.tensor([0.9, 0.1, 0.8], dtype=torch.float32)
    y_true = torch.tensor([1.0, 0.0, 1.0], dtype=torch.float32)

    bce_loss = F.binary_cross_entropy(y_pred, y_true)
    print(f"BCE Loss: {bce_loss.item()}")
    ```

#### Categorical Cross-Entropy (CCE)

- **Intuition**: The multi-class generalization of Binary Cross-Entropy. It evaluates the divergence between the predicted probability distribution across $C$ classes and the true distribution (typically represented as a one-hot encoded vector).

- **Mathematical Formula**:

    $$
    CCE = -\frac{1}{n} \sum_{i=1}^{n} \sum_{c=1}^{C} y_{ic} \log(\hat{y}_{ic})
    $$

- Variable Breakdown:

    - $n$: Total number of samples in the dataset or batch.

    - $i$: The index of the current sample (from $1$ to $n$).

    - $C$: Total number of possible classes.

    - $c$: The index of the current class being evaluated (from $1$ to $C$).

    - $y_{ic}$: Binary indicator ($0$ or $1$) denoting whether class $c$ is the true correct classification for sample $i$. In one-hot encoding, this equals $1$ for the true class and $0$ for all others.

    - $\hat{y}_{ic}$: The model's predicted probability (from $0$ to $1$, usually after a Softmax activation) that sample $i$ belongs to class $c$.

- **Use Case**: Multi-class classification where classes are mutually exclusive (e.g., ImageNet classification).

- Code Example (PyTorch):

    ```python
    
    import torch.nn.functional as F

    # Logits output from model (before Softmax) and class indices
    logits = torch.tensor([[2.0, 1.0, 0.1], [0.1, 2.5, 0.4]])
    targets = torch.tensor([0, 1]) # Class 0 for sample 1, Class 1 for sample 2

    cce_loss = F.cross_entropy(logits, targets)
    print(f"Categorical Cross-Entropy: {cce_loss.item()}")

    ```

#### Hinge Loss

- **Intuition**: Designed primarily for Support Vector Machines (SVMs). It penalizes predictions not just for being wrong, but for not being confident enough (falling within the decision margin).

- **Mathematical Formula**:

    $$
    L(y, \hat{y}) = \max(0, 1 - y \cdot \hat{y})
    $$

- **Variable Breakdown**:

    - $y$: The true class label for the sample, formatted strictly as either $-1$ or $+1$.

    - $\hat{y}$: The raw, unnormalized output score (distance from the decision boundary) produced by the classifier, not a probability.

    - $1$: The required margin threshold. If $y \cdot \hat{y} \ge 1$, the prediction is both correct and outside the margin, resulting in $0$ loss.

    - $\max(0, \dots)$: A function that clips negative values to $0$, ensuring that correctly classified points beyond the margin incur no penalty.

- **Use Case**: Maximum-margin classification algorithms like linear SVMs.

- Code Example (PyTorch):

    ```python
    # Predictions (raw margins) and labels in {-1, 1}
    y_pred = torch.tensor([0.8, -0.5, 1.2])
    y_true = torch.tensor([1.0, -1.0, 1.0])

    # Manual calculation matching the formula above
    hinge_loss = torch.mean(torch.clamp(1 - y_true * y_pred, min=0))
    print(f"Hinge Loss: {hinge_loss.item()}")
    ```

## Quick refernce Table

| Task Type | Loss Function | Key Characteristic | Common Framework Name|
| :---: | :---: | :---: | :---: |
| Regression | MSE (L2) | Heavily penalizes large errors, sensitive to outliers. | `MSELoss` / `mean_squared_error` |
| Regression | MAE (L1) | Robust to outliers, constant gradients. | `L1Loss` / `mean_absolute_error` |
| Regression | Huber Loss | Combines MSE near zero and MAE for large errors. | `HuberLoss` |
| Binary Classification | Binary Cross-Entropy | Logarithmic penalty for probabilistic outputs. | `BCELoss` / `BCEWithLogitsLoss` |
| Multi-Class Classification | Categorical Cross-Entropy | Expects logits + one-hot or class indices. | `CrossEntropyLoss` |
| Classification (SVM) | Hinge Loss | Encourages a strict decision boundary margin. | `HingeEmbeddingLoss` |

