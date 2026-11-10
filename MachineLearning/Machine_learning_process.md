# what is the process of a Machine learning model

Machine learning might seem intimidating with all the math and theory, but at its core, it is a powerful extension of software engineering—shifting from writing explicit instructions to teaching systems how to learn patterns from data.

## what is Machine learning 

>[!note]
>this was coverd already in the octopus guide

To understand machine learning, it helps to contrast it with traditional programming:

- **Traditional Programming**: You feed the computer Data and Rules (`code`), and the program spits out the Answers.

- **Machine Learning**: You feed the computer Data and the desired Answers, and the algorithm figures out the Rules (the model) for you.

Instead of hardcoding rules like "If email contains 'win' and 'million', mark as spam," an ML model looks at thousands of emails and learns the statistical patterns that distinguish spam from legitimate messages on its own.

## 2. The Process of Training an ML Model

Training an ML model is a structured, iterative engineering pipeline. Here are the **6 core phases**:

### 1. Data Collection

An ML model is only as good as the data it learns from. In this phase, you gather raw data relevant to your problem (e.g., historical house prices, customer transaction logs, or text corpora).

- **Key focus**: Ensuring the dataset is large, diverse, and representative of the real-world scenarios the model will face.


### 2. Data Preparation & Preprocessing (Data Cleaning)

>[!note]
>This is mostly the same process that we used with Data analytics 

Raw data is notoriously messy. It contains missing values, duplicates, formatting inconsistencies, and outliers. During preprocessing, you:

- Handle missing data (imputation or removal).

- Convert categorical variables (like text labels) into numerical formats models can process (e.g., one-hot encoding). 
<!-- ask about one-hot encoding examples -->

- Normalize or standardize numerical ranges.

- Feature Selection
  - Decide which features (variables) of the data are relevant to the problem. 
  - Here you will start by considering which features are typically relevant then you will conduct a preliminary analysis of the data.
  - If two features are highly correlated you might choose to keep only one to avoid redundancy.
  - ome features, while theoretically relevant, might not be practical due to data quality or availability

- Split your dataset into a **Training Set** (usually 80%) and a **Test Set** (usually 20%) to evaluate performance later.
  - **Training** is the process where a machine learning model learns from a dataset. This is what you will feed your model so it can learn
  - **Testing** is the process where you evaluate the performance of a model on new, unseen data.

### 3. Model Selection

Depending on your problem type, you select an appropriate algorithm architecture. This could be a simple linear regression model, a complex neural network, decision trees, etc:

- **Supervised Learning**: The data is labeled (e.g., predicting house prices or classifying emails). Examples: Linear Regression, Random Forests, Support Vector Machines.

- **Unsupervised Learning**: The data is unlabeled, and the model looks for hidden structures (e.g., customer segmentation). Examples: K-Means Clustering, Principal Component Analysis.

- **Deep Learning**: Neural networks used for complex tasks like computer vision or natural language processing.

>[!note]
> these are explained in the [ML_types guide](Types_ML.md)

### 4: Training (Fitting) the Model
<!-- ask gemini to expand on training the model (ask to explain the image from ocotopus) -->
This is where the actual "learning" happens.

- You feed the **Training Set** (see above) into your chosen algorithm.

- The model makes predictions, measures its error using a Loss Function (how far off its predictions are from the actual answers), and tweaks its internal parameters (weights and biases) using an Optimizer (like Gradient Descent) to minimize that error.

- This cycle repeats over many passes through the data, known as epochs.

### 5. Evaluation

<!-- ask to break down this section along with the metrics from octopus -->
Once training is complete, you test the model using the **Test Set**—data the model has never seen before. This measures how well the model generalizes to new data rather than just memorizing the training examples.

- Metrics depend on the task: Accuracy, Precision, Recall, or F1-Score for classification; Mean Squared Error (MSE) for regression.

>[!note]
> expalined in detail in [Machine Learning Evaluation Guide](ML_Evaluation.md)

### 6. Deployment, Monitoring or Optimization

If the model’s performance is not satisfactory, you may need to adjust the model (tune its parameters), add more data, or even change the model or preprocessing methods.

or 

Deploy the model and Moniter the results to see if you need to fine tune in the future
