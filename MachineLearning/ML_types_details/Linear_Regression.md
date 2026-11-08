# Linear Regression 

At its core, linear regression models the relationship between a dependent target variable ($y$) and one or more independent feature variables ($X$).

## The Mathematical Model

For a single feature, the equation of the line is defined as:

$$
y = mx+b
$$

- $y$: The predicted target value (e.g., House Price).
- $x$: The input feature (e.g., Square Footage).
- $m$ (somtimes written as $w$): The slope (or weight), representing how much $y$ changes for a unit change in $x$.
- $b$: The bias (or intercept), representing the expected value of $y$ when $x = 0$.

## Cost Function & Optimization

To find the best-fitting line, the algorithm minimizes the Mean Squared Error (MSE), which acts as our cost function:

$$
MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2
$$

where:

- $y_i$ = actual value
- $\hat{y}_i$ = predicted value
- $n$ = number of samples

In standard ***scikit-learn*** implementations, this is solved analytically using Ordinary Least Squares (OLS), meaning it calculates the exact mathematical solution for $w$ and $b$ instantly without needing iterative loops.

## basic example

Below is a complete, step-by-step example using NumPy for data generation, scikit-learn (sklearn) to train the model, and Matplotlib to visualize the results.


### 1. import our packages

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score
```
if they are not installed in your enviorment (virtual or otherwise) install the packages using `pip install`

>[!note]
>the *sklearn* package is installed as `pip install scikit-learn`

### 2. Create our data

This is a "Real world" inspired dataset that we are generating

```python
X = np.array([[12], [15], [18], [20], [22], [25], [28], [30], [35], [40]])
y = np.array([155, 185, 215, 245, 275, 310, 345, 370, 430, 495])
```

- `X`: Size of the house in hundreds of sq. ft. (e.g., 15 = 1,500 sq. ft.)

- `y`: Price of the house in thousands of dollars (e.g., 180 = $180,000)

### 3. Initialize and train the model

```python
model = LinearRegression()
model.fit(X, y)
```

### 4. Extract learned parameters

```python
weight = model.coef_[0]
bias = model.intercept_

print(f"Learned Weight (Price increase per 100 sq. ft.): ${weight * 1000:,.2f}")
print(f"Learned Bias (Base baseline price): ${bias * 1000:,.2f}")
```

### Make predictions

Now that we have a model that is trained on our data lets make predictions 

```python
houses_to_predict = np.array([[16], [32]])
predicted_prices = model.predict(houses_to_predict)

for size, price in zip(houses_to_predict, predicted_prices):
    print(f"Predicted price for a {size[0]*100} sq. ft. house: ${price*1000:,.2f}")

# Generate line coordinates for plotting the fit this will be used later
X_range = np.array([[10], [42]])
y_pred_line = model.predict(X_range)
```

- Let's predict prices for a 1,600 sq. ft. house (16) and a 3,200 sq. ft. house (32)

### Evaluate the model performance

```python
y_train_pred = model.predict(X)
r2 = r2_score(y, y_train_pred)
rmse = np.sqrt(mean_squared_error(y, y_train_pred))

print(f"\nModel Evaluation:")
print(f"R-squared (R²): {r2:.4f} (Explains {r2*100:.1f}% of price variance)")
print(f"Root Mean Squared Error: ${rmse * 1000:,.2f}")
```
When assessing how well your linear regression model performs on real-world data, look at two key metrics:

- $R^2$ Score (Coefficient of Determination): Ranges from $0$ to $1$. An $R^2$ of $0.95$ means that $95\%$ of the variance in house prices is explained by the square footage feature alone.
-  Root Mean Squared Error (RMSE): Tells you the average deviation of your model's predictions from the actual target values, measured in the original units (dollars).

### Plot the model

```python
plt.figure(figsize=(9, 6))
plt.scatter(X * 100, y * 1000, color='royalblue', s=70, edgecolors='k', label='Actual Home Sales')
plt.plot(X_range * 100, y_pred_line * 1000, color='crimson', linewidth=2.5, label='Linear Regression Fit')

# Highlight specific predictions
plt.scatter(houses_to_predict * 100, predicted_prices * 1000, color='darkorange', s=100, zorder=5, label='New Predictions')

plt.title('Real Estate Market: House Price vs. Square Footage', fontsize=13, fontweight='bold')
plt.xlabel('Square Footage (sq. ft.)', fontsize=11)
plt.ylabel('Price ($)', fontsize=11)
plt.legend(fontsize=10)
plt.grid(True, linestyle='--', alpha=0.5)
plt.tight_layout()

plt.show()
```


