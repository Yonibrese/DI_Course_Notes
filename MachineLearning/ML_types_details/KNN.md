# K-Nearest Neighbours (KNN)

## What is K-Nearest Neighbors (KNN)

**KNN** is a non-parametric, lazy-learning algorithm used for both classification and regression.

 - **Non-parametric**: It makes no underlying assumptions about the distribution of the data (unlike Linear Regression, which assumes a linear relationship).

 - **Lazy Learning**: It doesn't learn a discriminative function from the training data during a training phase. Instead, it memorizes the entire dataset and performs computation only when a prediction is requested.

 ## How it works

 1. Choose $K$: Select the number of nearest neighbors ($K$) to consider (e.g., $K=5$).

 2. Calculate Distance: Measure the distance between a new, unseen data point and all points in the training dataset.

 3. Find Neighbors: Identify the $K$ training points closest to the new data point.

 4. Vote/Average:
    - For Classification, the algorithm assigns the class that is most common among its $K$ neighbors (majority vote).
    - For Regression, it calculates the average of the values of the $K$ neighbors.

## Real world Example

Imagine an e-commerce platform that wants to predict whether a customer will subscribe to a Premium Membership based on two features:

1. Age (Years)
2. Monthly Spending ($)

### load the Packages

We are using `numpy`, `matplotlib`, and `scikit-learn`

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score, classification_report
```

### Create and load our dataset

Generate synthetic dataset (Customer Age vs Monthly Spending)
then we need to Rescale features to realistic scales: `Age (20-65)` and `Monthly Spending ($50-$550)`

```python
np.random.seed(42)
X, y = make_classification(
    n_samples=300, 
    n_features=2, 
    n_redundant=0, 
    n_informative=2, 
    n_clusters_per_class=1, 
    weights=[0.6, 0.4], 
    random_state=42
)
#Rescale
X[:, 0] = X[:, 0] * 12 + 40  # Age
X[:, 1] = X[:, 1] * 120 + 250 # Monthly Spending ($)
```

### split our data

Split data into training and testing sets (80% train, 20% test)

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20, random_state=42)
```
### Feature Scaling

CRITICAL for KNN since it relies on distance calculations

```python
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### Initialize and Train the KNN Classifier

```python
k = 5
knn = KNeighborsClassifier(n_neighbors=k, metric='euclidean')
knn.fit(X_train_scaled, y_train)
```

### Evaluate our model

```python
y_pred = knn.predict(X_test_scaled)
print(f"Model Accuracy with K={k}: {accuracy_score(y_test, y_pred) * 100:.2f}%\n")
print("Classification Report:")
print(classification_report(y_test, y_pred, target_names=['Free Tier', 'Premium']))
```

### plot the graph

```python
plt.figure(figsize=(10, 6))

# Create a mesh grid to plot background decision regions
x_min, x_max = X_train_scaled[:, 0].min() - 1, X_train_scaled[:, 0].max() + 1
y_min, y_max = X_train_scaled[:, 1].min() - 1, X_train_scaled[:, 1].max() + 1
xx, yy = np.meshgrid(np.arange(x_min, x_max, 0.1),
                     np.arange(y_min, y_max, 0.1))

Z = knn.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)

# Plot decision regions and training points
plt.contourf(xx, yy, Z, alpha=0.3, cmap=plt.cm.coolwarm)
scatter = plt.scatter(X_train_scaled[:, 0], X_train_scaled[:, 1], c=y_train, cmap=plt.cm.coolwarm, edgecolor='k', s=50)

plt.title(f"KNN Decision Boundary (K = {k}) - Premium Subscription Prediction", fontsize=13, fontweight='bold')
plt.xlabel("Scaled Age", fontsize=11)
plt.ylabel("Scaled Monthly Spending", fontsize=11)
plt.legend(*scatter.legend_elements(), title="Customer Segments", loc="upper left")
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()
```
