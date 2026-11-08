# Logisitc Regression

Logistic Regression is one of the most fundamental and widely used supervised machine learning algorithms for binary classification problems. Despite "regression" in its name, it is used for predicting categorical outcomes (e.g., Yes/No, Spam/Not Spam, Pass/Fail, Churn/Retain) rather than continuous numerical values.

## How Logistic Regression Works

### The Linear Combination: 

Like linear regression, it starts by computing a linear combination of the input features:

$$
z = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \dots + \beta_n x_n
$$

where:

- $\beta_0$: is the intercept (bias) term
- $\beta_1 \dots \beta_n$: are the model coefficients for each independent variable $X_1, X_2, ... , X_n$

### The Sigmoid Function

We then convert this linear output (which can range from $-\infty$ to $+\infty$) into a probability score between $0$ and $1$, Logistic Regression passes $z$ through the Sigmoid (Logistic) function:

$$\hat{y} =P(y=1|x) = \frac{1}{1 + e^{-z}}$$

- $e$: is Euler's number (approx. 2.71828).

- $\hat{y}$ : The predicted probability that the given input belongs to the positive class

- Decision Boundary: A threshold (typically $0.5$) is applied. If $\hat{y} \ge 0.5$, the model predicts class 1; otherwise, it predicts class 0

## Real-World Example: Predicting SaaS Customer Conversion

Imagine you are a data scientist at a B2B Software-as-a-Service (SaaS) company. You want to predict whether a trial user will **upgrade to a paid subscription (1)** or **churn (0)** (when a customer or subscriber stops using a company’s product or cancels their service) based on two key behavioral metrics:

 - Feature 1: Total time spent using the app during the trial (in hours).
 - Feature 2: Number of support tickets submitted during the trial.

The following is a complete, end-to-end Python script using NumPy, scikit-learn, and Matplotlib to generate realistic data, train the model, evaluate its performance, and visualize the decision boundary.

### Import the libraries that we will use

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
```

### Generate a realistic synthetic dataset

```python
np.random.seed(42)
n_samples = 300

# Feature 1: Time spent on app (hours, ranged 1 to 30)
app_hours = np.random.uniform(2, 28, n_samples)

# Feature 2: Support tickets submitted (ranged 0 to 6)
support_tickets = np.random.uniform(0, 6, n_samples)

# Generate target labels based on logic: 
# More app hours -> higher chance of buying. More tickets -> often friction/churn, but let's weight app hours positively.
z = 0.35 * app_hours - 0.6 * support_tickets - 4.5
probabilities = 1 / (1 + np.exp(-z))
purchased = np.random.binomial(1, probabilities)

# Combine features into X and target into y
X = np.column_stack((app_hours, support_tickets))
y = purchased
```

### Split to Training and testing sets

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

### Initialize and train the Logistic Regression model

```python
model = LogisticRegression()
model.fit(X_train, y_train)
```

### Make predictions on the test set

```python
y_pred = model.predict(X_test)
```

### Evaluate model performance

```python
print("--- Model Evaluation ---")
print(f"Accuracy: {accuracy_score(y_test, y_pred) *:.2f}%")
print("\nConfusion Matrix:")
print(confusion_matrix(y_test, y_pred))
print("\nClassification Report:")
print(classification_report(y_test, y_pred))
```

### Visualize the Decision Boundary and Test Results

```python
plt.figure(figsize=(10, 6))

# Create a mesh grid for background coloring
x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
xx, yy = np.meshgrid(np.linspace(x_min, x_max, 200),
                     np.linspace(y_min, y_max, 200))

Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)

# Plot decision boundary region
plt.contourf(xx, yy, Z, alpha=0.3, cmap=plt.cm.coolwarm)

# Scatter plot of the test data
scatter = plt.scatter(X_test[:, 0], X_test[:, 1], c=y_test, edgecolors='k', cmap=plt.cm.coolwarm, s=60)
plt.title("Logistic Regression Decision Boundary (SaaS Conversion)")
plt.xlabel("App Usage Time (Hours)")
plt.ylabel("Support Tickets Submitted")
plt.legend(*scatter.legend_elements(), title="Purchased")
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()
```

