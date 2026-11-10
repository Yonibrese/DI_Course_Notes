# Naïve Bayes

## Understanding Naïve Bayes

**Naïve Bayes** is a family of probabilistic classification algorithms based on **Bayes' Theorem**. It calculates the probability of each class given a set of feature values and predicts the class with the highest probability.

## Why is it called "Naïve"?

It makes a "naïve" assumption that all features are conditionally independent given the class label. While this assumption rarely holds true in the real world (e.g., height and weight are correlated), Naïve Bayes classifiers remarkably outperform expectations and work exceptionally well in practice for text classification, spam detection, medical diagnosis, and customer segmentation.

## The core fomula

$$
    p(C_k|X) = \frac{P(X|C_k) \cdot P(C_k)}{P(X)}
$$

- where:
  - $P(C_k|X)$ is the Posterior Probability: The probability that a data point belongs to class $C_k$ given the features $X$
  - $P(X|C_k)$ Likelihood: The probability of the features given that the data point belongs to class $C_k$.
  - $P(C_k)$ Prior Probability: The baseline probability of the class occurring in the dataset
  - $P(X)$ is the Predictor Prior Probability (Marginal Likelihood): The total probability of the features occurring (acts as a normalizing constant).

## Core Variants:

- **Gaussian NB**: Used when continuous features follow a normal (Gaussian) distribution.

- **Multinomial NB**: Ideal for discrete counts (e.g., word counts in text classification).

- **Bernoulli NB**: Useful for binary/boolean features.

## Real world example

Let's explore a new domain: food science and enology. We will use `scikit-learn`'s built-in Wine dataset to classify the origin/cultivar of different wines based on two chemical components: *Alcohol content* and *Malic acid*.

### Load our packages

We are using `numpy`, `matplotlib`, and `scikit-learn` that trains a Gaussian Naïve Bayes classifier and plots its multi-class decision boundary.

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
```

### load our data set

Load the real-world Wine dataset
We select the first two features (Alcohol and Malic acid) for 2D visualization

```python
data = load_wine()
X = data.data[:, :2]  
y = data.target       # 3 different wine cultivars (classes 0, 1, and 2)
```
### split the Data set

Split the dataset into training (75%) and testing (25%) sets

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)
```

### Initlize and train

Initialize and train the Gaussian Naïve Bayes model

```python
gnb = GaussianNB()
gnb.fit(X_train, y_train)
```

### make predictions

```python
y_pred = gnb.predict(X_test)
```

### Evaluate the model performance

```python
print("--- Model Performance ---")
print(f"Accuracy: {accuracy_score(y_test, y_pred) * 100:.2f}%\n")
print("Confusion Matrix:")
print(confusion_matrix(y_test, y_pred))
print("\nClassification Report:")
print(classification_report(y_test, y_pred, target_names=data.target_names))
```

### plot the Decision Boundary

```python
plt.figure(figsize=(9, 6))

# Create a dense mesh grid to plot the decision regions for multiple classes
x_min, x_max = X[:, 0].min() - 0.5, X[:, 0].max() + 0.5
y_min, y_max = X[:, 1].min() - 0.5, X[:, 1].max() + 0.5
xx, yy = np.meshgrid(
    np.arange(x_min, x_max, 0.05),
    np.arange(y_min, y_max, 0.05)
)

# Predict the class for each point in the mesh grid
Z = gnb.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)

# Plot decision boundary contours and test data scatter points
plt.contourf(xx, yy, Z, alpha=0.3, cmap=plt.cm.Set2)
scatter = plt.scatter(X_test[:, 0], X_test[:, 1], c=y_test, edgecolors='k', cmap=plt.cm.Set2, s=50)
plt.title("Naïve Bayes Decision Boundary (Wine Cultivar Classification)", fontsize=12, fontweight='bold')
plt.xlabel("Alcohol Content", fontsize=10)
plt.ylabel("Malic Acid", fontsize=10)
plt.legend(handles=scatter.legend_elements()[0], labels=list(data.target_names), title="Cultivars")
plt.grid(True, linestyle='--', alpha=0.5)
plt.tight_layout()
plt.show()
```