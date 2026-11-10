# Gradient Boosting

Gradient Boosting is one of the most powerful and widely used machine learning techniques for structured or tabular data. It belongs to the family of **ensemble methods**, which combine multiple "weak learners" (typically shallow decision trees) to create a single, highly accurate "strong learner."

Unlike Random Forests—which build trees in parallel using *bagging* (bootstrap aggregating)—Gradient Boosting builds trees *sequentially*. Each new tree is trained specifically to correct the errors (residuals) made by the previous sequence of trees.

## How Gradient Boosting Works

1. **Initial Prediction**: The model starts with a baseline prediction, usually the average value of the target variable for regression, or log-odds for classification.

2. **Compute Residuals**: For every data point, the model calculates the pseudo-residuals (the negative gradient of the loss function with respect to the current predictions). If we are using Mean Squared Error (MSE), these residuals are simply the difference between the actual and predicted values ($y - \hat{y}$).

3. **Train a Weak Learner**: A shallow decision tree is fit to these residuals rather than the original target variable.

4. **Update Predictions**: The predictions of the new tree are scaled by a learning rate (shrinkage) and added to the ensemble's running total.

5. **Iterate**: Steps 2–4 are repeated for a specified number of estimators or until convergence.


## Real world example

To make the mechanics crystal clear, let's build a 1D regression example from scratch using `NumPy`, `Scikit-Learn`'s Decision Trees, and `Matplotlib`. We will visualize how sequential trees fit the residuals and gradually approximate a complex non-linear relationship (house price vs. square footage). 

In practice, you don't need to code the loop manually. `Scikit-Learn` provides optimized classes like `GradientBoostingRegressor` and `GradientBoostingClassifier`:

### Import our Packages

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.tree import DecisionTreeRegressor
```

### Create Synthetic Real-World

```python
np.random.seed(42)
n_samples = 150
X = np.sort(np.random.uniform(500, 4500, n_samples)) # Square footage
# True underlying relationship (non-linear) with some noise
y = 50000 + 150 * X + 50000 * np.sin(X / 500) + np.random.normal(0, 15000, n_samples)

X = X.reshape(-1, 1)
```

### Manual example 

the following is an example to show how to Gradiand boosting works with python and how we gradually approximate a complex non-linear relationship

#### define our Hyperparameters

```python
n_estimators = 5
learning_rate = 0.3
```

#### Initialize the model

we next Initilize the model and create an inital (weak) prodiction

```python
# Initial prediction is the mean of y
init_pred = np.mean(y)
predictions = np.full(shape=y.shape, fill_value=init_pred)
```

#### polt our inital Prediction

to visualize how our predictions change over time we will plot our inital prediction and compair it to later conditions

```python
# Store trees for later visualization
trees = []

plt.figure(figsize=(14, 10))

# Plot initial state
plt.subplot(3, 2, 1)
plt.scatter(X, y, color='black', alpha=0.6, s=20, label='Actual Data')
plt.plot(X, predictions, color='red', lw=2, label=f'Initial Pred (Mean)')
plt.title('Step 0: Initial Baseline')
plt.legend(loc='upper left', fontsize=8)
```

#### Boosting loop

```python
for i in range(n_estimators):
    # Calculate residuals (negative gradient for MSE)
    residuals = y - predictions
    
    # Fit a weak decision tree to the residuals
    tree = DecisionTreeRegressor(max_depth=3, random_state=42 + i)
    tree.fit(X, residuals)
    trees.append(tree)
    
    # Update predictions using learning rate shrinkage
    update = tree.predict(X)
    predictions += learning_rate * update
    
    # Plotting the progression
    plt.subplot(3, 2, i + 2)
    plt.scatter(X, y, color='black', alpha=0.4, s=15, label='Actual Data')
    plt.plot(X, predictions, color='blue', lw=2, label=f'Ensemble after Tree {i+1}')
    plt.title(f'Boosting Iteration {i+1}')
    plt.legend(loc='upper left', fontsize=8)
```

#### Final ploting 

```python
plt.tight_layout()
plt.show()
```

### `Scikit-Learn`'s Production-Ready Implementation

we will use the same dataset that we created above as well as the packages from above

#### Import Packages

we need to import the appropreate additional packages

```python
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score
```

#### Split our data

split our data into training(80%) and testing(20%) datasets

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

#### Initalize and fit the model

```python
gbr = GradientBoostingRegressor(
    n_estimators=100, 
    learning_rate=0.1, 
    max_depth=3, 
    random_state=42
)
gbr.fit(X_train, y_train)
```

#### Evaluate the model

```python
y_pred = gbr.predict(X_test)
print(f"R^2 Score: {r2_score(y_test, y_pred):.4f}")
print(f"RMSE: {np.sqrt(mean_squared_error(y_test, y_pred)):.2f}")
```