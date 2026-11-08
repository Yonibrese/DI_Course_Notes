# Machine Learning Evaluation

Once training is complete, you test the model using the **Test Set**—data the model has never seen before. This measures how well the model generalizes to new data rather than just memorizing the training examples.

Evaluating a machine learning model is where theory meets reality. During training, a model optimizes its parameters on data it has already seen. But the true test of intelligence is generalization: how well does it perform on completely unseen data?

To answer this objectively, we rely on quantitative evaluation metrics, robust validation strategies, and diagnostic tools to detect whether our model is learning properly or simply memorizing noise.

## Classifcaion Metrics 

These are the metrics that we use to evaluate Classification models

This is some basic terminology that we use to define the outcome from our ML model:

- **True Positives (TP)**: True Positives are the cases where the model correctly predicts the positive class. In other words, TP is the number of instances where the model correctly identifies that the condition or feature it is looking for does exist.

- **True Negatives (TN)** : True Negatives are the cases where the model correctly predicts the negative class. TN is the count of instances where the model correctly identifies that the condition or feature it is looking for does not exist.

- **False Positives (FP)** : False Positives are the cases where the model incorrectly predicts the positive class. FP happens when the model predicts the presence of a condition or feature when it is actually not present.

- **False Negatives (FN)**: False Negatives are the instances where the model incorrectly predicts the negative class. FN occurs when the model predicts the absence of a condition or feature when it is actually present.

Using these building blocks, we calculate core performance metrics:

## Preforamce Metrics

### Accuracy

Accuracy measures the overall fraction of correct predictions made out of all predictions. 

$$
Accuracy = \frac{TP+TN}{TP+TN+FP+FN}
$$

>[!warning]
>The Trap: Accuracy can be deeply misleading with *imbalanced datasets*. If 99% of your transactions are legitimate and 1% are fraudulent, a lazy model predicting "Legitimate" for everything achieves 99% accuracy while catching zero fraud.

### Precision (Quality of Predictions)

Out of all instances the model predicted as positive, how many were actually positive?

$$
Percision = \frac{TP}{TP+FP}
$$

- When to use: When False Positives are costly
  - filtering spam so legitimate emails aren't sent to the junk folder
  - High-Value Fraud Freezing: Instantly freezing a user's credit card or bank account based on a transaction flag

### Recall / Sensitivity (Quantity of Detections)

Out of all actual positive instances in the dataset, how many did the model manage to find?

$$
Recall = \frac {TP}{TP+FN}
$$

- When to use: When False Negatives are dangerous
  - medical disease screening, where missing a positive case carries severe consequences
  -  Predictive Maintenance for Critical Machinery: Sensors feed data into a classifier predicting whether a jet engine part is about to fail mid-flight.

### F1-Score (The Balance)

Because precision and recall are often in a tug-of-war, the F1-Score calculates their harmonic mean to provide a single balanced metric.

$$
F1-Score = 2 \ast \frac{Precision \ast Recall}{Prescion + Recall} = \frac {2TP}{2TP+FP+FN}
$$

## Regression Metrics 

For regression tasks (predicting numbers like house prices), we measure the magnitude of errors (residuals) between actual values ($y_i$) and predictions ($\hat{y}_i$).

The goal of training a neural network is to minimize this error (making this difference as small as possible)

<!-- Need to expand the regression Metrics section to include Categorical Crossentropy Binary Crossentropy -->

### Mean Squared Error (MSE)

$$
\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2
$$

where:

- $y_i$ = actual value
- $\hat{y}_i$ = predicted value
- $n$ = number of samples

- How it works: Squaring the differences heavily penalizes large mistakes.

- Related metric: Root Mean Squared Error (RMSE) ($\sqrt{\text{MSE}}$), which converts the error back into the original units of your target variable.

### Crossentropy

Crossentropy originates from information theory. It measures the difference between two probability distributions—the true ground truth labels (which are absolute: 0 or 1) and the predicted probabilities outputted by your model.

#### Binary Crossentropy (Log Loss)

Binary crossentropy is used for binary classification tasks where an instance can belong to only one of two classes (e.g., Spam vs. Not Spam, Fraud vs. Legitimate, Sick vs. Healthy). The model outputs a single probability between $0$ and $1$ (using a sigmoid activation function).

- The Formula: For a dataset of $N$ samples, the Binary Crossentropy (BCE) loss is calculated as:
  
  $$
  \text{BCE} = -\frac{1}{N} \sum_{i=1}^{n} \left[ y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i) \right]
  $$

- where:
  - $y_i$ The actual true label for sample $i$ (either $0$ or $1$)
  - $\hat{y}_i$ The predicted probability that sample $i$ is class $1$ (a value between $0$ and $1$).
  - $N$ Total number of samples 

#### Categorical Crossentropy

Categorical crossentropy is used for multi-class classification tasks where an instance belongs to exactly one of three or more mutually exclusive classes (e.g., classifying handwritten digits from 0 to 9, or sorting clothes into categories like Shirt, Pants, Shoes). The model outputs a probability distribution across all classes using a Softmax activation function.

- The formula: To handle multiple classes, ground truth labels are typically one-hot encoded (e.g., class 2 out of 4 becomes [0, 1, 0, 0]). The formula sums across all $N$ samples and all $C$ classes:
  $$
  \text{CCE} = -\frac{1}{N} \sum_{i=1}^{N} \sum_{c=1}^{C} y_{i,c} \log(\hat{y}_{i,c})
  $$

- where:
  - $y_{i,c}$: A binary indicator ($0$ or $1$) if class $c$ is the correct ground truth category for sample $i$.
  - $\hat{y}_{i,c}$: The model's predicted probability that sample $i$ belongs to class $c$.
  - $C$: Total number of classes.
  
## Robust Validation Strategies

A standard single train-test split can be fragile; your evaluation score might depend heavily on which specific data points happened to land in your test set. To ensure reliable performance estimates, we use cross-validation techniques.

### K-Fold Cross-Validation

- How it works: The dataset is divided into $K$ equal-sized subsets (folds)—commonly $K = 5$ or $10$. The model is trained $K$ times. In each iteration, one fold is held out as the validation set, while the remaining $K-1$ folds are combined to train the model. The final performance score is the average across all $K$ trials.

- Why use it: It ensures every data point gets used for both training and validation, dramatically reducing evaluation variance and utilizing data more efficiently.

### Stratified K-Fold Cross-Validation

- How it works: A specialized variation of standard K-Fold designed for classification tasks. It ensures that the proportion of each target class remains approximately equal across every individual fold, matching the distribution of the overall dataset.

- Why use it: Essential when working with **imbalanced datasets** to prevent a situation where one fold accidentally contains zero instances of a rare class.

## Diagnosing Model Performance with Learning Curves
<!-- I want a better example of learning curves -->
How do you know if your model is underfitting (too simple) or overfitting (too complex)? Learning curves plot the model's performance (error or accuracy) on both the training set and the validation set as a function of the training set size (or training epochs).

- Diagnosing Underfitting (High Bias): Both training error and validation error are high and plateau at a poor performance level. The model is simply too simple to capture the underlying patterns.

- Diagnosing Overfitting (High Variance): Training error is extremely low, but validation error remains high. The model has memorized the training data (including noise and outliers) rather than learning generalized rules.
