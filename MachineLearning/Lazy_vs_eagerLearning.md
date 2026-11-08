# Lazy and Eager Learning

In machine learning, the distinction between lazy learning and eager learning comes down to when the model does the heavy lifting: during the training phase or during the prediction (testing) phase.

Here is a detailed breakdown of how they differ, their pros and cons, and when to use each approach.

## Quick Comparison table

| Feature | Eager Learning | Lazy Learning | 
| :---: | :---: | :---: |
| Training Phase | Constructs an explicit general model using training data.| Merely stores the training instances (minimal to no training).|
| Prediction Phase | Fast; uses the pre-built model to classify new data. | Slow; computes distances or similarities against stored data on-the-fly. | 
| Memory / Storage | Usually compact (stores model parameters, not raw data). | High (must store the entire training dataset).
| Adaptability | Hard to update; requires retraining the model for new data. | Highly adaptable; new data can be added instantly.
| Classic Examples | Decision Trees, SVMs, Neural Networks, Naive Bayes | k-Nearest Neighbors (k-NN), Case-Based Reasoning | 

## Eager Learning

An eager learner constructs a generalized, explicit target function (a model) during the training phase before receiving any new or unseen data. Once the training phase is complete, the original training data is typically no longer needed for predictions.

### How Eager Learning works

The algorithm analyzes the entire training dataset to find patterns, establish boundaries, or learn weights. When a test instance arrives, it simply plugs the features into the already-built mathematical model.

### Eager Learning Advantages

- Fast Inference: Predictions are extremely fast because the mapping function is already computed.

- Compact Storage: The system only needs to store the model parameters (e.g., weights, tree structures), which often requires less space than the raw dataset.

### Eager Learning Disadvantages

- Long Training Time: Training can be computationally expensive and time-consuming, especially for large datasets.

- Inflexible Updates: If new data becomes available, the model often has to be retrained from scratch (or incrementally updated via complex retraining pipelines).

### Example Eager Learning ML Types

- Logistic Regression
- Support Vector Machines (SVMs)
- Neural Networks
- Random Forests

## Lazy Learning

A lazy learner delays generalization or model building until a specific query or test instance needs to be classified. Instead of pre-computing a model, it simply memorizes or stores the training dataset.

### How Lazy Learning works

When a new data point is presented, the algorithm searches through the stored training instances, finds the most similar examples, and uses them to make a localized prediction for that specific point.

### Lazy Learning Advantages

- instant Training: There is virtually no training phase (often called "zero training" or "lazy evaluation").

- Natural Adaptability: New training data can be added instantly without needing to retrain a global model—you just add it to the memory bank.

- Complex Decision Boundaries: Because it uses local approximations, it can easily model highly complex, non-linear decision boundaries that global models might miss.

### Lazy Learning Disadvantages

- Slow Predictions: Testing/inference is computationally expensive because the algorithm has to evaluate distances or similarities against the entire training set for every single prediction.

- High Memory Footprint: The algorithm must keep all training data in memory at runtime.

- Curse of Dimensionality: Performance degrades quickly as the number of features increases, as distance metrics become less meaningful in high-dimensional spaces.

### Examples of Lazy Learning ML Types

- k-Nearest Neighbors (k-NN)
- Case-Based Reasoning
- Locally Weighted Regression.

## which should we use

- Use **eager learning** when your primary constraint is prediction speed and you have a stable dataset where you can afford a heavy upfront training phase.

- Use **lazy learning** when your dataset changes frequently, you need real-time data ingestion without retraining cycles, or you are working with smaller datasets where local neighborhoods define the decision space better than a global model.
