# The Landscape of Basic Machine Learning Algorithms

Machine Learning can be broadly classified into three types based on the nature of the learning signal or feedback available to the system: **Supervised Learning**, **Unsupervised Learning**, and **Reinforcement Learning**.

Choosing the right algorithm is a critical turning point in the machine learning lifecycle. Think of algorithms as different types of analytical lenses: some excel at drawing straight lines through continuous data, others at finding complex web-like decision boundaries, others at grouping items by unseen similarities, and others at learning optimal behaviors through trial and error.

The choice depends on three core factors: the nature of your data (labeled vs. unlabeled), your problem objective (predicting values, grouping items, or taking actions), and whether the system learns from static data or active interaction with an environment.

## Supervised Learning

In Supervised Learning, the algorithm learns from a labeled dataset which means that we know the output of the input in advance.

### Regression (Predicting Continuous Numbers)

Regression algorithms predict a continuous numerical value when provided with labeled inputs.

#### Linear Regression

- *How it works*: Fits a straight line (or hyperplane in higher dimensions) that minimizes the distance between predicted values and actual data points.

- *Real-World Example*: **Predicting house prices**. Given features like square footage, number of bedrooms, and location age, the model calculates a dollar value.

- defined by the line formula
  
  $$
  y = mx+b
  $$
  - **y** is the Dependent Variable that Lies along the Y-axis
  - **m** is the Slope of the regression line.
  - **b** is the y-intercept
  - **x** is an Independent Variable that Lies along the X-axis

### Classification

Classification algorithms assign discrete class labels or probabilities to inputs based on labeled training data. classification sorts data into distinct buckets (like "Spam" vs. "Not Spam")

#### Logistic Regression

- *How it works*: Despite its name containing "regression," this is a classification algorithm. It uses a sigmoid function to squash outputs into probabilities between 0 and 1.

- *Real-World Example*: **Email spam detection**. Classifying an incoming email as either "Spam (1)" or "Not Spam (0).

-
  $$
  P(Y=1|X)= \frac{1}{1+e-^(\beta_0+\beta_1X_1+\beta_2X2+...+\beta_nX_n)}
  $$
  - $P(Y=1|x)$ is the probability that the dependent variable equals \(1\) (success).

  - $e$ is is Euler's number (approx. 2.71828).

  - $\beta_0$ is the intercept (bias) term

  - $\beta_1, \beta_2, \dots, \beta_n$ are the model coefficients for each independent variable $X_1, X_2, ... , X_n$

#### Decision Trees & Random Forests

- *How it works*: Decision trees split data into branches using feature thresholds (e.g., Is credit score > 700?). A Random Forest builds hundreds of independent decision trees and averages their votes to deliver robust predictions.

- Real-World Example: Loan approval systems. Evaluating customer credit metrics, employment history, and debt ratios to approve or deny applications.

#### Support Vector Machines (SVM)

- *How it works*: SVM identifies a decision boundary (hyperplane) that separates classes with the maximum possible margin, utilizing "kernel tricks" for non-linear boundaries.

- *Real-World Example*: **Handwritten digit recognition**. Sorting postal zip codes on physical mail.

#### K-Nearest Neighbors (KNN)

- *How it works*: A "lazy learner" algorithm. It doesn't build an explicit internal model; instead, it looks at the $K$ closest data points in the training set and assigns the majority class

- Real-World Example: Product recommendation systems ("Customers who bought items similar to your recent neighborhood purchases also bought...").

#### Naive Bayes

- *How it works*: Based on Bayes' Theorem, it calculates the probability of each class given a set of features, assuming all features are independent of one another.

- *Real-World Example*: Sentiment analysis on customer reviews (classifying a tweet as positive, neutral, or negative).

- core formula
  $$
  p(C_k|X) = \frac{P(X|C_k) \cdot P(C_k)}{P(X)}
  $$

  - $P(C_k|X)$ is the Posterior Probability: The probability that a data point belongs to class $C_k$ given the features $X$

  - $P(X|C_k)$ Likelihood: The probability of the features given that the data point belongs to class $C_k$.

  - $P(C_k)$ Prior Probability: The baseline probability of the class occurring in the dataset

  - $P(X)$ is the Predictor Prior Probability (Marginal Likelihood): The total probability of the features occurring (acts as a normalizing constant).

## Unsupervised Learning

Unsupervised Learning involves training the algorithm with data that is neither classified nor labeled. The system tries to learn the patterns and structures from the data.

### Clustering

Clustering algorithms analyze unlabeled data to discover inherent grouping patterns without pre-defined targets.

#### K-Means Clustering

- *How it works*: Places $K$ center points (centroids) in data space, assigns each point to the nearest centroid, and iteratively recalculates center positions until converged.

- *Real-World Example*: **Customer segmentation**. Grouping store shoppers into buying behavior personas (e.g., bargain hunters, premium subscribers) for marketing.

#### Hierarchical Clustering

- *How it works*: Builds a tree structure (dendrogram) by progressively merging small clusters (agglomerative) or splitting large ones (divisive).

- *Real-World Example*: **Gene expression grouping** and biological taxonomy mapping

### Dimensionality Reduction (Compressing Data)

Dimensionality reduction techniques compress high-dimensional feature spaces into lower dimensions while retaining critical structural information.

#### Principal Component Analysis (PCA)

- *How it works*: Identifies orthogonal axes of maximum variance (principal components) in data and projects high-dimensional coordinates onto them.

- *Real-World Example*: **Image compression & feature extraction**. Reducing pixel dimension size while maintaining core geometric features for facial recognition models.

## Reinforcement Learning (Learning Through Trial & Error)

Unlike **supervised** or **unsupervised** methods, **Reinforcement Learning (RL)** does not rely on a fixed dataset. Instead, an agent learns to make sequential decisions by interacting with an environment. The agent performs actions, receives rewards or penalties, and aims to maximize cumulative long-term reward.

```
+-------------+    Action    +-------------+
|             | ------------>|             |
|    Agent    |              | Environment |
|             | <------------|             |
+-------------+ State/Reward +-------------+
```

### Q-Learning (Value-Based)

- *How it works*: The agent maintains a table (or uses a neural network in Deep Q-Learning) mapping states and actions to expected future rewards (Q-values), continuously updating values as it explores.

- *Real-World Example*: **Game-playing AI**. Teaching an agent to master games like Pac-Man, Atari Breakout, or Chess purely from pixels and score changes.

### Policy Gradient Methods (Policy-Based)

- *How it works*: Directly optimizes the agent's behavior strategy (policy) without mapping explicit value tables, making it effective for continuous control tasks.

- *Real-World Example*: **Robotic motor control**. Training bipedal robots to walk, jump, or manipulate physical objects by rewarding stability and penalizing falls.

### Deep Q-Networks (DQN) & Actor-Critic (Hybrid)

- *How it works*: Combines deep neural networks with RL principles to process complex inputs (like camera feeds or high-dimensional telemetry) to execute control actions.

- *Real-World Example*: **Autonomous vehicles & industrial logistics**. Optimizing self-driving trajectory planning or dynamic HVAC heating/cooling in server data centers to minimize power usage.


## Model Selection Strategy

When determining which algorithm class to deploy:

- Identify the feedback loop (how the data is structured and how you think we should find the desited outcome)

  - Do you have targets/labels? $\rightarrow$ Supervised Learning

  - Do you have raw, unlabeled data needing structure? $\rightarrow$ Unsupervised Learning

  - Do you have an environment where actions produce outcomes? $\rightarrow$ Reinforcement Learning

- Start simple: Establish a baseline (e.g., Linear/Logistic Regression for static data, basic tabular Q-Learning for simple environments).

- Scale up as needed: Progress toward non-linear ensembles (Random Forests, Gradient Boosting) or Deep Reinforcement Learning when baseline capacity is insufficient for problem complexity.