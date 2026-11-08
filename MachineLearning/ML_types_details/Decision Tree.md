# Decision Tree

## How Decision Trees Work

A decision tree consists of:

- **Root Node**: The starting point representing the entire dataset, which gets split based on the most informative feature.

- **Decision Nodes**: Intermediate nodes representing features and conditional splits (e.g., Is Age > 30?).

- **Leaves (Terminal Nodes)**: The final classification outcomes or class labels.

To determine the best splits, algorithms like CART (Classification and Regression Trees) use impurity metrics such as Gini Impurity or Information Gain (Entropy) to measure how well a split separates the classes.

## Real world example

Imagine a bank wanting to predict whether a loan applicant is a **High Risk (likely to default)** or **Low Risk (likely to repay)** based on two applicant attributes:

1. Credit Score (normalized/scaled)
2. Annual Income (normalized/scaled)

### Load our Packages

We are using `numpy`, `matplotlib`, and `scikit-learn` to build, train, evaluate, and visualize the decision tree.

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.metrics import accuracy_score, classification_report
```

### Generate our Data

Generate synthetic real-world data (Applicant Attributes vs. Loan Risk)

```python
X, y = make_classification(
    n_samples=400, 
    n_features=2, 
    n_redundant=0, 
    n_informative=2, 
    n_clusters_per_class=1, 
    random_state=42
)
```

### Split the Data

Split data into training (80%) and testing (20%) sets

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

### Initalize and Train the Model

We set max_depth=3 to prevent overfitting and keep the tree interpretable

```python
clf = DecisionTreeClassifier(criterion='gini', max_depth=3, random_state=42)
clf.fit(X_train, y_train)
```

### Evaluate

```python
y_pred = clf.predict(X_test)
print(f"Model Accuracy: {accuracy_score(y_test, y_pred) * 100:.2f}%\n")
print("Classification Report:")
print(classification_report(y_test, y_pred, target_names=['Low Risk (Repaid)', 'High Risk (Default)']))
```

### plot 

```python
plt.figure(figsize=(14, 6))

# Subplot 1: The Decision Tree Structure
plt.subplot(1, 2, 1)
plot_tree(
    clf, 
    filled=True, 
    feature_names=['Credit Score', 'Annual Income'], 
    class_names=['Low Risk', 'High Risk'],
    rounded=True
)
plt.title("Decision Tree Structure", fontsize=14)

# Subplot 2: Decision Boundaries
plt.subplot(1, 2, 2)
x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
xx, yy = np.meshgrid(np.arange(x_min, x_max, 0.01),
                     np.arange(y_min, y_max, 0.01))
Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)

plt.contourf(xx, yy, Z, alpha=0.4, cmap=plt.cm.coolwarm)
plt.scatter(X[:, 0], X[:, 1], c=y, edgecolor='k', cmap=plt.cm.coolwarm, s=40)
plt.title("Feature Space & Decision Boundaries", fontsize=14)
plt.xlabel("Credit Score (Scaled)")
plt.ylabel("Annual Income (Scaled)")

plt.tight_layout()
plt.show()
```