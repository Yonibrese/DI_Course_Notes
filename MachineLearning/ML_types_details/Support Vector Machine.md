# Support Vector Machine (SVM)

## What is a SVMs

A Support Vector Machine is a powerful supervised machine learning algorithm primarily used for classification tasks (though it can also handle regression).

- **The Core Idea**: SVM looks for an optimal decision boundary—called a **hyperplane**—that separates data points of different classes with the **maximum possible** margin.

- **Support Vectors**: These are the data points closest to the decision boundary. They are critical because they define and "support" the orientation and position of the hyperplane. If you remove any other points, the boundary doesn't change; remove a support vector, and it shifts.

- **The Kernel Trick**: Real-world data is rarely linearly separable in its original dimension. SVMs use kernels (like Linear, Polynomial, or Radial Basis Function - RBF) to project data into a higher-dimensional space where a linear hyperplane can easily separate the classes.

## Choosing the right Kernel
Imagine you have a 1D dataset of data points along a straight line where red dots are sandwiched between blue dots: `[Red, Blue, Red]`. A straight line (1D hyperplane) cannot separate the red dots from the blue dots.

However, if you take that line and "bend" or lift it into a 2D space (e.g., squaring the values), the red dots suddenly lift up into the air while the blue dots stay at the bottom. Now, you can easily slice through the 2D space with a flat sheet of paper (a 2D hyperplane) to separate them.

**The Problem**: Explicitly calculating higher-dimensional coordinates for thousands or millions of data points is computationally catastrophic (the "curse of dimensionality").

**The Kernel Solution**: Kernels use mathematical shortcut functions to compute the similarity between data points as if they were in that higher-dimensional space—without actually transforming or computing the coordinates there.

### Linear Kernel (`kernel='linear'`)

- **How it works**: It doesn't transform the data; it finds a straight line, flat plane, or hyperplane in the original feature space.

- Best Used For:
  - Massive datasets where linear separability is likely.
  - **Text classification** and **NLP** (e.g., spam detection, sentiment analysis), where feature spaces are already massive and sparse (tens of thousands of word-count features).

- Pros/Cons: Extremely fast to train and scale, but fails if your data has complex, non-linear patterns.

### Radial Basis Function (RBF) / Gaussian Kernel (`kernel='rbf'`)

- **How it works**: It maps data into an infinite-dimensional Hilbert space. It measures distance similarity, creating bell-shaped "islands" around data points.

- Best Used For: General-purpose, non-linear data when you don't know the underlying distribution. It is the default choice in `scikit-learn` (`SVC()`).

- **Key Hyperparameter** ($\gamma$): Controls the radius of the bell curve.High $\gamma$: Creates tight, isolated pockets around individual data points (risk of overfitting).Low $\gamma$: Creates smooth, broad, generalized decision boundaries (risk of underfitting).

### Polynomial Kernel (`kernel='poly'`)

- **How it works**: Computes relationships based on polynomials of features up to a specified degree ($d$).

- **Best Used For**: Image processing and computer vision tasks where interactions between adjacent pixels or features matter.

- **Key Hyperparameter (`degree`)**: Defines the exponent power (e.g., `degree=3` builds cubic boundaries). Higher degrees create wildly complex, wavy boundaries that easily overfit.

### Sigmoid Kernel (`kernel='sigmoid'`)

- How it works: Mimics a two-layer neural network using hyperbolic tangent functions.

- Best Used For: Mostly historical interest or specialized neural network comparisons; rarely used in modern practical workflows because it behaves poorly compared to RBF under many configurations.

## Real world example

We will focus on classifying two species of iris flowers (Versicolor and Virginica) using two physical measurements (sepal length and sepal width) to clearly visualize how an SVM builds its decision boundary and identifies support vectors.

### Load our Libraries

```python
import matplotlib.pyplot as plt
import numpy as np
from sklearn.datasets import load_iris
from sklearn.metrics import accuracy_score, classification_report
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC
```

### Load and Prep the data set

```python
iris = load_iris()
X = iris.data
y = iris.target

# Filter for two classes (Versicolor [1] and Virginica [2]) and two features (Sepal Length & Width)
mask = y != 0
X = X[mask, :2]
y = y[mask]
y = np.where(y == 1, 0, 1)  # Remap labels to 0 and 1
```

### Split our data set

Split into Training and Testing sets (70% train, 30% test)

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42, stratify=y
)
```

### Feature Scaling

Feature Scaling is Mandatory: SVMs calculate distances between data points. If one feature ranges from 0 to 1,000 and another from 0 to 1, the former will dominate the optimization. Always use `StandardScaler` or `MinMaxScaler`.

```python
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### Initialize and Train

Initialize and Train the SVM Classifier with an RBF Kernel

```python
svm_model = SVC(kernel="rbf", C=1.0, gamma="scale")
svm_model.fit(X_train_scaled, y_train)
```

### Evaluate the model

```python
y_pred = svm_model.predict(X_test_scaled)
print(f"Accuracy: {accuracy_score(y_test, y_pred) * 100:.2f}%\n")
print("Classification Report:")
print(
    classification_report(
        y_test, y_pred, target_names=["Versicolor", "Virginica"]
    )
)
```

### visulize and plot the information

```python
plt.figure(figsize=(10, 6))

x_min, x_max = (
    X_train_scaled[:, 0].min() - 1,
    X_train_scaled[:, 0].max() + 1,
)
y_min, y_max = (
    X_train_scaled[:, 1].min() - 1,
    X_train_scaled[:, 1].max() + 1,
)
xx, yy = np.meshgrid(
    np.arange(x_min, x_max, 0.02), np.arange(y_min, y_max, 0.02)
)

Z = svm_model.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)

# Plot decision boundary region
plt.contourf(xx, yy, Z, alpha=0.3, cmap=plt.cm.coolwarm)

# Plot training points
scatter = plt.scatter(
    X_train_scaled[:, 0],
    X_train_scaled[:, 1],
    c=y_train,
    edgecolors="k",
    cmap=plt.cm.coolwarm,
    s=60,
)

# Highlight Support Vectors (circled in black)
support_vectors = svm_model.support_vectors_
plt.scatter(
    support_vectors[:, 0],
    support_vectors[:, 1],
    s=150,
    facecolors="none",
    edgecolors="black",
    linewidth=1.5,
    label="Support Vectors",
)

plt.xlabel("Sepal Length (Scaled)")
plt.ylabel("Sepal Width (Scaled)")
plt.title(
    "SVM Decision Boundary & Support Vectors (Iris Dataset: Versicolor vs Virginica)",
    fontsize=12,
)
plt.legend(loc="upper right")
plt.colorbar(scatter, label="Classes (0: Versicolor, 1: Virginica)")
plt.tight_layout()
plt.show()
```

