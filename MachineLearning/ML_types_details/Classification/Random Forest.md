# Random Forest

## What is a Random Forest

A Random Forest is an ensemble learning method primarily used for classification and regression tasks. It operates by constructing a multitude of Decision Trees during training and outputting the class that is the mode of the classes (classification) or mean prediction (regression) of the individual trees.

## How it works

- **Bootstrap Aggregating (Bagging)**: The algorithm creates multiple subsets of the original dataset with replacement (bootstrap samples). Each subset is used to train a separate decision tree.

    > [!note]
    > we will go over bootstraping and Bagging later

- **Feature Randomness**: When building each tree, instead of looking at all features to find the best split, the algorithm selects a random subset of features at each node split. This decorrelates the trees, preventing overfitting and making the forest much more robust than a single decision tree.

- **Majority Voting**: For classification, every tree "votes" for a class label, and the class with the most votes wins.

## Real world example

To make this concrete, let's look at a classic business problem: Customer Churn Prediction.

Imagine you work for a subscription-based streaming service. You want to predict whether a customer is likely to cancel their subscription (Churn = 1) or stay (Churn = 0) based on behavioral features like:

- Monthly charges
- Total tenure (months with the company)
- Support tickets logged per month
- Average streaming hours per week

### Load our Packages

we are using `numpy`, `matplotlib`, and `scikit-learn` that simulates this data, trains a Random Forest classifier, evaluates its *performance*, and visualizes the *Feature Importances*.

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import make_classification
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score
```

### Create our Dataset

Generate a realistic synthetic dataset (Customer Churn Scenario)

Features: Tenure, Monthly Charges, Support Tickets, Streaming Hours, etc.

```python
X, y = make_classification(
    n_samples=2000, 
    n_features=5, 
    n_informative=4, 
    n_redundant=1, 
    random_state=42
)

feature_names = [
    "Tenure (Months)", 
    "Monthly Charges ($)", 
    "Support Tickets/Mo", 
    "Streaming Hours/Wk", 
    "Age"
]
```

### Split our data 
Split the data into training (80%) and testing (20%) sets
```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

### Initlize and train the Random Forest Classifier

```python
rf_classifier = RandomForestClassifier(
    n_estimators=100,      # Number of trees in the forest
    max_depth=6,           # Maximum depth of each tree
    random_state=42,
    n_jobs=-1              # Use all available CPU cores
)

rf_classifier.fit(X_train, y_train)
```

### make predicitons

Make predictions from the test set

```python
y_pred = rf_classifier.predict(X_test)
```

### Evaluate the predictions 

```python
print("--- Model Evaluation ---")
print(f"Accuracy Score: {accuracy_score(y_test, y_pred):.4f}\n")
print("Classification Report:")
print(classification_report(y_test, y_pred, target_names=["Retained", "Churned"]))
```

### plot Feature Importances using Matplotlib

```python
importances = rf_classifier.feature_importances_
indices = np.argsort(importances)[::-1]

plt.figure(figsize=(10, 6))
plt.title("Random Forest - Feature Importance in Customer Churn", fontsize=14, fontweight='bold')
plt.bar(range(X.shape[1]), importances[indices], color='dodgerblue', align='center')
plt.xticks(range(X.shape[1]), [feature_names[i] for i in indices], rotation=25, ha='right')
plt.ylabel("Relative Importance", fontsize=12)
plt.xlabel("Features", fontsize=12)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()
```

