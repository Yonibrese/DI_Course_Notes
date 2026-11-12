# Inferential Statistics in Data Science and Machine Learning

## What Is Inferntial Statistics

Understanding Inferential Statistics is essential. While descriptive statistics summarize your historical dataset, inferential statistics allow you to draw conclusions, test assumptions, and make predictions about a broader population based on a limited sample.

In Data Science and Machine Learning, inferential statistics power A/B testing, model validation, feature selection, and generalization guarantees.

## Core Concepts of Inferential Statistics

In Data Science and Machine Learning, inferential statistics is crucial for:

- **Feature Selection**: Determining which features have a statistically significant relationship with the target variable.

- **Model Validation**: Assessing whether a model's performance improvements are genuine or just due to random chance.

- **A/B Testing**: Evaluating whether changes to an application, algorithm, or product yield statistically significant user behavior changes.

### Hypothesis Testing 

Hypothesis testing provides a structured framework for making decisions using data. It involves setting up two competing propositions:

- **Null Hypothesis** ($H_0$): Assumes there is no significant effect, difference, or relationship (e.g., "The new machine learning model does not improve conversion rates compared to the baseline").

- **Alternative Hypothesis** ($H_a$ or $H_1$): Assumes there is a significant effect or relationship (e.g., "The new machine learning model significantly improves conversion rates").

During testing, we risk making two types of errors:

- **Type I Error ($\alpha$)**: Rejecting the null hypothesis when it is actually true (False Positive).

- **Type II Error ($\beta$)**: Failing to reject the null hypothesis when it is actually false (False Negative).


### P-values

The **p-value** (probability value) measures the strength of evidence against the null hypothesis. Specifically, it is the probability of obtaining test results at least as extreme as the results observed, assuming that the null hypothesis is true.

- If $p \leq \alpha$ (commonly $\alpha = 0.05$), we reject $H_0$ and conclude the result is statistically significant.

- If $p > \alpha$, we fail to reject $H_0$.

>[!note]
>ML Context: In data science, p-values are frequently used in feature selection (e.g., in linear regression summaries from libraries like statsmodels) to determine if a feature's coefficient is significantly different from zero.

### Confidence Intervals

A **Confidence Interval (CI)** gives an estimated range of values which is likely to include an unknown population parameter.

- A **95% Confidence Interval** means that if you were to take repeated samples and compute a confidence interval for each, approximately 95% of those intervals would contain the true population parameter.

- **Margin of Error**: Calculated using the standard error and the critical value from a t-distribution or z-distribution.

### python example

Let's look at how to perform a hypothesis test (Independent Two-Sample T-Test) and calculate confidence intervals using Python's `scipy` and `numpy` libraries.

#### Hypothesis Testing & P-Value (Independent T-Test)

```python
import numpy as np
from scipy import stats

# Generate sample latency data for Model A and Model B
np.random.seed(42)
model_a_latency = np.random.normal(loc=45.0, scale=5.0, size=100)
model_b_latency = np.random.normal(loc=42.5, scale=4.8, size=100)

# Perform Independent Two-Sample T-Test
t_stat, p_value = stats.ttest_ind(model_a_latency, model_b_latency)

print(f"T-statistic: {t_stat:.4f}")
print(f"P-value: {p_value:.4f}")

# Decision rule with alpha = 0.05
alpha = 0.05
if p_value < alpha:
    print("Result: Reject the null hypothesis. There is a statistically significant difference in latency.")
else:
    print("Result: Fail to reject the null hypothesis. No significant difference found.")
```

#### Calculating Confidence Intervals

Suppose we want to calculate the 95% confidence interval for the mean accuracy of a machine learning model across 30 cross-validation folds.

```python
import numpy as np
from scipy import stats

# Model accuracy scores across 30 cross-validation folds
cv_accuracies = np.array([
    0.85, 0.88, 0.82, 0.89, 0.87, 0.86, 0.90, 0.84, 0.85, 0.88,
    0.86, 0.87, 0.83, 0.91, 0.86, 0.85, 0.89, 0.88, 0.84, 0.87,
    0.86, 0.88, 0.85, 0.89, 0.87, 0.86, 0.90, 0.85, 0.88, 0.86
])

mean_acc = np.mean(cv_accuracies)
n = len(cv_accuracies)
std_err = stats.sem(cv_accuracies) # Standard Error of the Mean

# Calculate 95% Confidence Interval using the t-distribution
confidence = 0.95
ci_lower, ci_upper = stats.t.interval(
    confidence, 
    df=n-1, 
    loc=mean_acc, 
    scale=std_err
)

print(f"Mean Accuracy: {mean_acc:.4f}")
print(f"95% Confidence Interval: ({ci_lower:.4f}, {ci_upper:.4f})")
```

## F-value and F-test

he F-value is a test statistic that helps determine the overall significance of the regression model. It is a ratio of variances that compares the explained variance (due to the predictors in the model) to the unexplained variance (due to random error). A higher F-value indicates that the model explains a greater portion of the variation in the target variable, suggesting a significant overall model.

How F-Values are Used in Data Science:

### How the F-value and p-value Relate

The F-value and the p-value are inextricably linked; they represent two different ways of looking at the exact same statistical test:

1. The Map to the Distribution: Once you calculate an F-value, you map it onto an F-distribution curve (which depends on your sample size and degrees of freedom).

2. Inverse Relationship: They move in opposite directions. As the F-value increases, the p-value decreases. A massive F-value pushes far out into the tail of the distribution curve, leaving very little probability (a tiny p-value) beyond it.

3. Decision Making: 

- Large F-value $\rightarrow$ Tiny p-value $\rightarrow$ Reject the Null Hypothesis (Your model or groups have a statistically significant effect).

- Small F-value $\rightarrow$ Large p-value $\rightarrow$ Fail to Reject the Null Hypothesis (Any differences are likely just noise).

- **One-Way ANOVA (Analysis of Variance)**: Used to determine whether there are any statistically significant differences between the means of three or more independent groups. A high F-value indicates that the variation among group means is greater than you would expect by chance.

- **Global Significance in Linear Regression**: The overall F-test in regression checks whether at least one predictor variable has a non-zero coefficient. It compares a model with predictors to a null model (intercept only).

- **Feature Selection** (`f_classif` / `f_regression`): In scikit-learn, F-tests are used to score individual features based on their linear dependency with the target variable.

### Python examples for F-tests

#### One-Way ANOVA F-Test (Comparing Multiple Groups)

Suppose you are analyzing three different machine learning model architectures (Model A, B, and C) and want to test if their mean accuracy scores are significantly different.

```python
import scipy.stats as stats

# Sample accuracy scores for three different models
model_a = [0.85, 0.88, 0.86, 0.89, 0.87]
model_b = [0.91, 0.90, 0.93, 0.92, 0.91]
model_c = [0.78, 0.80, 0.79, 0.82, 0.81]

# Perform One-Way ANOVA
f_stat, p_value = stats.f_oneway(model_a, model_b, model_c)

print(f"F-statistic: {f_stat:.4f}")
print(f"P-value: {p_value:.4e}")

if p_value < 0.05:
    print("Reject the null hypothesis: Significant difference in model performances.")
else:
    print("Fail to reject the null hypothesis: No significant difference.")
```

#### Regression F-Test (Evaluating Overall Model Significance)

In linear regression, the F-statistic tells us if our set of independent variables collectively explains a significant amount of variance in the target variable.

```python
import statsmodels.api as sm
import statsmodels.formula.api as smf
import pandas as pd
import numpy as np

# Generate synthetic dataset
np.random.seed(42)
n = 100
x1 = np.random.randn(n)
x2 = np.random.randn(n)
# Target depends on x1 and x2 plus noise
y = 3 * x1 + 2 * x2 + np.random.randn(n)

df = pd.DataFrame({'y': y, 'x1': x1, 'x2': x2})

# Fit Ordinary Least Squares (OLS) model
model = smf.ols('y ~ x1 + x2', data=df).fit()

# Print summary containing the F-statistic and F-pvalue
print(model.summary())

print(f"\nOverall Model F-statistic: {model.fvalue:.4f}")
print(f"Overall Model F p-value: {model.f_pvalue:.4e}")
```

## Confidence Intervals (CIs)

In machine learning, a Confidence Interval (CI) gives a range of values—derived from sample data—that is likely to contain the true performance metric (such as accuracy, precision, or RMSE) of your model with a specified level of confidence (typically 95%)

### Why Confidence Intervals Matter

1. **Accounting for Sampling Variability**: A single point estimate (e.g., "my model has 85% accuracy") only reflects performance on one specific test set. If you split your data differently, that number would change.

2. **Model Comparison**: If Model A has 86% accuracy and Model B has 84%, are they genuinely different, or is the gap just random noise? CIs help answer this by showing the overlap in uncertainty.

3. **Reliability and Risk Assessment**: Stakeholders care about robustness. Knowing that a model's accuracy falls between 81% and 89% with 95% confidence provides a much clearer picture of risk than a single optimistic number.