# Bootstraping

Bootstrapping is a powerful, non-parametric resampling technique used in statistics and machine learning to estimate the accuracy, variance, or confidence intervals of a sample statistic or a machine learning model.

Instead of gathering new independent datasets from the real world—which is often expensive or impossible—bootstrapping creates multiple simulated datasets (called bootstrap samples) from your original dataset by sampling with replacement.

## Key Characteristics

- **Sampling with Replacement**: Each time an observation is drawn from the original dataset of size $N$, it is placed back into the pool. This means a single data point can appear multiple times in a bootstrap sample, while other data points might not appear at all.

- **Out-of-Bag (OOB)** Observations: On average, any given bootstrap sample contains about $63.2\%$ of the original dataset's unique points. The remaining $\approx 36.8\%$ of points are called out-of-bag samples and are often used for validation.

- **Foundation of Ensemble Methods**: Bootstrapping is the "B" in **Bagging** (Bootstrap Aggregating) and forms the backbone of algorithms like **Random Forests**, where multiple models are trained on different bootstrap samples to reduce variance and prevent overfitting.

## Python exampls

### Python example (from scrach)

This example demonstrates how to use bootstrapping to compute a 95% confidence interval for the mean of a small dataset using pure `Python` and `NumPy`.

```python
import numpy as np

# 1. Original dataset (e.g., sample measurements)
data = np.array([45, 50, 52, 58, 60, 63, 70, 75, 80, 95])

# Set random seed for reproducibility
np.random.seed(42)

n_iterations = 1000
n_size = len(data)
bootstrap_means = []

# 2. Perform bootstrapping
for _ in range(n_iterations):
    # Sample with replacement
    bootstrap_sample = np.random.choice(data, size=n_size, replace=True)
    # Calculate the statistic of interest (mean)
    bootstrap_means.append(np.mean(bootstrap_sample))

# 3. Calculate 95% confidence intervals from the bootstrap distribution
lower_bound = np.percentile(bootstrap_means, 2.5)
upper_bound = np.percentile(bootstrap_means, 97.5)

print(f"Original Dataset Mean: {np.mean(data):.2f}")
print(f"Bootstrap 95% Confidence Interval: [{lower_bound:.2f}, {upper_bound:.2f}]")
```

### Scikit-Learn Example

Scikit-learn provides a built-in utility function called `resample` which makes it easy to generate bootstrap samples for both features ($X$) and target labels ($y$) simultaneously while maintaining row alignment.

```python
import numpy as np
from sklearn.utils import resample

# Sample feature matrix X and target array y
X = np.array([[1, 2], [3, 4], [5, 6], [7, 8], [9, 10]])
y = np.array([0, 0, 1, 1, 1])

# Create a single bootstrap sample using sklearn's resample function
X_bootstrap, y_bootstrap = resample(X, y, 
                                    replace=True, 
                                    n_samples=len(X), 
                                    random_state=42)

print("Original X:\n", X)
print("\nBootstrap Sample X (notice duplicated rows like [5,6]):\n", X_bootstrap)
print("\nCorresponding Bootstrap Target y:\n", y_bootstrap)
```