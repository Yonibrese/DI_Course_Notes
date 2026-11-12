# A/B Testing

## unerstanding A/B Testing

When deploying machine learning models into production (e.g., a recommendation system, fraud detector, or click-through rate predictor), data scientists often use a **Champion-Challenger (A/B)** architecture:

- **Model A (Champion)**: The current production model.

- **Model B (Challenger)**: The new, upgraded model (e.g., a newly trained neural network or XGBoost model).

Incoming traffic is randomly split so that user requests are handled by either Model A or Model B. We then track key performance indicators (KPIs)—such as conversion rate, click-through rate, or revenue generated—to see if the challenger statistically outperforms the champion.

## Python Example

Comparing Two ML Models via Live A/B Testing

Let's simulate an A/B test where we compare two recommendation models based on whether users clicked on the recommended product (Binary outcome: 1 = Click, 0 = No Click).

### Import our Packages

```python
import numpy as np
from statsmodels.stats.proportion import proportions_ztest
```

### Create our Test Data

```python
# Simulate live traffic data collected from Model A and Model B
np.random.seed(42)

# Model A (Champion): 10,000 predictions served, 1,250 clicks
# Model B (Challenger): 10,000 predictions served, 1,380 clicks
clicks_A = 1250
nobs_A = 10000

clicks_B = 1380
nobs_B = 10000

# Package data for the test
conversions = np.array([clicks_A, clicks_B])
nobs = np.array([nobs_A, nobs_B])
```

### Calulate Conversion Rates

```python
rate_A = clicks_A / nobs_A
rate_B = clicks_B / nobs_B
print(f"Model A Conversion Rate: {rate_A:.4%}")
print(f"Model B Conversion Rate: {rate_B:.4%}")
```

### Preform a Z-Test

Perform a Two-Proportion Z-Test

```python
z_stat, p_value = proportions_ztest(conversions, nobs)

print(f"\nZ-statistic: {z_stat:.4f}")
print(f"P-value: {p_value:.6f}")
```

### Decide

```python
# Decision rule (Significance level alpha = 0.05)
alpha = 0.05
if p_value < alpha:
    print("Conclusion: Reject the null hypothesis. Model B performs significantly better! Ready to promote to champion.")
else:
    print("Conclusion: Fail to reject the null hypothesis. Keep Model A as champion.")
```

## Z tests

A Z-Test is a statistical hypothesis test used to determine whether two population means (or proportions) are significantly different from each other.

### Key Characteristics

- Used when the sample size is large (typically $n \ge 30$).

- Used when the population variance is known (or estimated accurately with a large sample size via the Central Limit Theorem).

- Follows a standard normal distribution (mean = 0, standard deviation = 1).

### Python example

```python
import numpy as np
from scipy import stats

# Given counts and sample sizes from our ML models above
count_a, n_a = 1250, 10000
count_b, n_b = 1380, 10000

# Sample proportions
p_a = count_a / n_a
p_b = count_b / n_b

# Pooled proportion under the null hypothesis
p_pooled = (count_a + count_b) / (n_a + n_b)

# Standard error of the difference
se = np.sqrt(p_pooled * (1 - p_pooled) * (1/n_a + 1/n_b))

# Calculate Z-score
z_score = (p_b - p_a) / se

# Calculate two-tailed p-value
p_val = 2 * (1 - stats.norm.cdf(abs(z_score)))

print(f"Calculated Z-score: {z_score:.4f}")
print(f"Calculated P-value: {p_val:.6f}")
```

| Feature | Z-test | T-Test | 
| :---: | :---: | :---: |
| Sample Size ($n$) | Large ($n \ge 30$) | Small ($n < 30$) |
| 