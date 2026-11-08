# Featrue engineering

Feature engineering is often considered the most crucial phase of the machine learning pipeline. While advanced algorithms and deep neural networks are powerful, the quality of your features ultimately dictates the ceiling of your model's performance. As the adage goes: 

>"Garbage in, garbage out."

## Why this is important

Feature engineering is crucial because good features can make a simple model perform better than a complex one with bad features. It’s like giving a detective the right clues to solve a mystery faster. Without good features, models can’t see the hidden patterns in data.

These artificial features are then used by that algorithm in order to improve its performance, or in other words reap better results. Data scientists spend most of their time with data, and it becomes important to make models accurate.

## Data Cleaning & Preparation

Just Like with Data Analytics before we start working with the Data we need to make sure that data is clean, consistant and reliable by  using some of our data cleaning method

### Handling missing data

Decide whether to drop rows/columns with missing data or impute them. Common imputation strategies include mean/median/mode substitution, using a constant value (e.g., 0 or "Unknown"), or leveraging predictive models like KNN imputation. 

Let us look at a simple data set with mising info so we can see how to use these methods with python

```python
import numpy as np
import pandas as pd

# Create sample data
data = {
    'Age': [25, np.nan, 35, 45, 22, 38],
    'Salary': [50000, 60000, np.nan, 80000, 45000, np.nan],
    'Department': ['HR', 'IT', 'HR', np.nan, 'Finance', 'IT'],
}

df = pd.DataFrame(data)
print('Original Dataset:')
print(df)
```

#### Univariate Imputation `SimpleImputer`

`SimpleImputer` replaces missing values using descriptive statistics (like mean, median, or mode) either across each column independently. It is fast, efficient, and ideal as a baseline.

- **Numeric features**: Mean, median, or a constant value.
- **Categorical features**: Most frequent (mode) or a constant string (e.g., `'Missing'`).

```python
from sklearn.impute import SimpleImputer

# Separate numeric and categorical columns
numeric_cols = ['Age', 'Salary']
categorical_cols = ['Department']

# 1. Numeric Imputation (using Median)
num_imputer = SimpleImputer(strategy='median')
df_numeric_imputed = pd.DataFrame(
    num_imputer.fit_transform(df[numeric_cols]), columns=numeric_cols
)

# 2. Categorical Imputation (using Most Frequent)
cat_imputer = SimpleImputer(strategy='most_frequent')
df_categorical_imputed = pd.DataFrame(
    cat_imputer.fit_transform(df[categorical_cols]), columns=categorical_cols
)

# Combine back together
df_simple_imputed = pd.concat([df_numeric_imputed, df_categorical_imputed], axis=1)
print('SimpleImputed Dataset:')
print(df_simple_imputed)
```
#### Local Structure Imputation `KNNImputer`

`KNNImputer` uses the K-Nearest Neighbors algorithm to impute missing values. Instead of looking at a column in isolation, it looks at the $k$ most similar rows (based on Euclidean distance or other metrics) and imputes the missing value using the weighted or unweighted average of those neighbors.

- **Best for**: Datasets where rows share local similarities (e.g., customer segmentation, medical records).
- **Note**: Requires numeric data (categorical features need to be encoded beforehand).

```python
from sklearn.impute import KNNImputer

# Prepare data: convert categorical to numeric dummy variables first
df_encoded = pd.get_dummies(df, columns=['Department'], drop_first=False)

# Initialize KNNImputer (e.g., k=2 nearest neighbors)
knn_imputer = KNNImputer(n_neighbors=2)

df_knn_imputed = pd.DataFrame(
    knn_imputer.fit_transform(df_encoded), columns=df_encoded.columns
)

print('KNNImputed Dataset:')
print(df_knn_imputed)
```

#### MICE (Multivariate Imputation) `IterativeImputer`

MICE (Multivariate Imputation by Chained Equations), implemented in scikit-learn as `IterativeImputer`, models each feature with missing values as a function of other features in a round-robin fashion. It treats missing values as the target variable in a regression or classification model, updating them iteratively until convergence.

- **Best for**: Complex datasets with intricate multivariate relationships between features.

- **Note**: This is an experimental feature in scikit-learn, so you must explicitly enable it before importing.

```python
# IterativeImputer is experimental, must enable first
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer

# Initialize IterativeImputer (uses BayesianRidge regression by default)
mice_imputer = IterativeImputer(max_iter=10, random_state=42)

df_mice_imputed = pd.DataFrame(
    mice_imputer.fit_transform(df_encoded), columns=df_encoded.columns
)

print('MICE / IterativeImputed Dataset:')
print(df_mice_imputed)
```

### Outlier Treatment

Outliers can heavily skew distance-based algorithms (like SVMs or KNN) and linear models. Use statistical bounds (like Z-score or Interquartile Range - IQR) to identify and clip, transform, or remove extreme anomalies depending on domain context.

Let us look at a simple data set with mising info so we can see how to use these methods with python

```python
import numpy as np
import pandas as pd

# Generate sample data with deliberate outliers
np.random.seed(42)
normal_data = np.random.normal(loc=50, scale=10, size=95)
outliers = np.array([105, -15, 120, 5, 135])

df = pd.DataFrame({'income_proxy': np.concatenate([normal_data, outliers])})
```

#### The Z-Score Method

The Z-score measures how many standard deviations a data point is away from the mean.

$$
\text{Z} = \frac{x - \mu}{\sigma}
$$

- **Best used for**: Data that is roughly normally distributed.

- **Common threshold**: Absolute Z-scores greater than $3$ (or $2.5$ for stricter filtering) are typically flagged as outliers.

```python
from scipy import stats

# 1. Calculate absolute Z-scores for the feature
z_scores = np.abs(stats.zscore(df['income_proxy']))

# 2. Define threshold
threshold = 3

# 3. Filter out rows containing outliers
df_filtered_z = df[z_scores < threshold]

print(f'Original rows: {len(df)}, Rows after Z-score filter: {len(df_filtered_z)}')
```

#### The Interquartile Range (IQR) Method

The IQR method relies on percentiles, making it robust against skewed distributions where the mean and standard deviation are heavily dragged by the outliers themselves.

$$
\text{IQR} = Q_3 - Q_1
$$

$$
\text{Lower Bound} = Q_1 - 1.5 \times \text{IQR}
$$

$$
\text{Upper Bound} = Q_3 + 1.5 \times \text{IQR}
$$

- **Best used for**: Skewed data or distributions with heavy tails.

```python
# 1. Calculate Q1 (25th percentile) and Q3 (75th percentile)
Q1 = df['income_proxy'].quantile(0.25)
Q3 = df['income_proxy'].quantile(0.75)
IQR = Q3 - Q1

# 2. Define bounds
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

# 3. Filter out outliers
df_filtered_iqr = df[
    (df['income_proxy'] >= lower_bound) & (df['income_proxy'] <= upper_bound)
]

print(
    f'Original rows: {len(df)}, Rows after IQR filter: {len(df_filtered_iqr)}'
)
```

#### Treatment Strategies: Removal vs. Clipping

Dropping rows entirely can lead to a loss of valuable training data, especially if you have multiple features and outliers overlap across rows. Alternative treatments include:

- Clipping (Winsorization): Capping extreme values at the lower and upper bounds rather than deleting them. This preserves sample size while removing the distortion.

- Transformation: Applying a log or power transformation beforehand to compress the scale.

```python
# Create a copy and clip values to the IQR bounds
df['income_clipped'] = df['income_proxy'].clip(
    lower=lower_bound, upper=upper_bound
)

# Verify the changes
print('Max value before clipping:', df['income_proxy'].max())
print('Max value after clipping:', df['income_clipped'].max())
```

## Feature Engineering

Raw features often violate model assumptions regarding scale, distribution, or data type. Transformations shape the data into a format that algorithms can learn from more effectively.

Let us create some sample data so we can learn some of the standared methods

```python
import numpy as np
import pandas as pd
from sklearn.preprocessing import (
    MinMaxScaler,
    OneHotEncoder,
    OrdinalEncoder,
    PowerTransformer,
    StandardScaler,
)

# Set random seed for reproducibility
np.random.seed(42)

# Create sample dataframe
df = pd.DataFrame(
    {
        'income': np.random.exponential(scale=30000, size=100)
        + 20000,  # Skewed continuous
        'age': np.random.randint(18, 70, size=100),  # Standard continuous
        'education_level': [
            'High School',
            'Bachelor',
            'Master',
            'PhD',
        ]
        * 25,  # Ordinal categorical
        'city': ['New York', 'London', 'Tokyo', 'Paris']
        * 25,  # Nominal categorical
        'target': np.random.choice([0, 1], size=100),  # Binary target
    }
)
```

### Scaling & Normalization

Algorithms sensitive to feature magnitudes (e.g., Gradient Descent, Neural Networks, KNN, SVMs) require features on a comparable scale.

#### Standardization (Z-score) & Normalization

Centers data around a mean of 0 with a standard deviation of 1.

Using `scikit-learn` preprocessors ensures transformations can be safely saved and applied to out-of-sample data.

```python
# Initialize scalers
std_scaler = StandardScaler()
minmax_scaler = MinMaxScaler()

# 1. Standardization: Mean = 0, Std = 1
df['age_standardized'] = std_scaler.fit_transform(df[['age']])

# 2. Min-Max Scaling: Bounded between [0, 1]
df['age_minmax'] = minmax_scaler.fit_transform(df[['age']])

print(
    df[['age', 'age_standardized', 'age_minmax']]
    .head(3)
    .to_string(index=False)
)
```

#### Handling Skewed Distributions

Highly skewed continuous variables (e.g., income, website traffic) can destabilize linear models. Applying a Log Transformation, Box-Cox, or Yeo-Johnson transformation pulls in long tails and stabilizes variance.

Highly skewed variables like income can be compressed using log transformations or power transformers (`Box-Cox` requires strictly positive values; `Yeo-Johnson` handles zero and negative values as well).

```python
# 1. Log Transformation (using log1p to safely handle zeros if any exist)
df['income_log'] = np.log1p(df['income'])

# 2. Power Transformation (Yeo-Johnson works on any real values, Box-Cox requires positive values)
pt = PowerTransformer(method='yeo-johnson')
df['income_boxcox'] = pt.fit_transform(df[['income']])

print(
    df[['income', 'income_log', 'income_boxcox']]
    .head(3)
    .to_string(index=False)
)
```

### Encoding Categorical Variables

Machine learning models require numerical inputs.

so we convert some categorical values to numbers

#### One-Hot Encoding (OHE)

Ideal for low-cardinality nominal variables (e.g., color: red, blue, green). Watch out for the dummy variable trap.
<!-- explain the dummy vairable trap -->
Best for low-cardinality nominal categories (e.g., `city`). Pandas `get_dummies` is great for quick analysis, while `OneHotEncoder` is preferred inside production pipelines.

```python
# Using pandas get_dummies (drops first to avoid the dummy variable trap / multicollinearity)
df_encoded = pd.get_dummies(df, columns=['city'], drop_first=True, dtype=int)

print(df_encoded[['city_London', 'city_New York', 'city_Paris']].head(3))
```

#### Ordinal Encoding

Used when categories possess a natural hierarchical order (e.g., low, medium, high).

```python
# Define custom mapping dictionary
education_mapping = {'High School': 1, 'Bachelor': 2, 'Master': 3, 'PhD': 4}

df['education_ordinal'] = df['education_level'].map(education_mapping)

print(
    df[['education_level', 'education_ordinal']]
    .drop_duplicates()
    .reset_index(drop=True)
)
```

#### Target / Mean Encoding

Replaces categories with the expected value of the target variable, highly effective for high-cardinality categorical features (like zip codes or user IDs), provided proper smoothing is used to prevent data leakage.

Replaces categories with the expected value of the target variable. To prevent severe data leakage, it should be calculated using cross-validation or smoothing techniques. Here is a basic implementation of smoothed target encoding:

```python
def target_encode(
    data, categorical_col, target_col, smoothing=10
):  # smoothing helps with rare categories
    global_mean = data[target_col].mean()

    # Compute aggregation counts and means per category
    stats = data.groupby(categorical_col)[target_col].agg(['count', 'mean'])
    counts = stats['count']
    means = stats['mean']

    # Smooth the estimate using Bayesian updating formula
    smooth_means = (counts * means + smoothing * global_mean) / (
        counts + smoothing
    )

    # Map back to dataframe
    return data[categorical_col].map(smooth_means)


# Apply target encoding to the 'city' feature based on 'target'
df['city_target_encoded'] = target_encode(df, 'city', 'target')

print(df[['city', 'city_target_encoded']].drop_duplicates().reset_index(drop=True))
```

