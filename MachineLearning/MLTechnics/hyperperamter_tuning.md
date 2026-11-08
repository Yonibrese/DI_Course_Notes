# Hyperparameters and Hyperperamter Tuning

## What are Hyperparameters

In machine learning, **hyperparameters** are configuration settings external to the model whose values cannot be estimated from the data. They are set *before* the training process begins and directly control the training behavior and model architecture (e.g., learning rate in gradient descent, maximum depth of a decision tree, or the number of hidden layers in a neural network).

This contrasts with **model parameters** (like weights and biases), which are learned automatically by the algorithm during training.

## Hyperparameters in Support Vector Machine

- $C$ (Regularization parameter): A lower $C$ allows a wider margin with some misclassifications (better generalization). A higher $C$ tries to classify all training points correctly, risking overfitting.

- $\gamma$ (Gamma): Defines how far the influence of a single training example reaches. High $\gamma$ creates complex, tight boundaries around points (overfitting risk).

- **Kernel** (Kernel Function): Think of the kernel as the type of glasses you wear when you look at your data. Different glasses (kernels) allow you to see different patterns in your data. For example, if you use a “linear” kernel, it’s like looking at your data through clear glasses, and you can see linear relationships between points. If you use a “polynomial” kernel, it’s like using glasses with a specific tint that highlights polynomial patterns in your data.

## Hyperparameters in XGBoost

- **learning_rate**: This hyperparameter determines the step size taken by the optimizer during each iteration of training. A larger learning rate can lead to faster convergence, but it may also increase the risk of overfitting. A smaller learning rate may result in slower convergence but can help prevent overfitting.

- **n_estimators**: This hyperparameter determines the number of boosting trees to be trained. A larger number of trees can improve the model’s accuracy, but it can also increase the risk of overfitting. A smaller number of trees may result in lower accuracy but can help prevent overfitting.

- **max_depth**: This hyperparameter determines the maximum depth of each tree in the ensemble. A larger max_depth can allow the trees to capture more complex relationships in the data, but it can also increase the risk of overfitting. A smaller max_depth may result in less complex trees but can help prevent overfitting.

- **min_child_weight**: This hyperparameter determines the minimum sum of instance weight (hessian) needed in a child node. A larger min_child_weight can help prevent overfitting by requiring more data to influence the splitting of trees. A smaller min_child_weight may allow for more aggressive tree splitting but can increase the risk of overfitting.

- **subsample**: This hyperparameter determines the percentage of rows used for each tree construction. A smaller subsample can improve the efficiency of training but may reduce the model’s accuracy. A larger subsample can increase the accuracy but may make training more computationally expensive.

## Hyperparameter Tuning techniques

### Grid Search

Grid Search (`GridSearchCV`): An exhaustive search through a manually specified subset of hyperparameter values. While thorough, it becomes computationally expensive as the number of hyperparameters and values grows.

```python
from sklearn.datasets import load_breast_cancer
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import GridSearchCV, train_test_split

# Load dataset
data = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(
    data.data, data.target, test_size=0.2, random_state=42
)

# Initialize base model
rf = RandomForestClassifier(random_state=42)

# Define hyperparameter grid
param_grid = {
    'n_estimators': [50, 100, 200],
    'max_depth': [None, 10, 20, 30],
    'min_samples_split': [2, 5, 10],
    'min_samples_leaf': [1, 2, 4],
}

# --- Grid Search ---
grid_search = GridSearchCV(
    estimator=rf, param_grid=param_grid, cv=5, scoring='accuracy', n_jobs=-1
)
grid_search.fit(X_train, y_train)

print('--- Grid Search Best Parameters ---')
print(grid_search.best_params_)
print(f'Best Cross-Validation Accuracy: {grid_search.best_score_:.4f}\n')

```

### Random Serch

As the name suggests, the random search method selects values at random as opposed to the grid search method’s use of a predetermined set of numbers. Every iteration, random search attempts a different set of hyperparameters and logs the model’s performance. It returns the combination that provided the best outcome after several iterations. This approach reduces unnecessary computation.

```python
from sklearn.datasets import load_breast_cancer
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import RandomizedSearchCV, train_test_split

# Load dataset
data = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(
    data.data, data.target, test_size=0.2, random_state=42
)

# Initialize base model
rf = RandomForestClassifier(random_state=42)

# --- Random Search ---
random_search = RandomizedSearchCV(
    estimator=rf,
    param_distributions=param_grid,
    n_iter=10,  # Number of parameter settings sampled
    cv=5,
    scoring='accuracy',
    random_state=42,
    n_jobs=-1,
)
random_search.fit(X_train, y_train)

print('--- Random Search Best Parameters ---')
print(random_search.best_params_)
print(f'Best Cross-Validation Accuracy: {random_search.best_score_:.4f}')
```

### Bayesian Optimization (`Optuna`)

Optuna is one of the most popular and Pythonic frameworks for automated hyperparameter optimization. (Install via `pip install optuna`).

```python
import optuna
from sklearn.datasets import load_breast_cancer
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import cross_val_score, train_test_split

# Load dataset
data = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(
    data.data, data.target, test_size=0.2, random_state=42
)


# Define objective function for Optuna
def objective(trial):
  # Suggest hyperparameter values
  n_estimators = trial.suggest_int('n_estimators', 50, 300)
  max_depth = trial.suggest_int('max_depth', 2, 32, log=True)
  min_samples_split = trial.suggest_int('min_samples_split', 2, 10)
  min_samples_leaf = trial.suggest_int('min_samples_leaf', 1, 5)

  model = RandomForestClassifier(
      n_estimators=n_estimators,
      max_depth=max_depth,
      min_samples_split=min_samples_split,
      min_samples_leaf=min_samples_leaf,
      random_state=42,
  )

  # Evaluate using cross-validation
  scores = cross_val_score(model, X_train, y_train, cv=5, scoring='accuracy')
  return scores.mean()


# Create study and optimize
study = optuna.create_study(direction='maximize')
study.optimize(objective, n_trials=20)

print('--- Optuna Best Parameters ---')
print(study.best_params)
print(f'Best Cross-Validation Accuracy: {study.best_value:.4f}')
```
## Best Practices for Hyperparameter Tuning

- **Always use Cross-Validation**: Evaluate configurations using k-fold cross-validation (`cv=5`) to prevent overfitting to a single validation split.

- **Prevent Data Leakage**: Perform preprocessing steps (like scaling or imputation) inside a pipeline combined with your search strategy rather than globally on the dataset beforehand.

- **Start Broad, Then Refine**: Begin with a Random Search to quickly scan a wide range of values, followed by a targeted Grid Search or Bayesian Optimization around the most promising region.