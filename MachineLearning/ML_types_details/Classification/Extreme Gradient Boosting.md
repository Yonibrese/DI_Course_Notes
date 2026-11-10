# Extreme Gradient Boosting

XGBoost, which stands for Extreme Gradient Boosting, is a specialized and enhanced version of the Gradient Boosting technique. It’s like a supercharged version of Gradient Boosting. 

## optimizations from Gradient Boosting

- **Regularization**: Unlike traditional gradient boosting, XGBoost includes $L_1$ (Lasso) and $L_2$ (Ridge) regularization parameters to penalize overly complex models and prevent overfitting.

- **Parallelization**: It constructs trees concurrently by utilizing multi-core processors, drastically speeding up training time.

- **Handling Missing Values**: XGBoost features a built-in routine to handle missing data by automatically learning the optimal default direction for missing values during split-finding.

- **Tree Pruning**: While traditional greedy algorithms stop splitting when a negative loss is encountered, XGBoost uses "max_depth" first and then prunes backward, capturing interaction effects that simpler algorithms miss.

## Real world Example

Let's build a real-world regression model using the California Housing Dataset. We will use `xgboost` (which integrates smoothly with `scikit-learn`), `numpy`, and `matplotlib` to train the model, evaluate its performance, and visualize the results.

### Import our Packages

```python
import matplotlib.pyplot as plt
import numpy as np
from sklearn.datasets import fetch_california_housing
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.model_selection import train_test_split
import xgboost as xgb
```

### Load Data-set

```python
housing = fetch_california_housing()
X, y = housing.data, housing.target
feature_names = housing.feature_names
```

### Split the Data

Split into training (80%) and testing (20%) sets

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42)
```

### Initalize and train the model

```python
model = xgb.XGBRegressor(
    n_estimators=100,
    learning_rate=0.1,
    max_depth=5,
    random_state=42,
    objective="reg:squarederror",
)

model.fit(X_train, y_train)
```

### Evaluate the model

```python
y_pred = model.predict(X_test)
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

print(f"\n--- Model Evaluation ---")
print(f"Mean Squared Error (MSE): {mse:.4f}")
print(f"R-squared (R2) Score:     {r2:.4f}")
```

### Visualize Results using Matplotlib

```python
plt.figure(figsize=(14, 6))

# Plot 1: Actual vs Predicted Values
plt.subplot(1, 2, 1)
plt.scatter(y_test, y_pred, alpha=0.3, color="#2b5c8f", edgecolor="k")
plt.plot(
    [y_test.min(), y_test.max()],
    [y_test.min(), y_test.max()],
    "r--",
    lw=2,
    label="Ideal Fit",
)
plt.xlabel("Actual House Value (in $100k)", fontsize=12)
plt.ylabel("Predicted House Value (in $100k)", fontsize=12)
plt.title("Actual vs. Predicted Housing Prices", fontsize=14, fontweight="bold")
plt.legend()
plt.grid(True, linestyle=":", alpha=0.6)

# Plot 2: Feature Importance
plt.subplot(1, 2, 2)
xgb.plot_importance(
    model,
    ax=plt.gca(),
    max_num_features=6,
    importance_type="gain",
    show_values=False,
    color="#2b5c8f",
)
plt.title("Top Feature Importance (Gain)", fontsize=14, fontweight="bold")
plt.grid(False)

plt.tight_layout()
plt.show()
```