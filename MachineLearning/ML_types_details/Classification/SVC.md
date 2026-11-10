# Support Vector Machines

## understaning SVCs

A Support Vector Classifier (SVC) is a powerful supervised machine learning algorithm used primarily for classification tasks. At its core, SVC aims to find the optimal decision boundary (called a hyperplane) that separates different classes in a dataset with the widest possible margin.

## Key concepts of SVC

- **Support Vectors**: These are the data points that lie closest to the decision boundary. They are critical because they alone define the position and orientation of the hyperplane; removing any other point wouldn't change the model.

- **The Kernel Trick**: Real-world data is rarely linearly separable. SVC uses kernel functions (like Linear, Polynomial, or Radial Basis Function - RBF) to implicitly map input data into higher-dimensional spaces where a linear hyperplane can separate the classes.

## Real world example

Imagine you work as a data scientist for an e-commerce company. Management wants to predict whether a website visitor will purchase a Premium Subscription based on two main features:

1. Age

2. Annual Income

We will simulate this scenario, train an SVC model using an RBF kernel, and visualize the decision boundary and support vectors using Python.

### Import our Packages

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.svm import SVC
from sklearn.preprocessing import StandardScaler
```

### Create our simulated Data

Next we create our simulated Data

```python
np.random.seed(42)
n_samples = 150

# Class 0: Non-buyers (typically younger with lower income)
age_0 = np.random.normal(32, 5, n_samples)
income_0 = np.random.normal(45, 8, n_samples)

# Class 1: Buyers (typically older with higher income)
age_1 = np.random.normal(48, 6, n_samples)
income_1 = np.random.normal(82, 10, n_samples)

# Combine into feature matrix X and target array y
X = np.vstack((np.column_stack((age_0, income_0)), np.column_stack((age_1, income_1))))
y = np.array([0] * n_samples + [1] * n_samples)
```

### Split the data set

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)
```

### Feature scaling

Crucial for SVM convergence and performance

```python
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### Initalize and Train the model

```python
model = SVC(kernel='rbf', C=1.0, gamma='scale')
model.fit(X_train_scaled, y_train)
```
### Test and Evaluate

```python
y_pred = model.predict(X_test_scaled)

print("=== Model Evaluation Metrics ===")
print(f"Accuracy Score: {accuracy_score(y_test, y_pred):.4f}\n")
print("Classification Report:")
print(classification_report(y_test, y_pred))

print("Confusion Matrix:")
print(confusion_matrix(y_test, y_pred))
```

### Plot the Model

```python
# Visualize the Decision Boundary and Test Results
plt.figure(figsize=(10, 6))

# Create a mesh grid to plot the decision regions based on training space
x_min, x_max = X_train_scaled[:, 0].min() - 1, X_train_scaled[:, 0].max() + 1
y_min, y_max = X_train_scaled[:, 1].min() - 1, X_train_scaled[:, 1].max() + 1
xx, yy = np.meshgrid(np.linspace(x_min, x_max, 300),
                     np.linspace(y_min, y_max, 300))

# Predict over the grid
Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)

# Plot the decision boundary and regions
plt.contourf(xx, yy, Z, alpha=0.3, cmap=plt.cm.coolwarm)

# Plot the unseen test customer data points
plt.scatter(X_test_scaled[:, 0], X_test_scaled[:, 1], c=y_test, cmap=plt.cm.coolwarm, edgecolors='k', s=60, label='Test Customers')

# Highlight the Support Vectors from training
support_vectors = model.support_vectors_
plt.scatter(support_vectors[:, 0], support_vectors[:, 1], s=140, facecolors='none', edgecolors='black', linewidth=1.5, label='Support Vectors')

plt.title('SVC Decision Boundary and Test Set Evaluation')
plt.xlabel('Age (Standardized)')
plt.ylabel('Annual Income (Standardized)')
plt.legend(loc='upper left')
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()
```

