# Optimizers

## what are optimizers?

In deep learning, an **optimizer** is an algorithm or method used to update the neural network's *weights* and *biases* in order to minimize the loss function. Optimization algorithms drive training, determining how efficiently and effectively a model navigates a complex, high-dimensional loss landscape.

A neural network learns by making predictions and comparing them to the actual results. The difference between predicted and actual values is measured using a loss function (e.g., Mean Squared Error for regression or Cross-Entropy Loss for classification).

The optimizer updates the model’s parameters in a way that reduces this loss over time, improving the accuracy of predictions.

Without an optimizer, the model wouldn’t know how to adjust its weights to improve.

## How do optimizers work

1. Compute the Loss:
    - The model makes predictions.
    - The difference between predictions and actual values is calculated (see the [loss function guide](LossFunctions.md))
2. Compute Gradients (Backpropagation)
    - The optimizer calculates how much each weight contributes to the loss using gradients (partial derivatives of the loss function with respect to each weight).
3. Update Weights and Biases
    - The optimizer adjusts the weights using a learning rule (like Gradient Descent).
    - This step is repeated for multiple iterations (epochs) until the model converges.

## optimizer classes

Optimizers can be grouped into distinct classes based on how they compute gradients and adjust learning rates.

### Gradient Descent (Batch Gradient Descent)

- **How it works**: Batch Gradient Descent computes the gradient of the loss function with respect to the parameters over the entire dataset in a single pass before updating the weights.

- **Characteristics**: 
    - Guaranteed to find the global minimum for convex error surfaces (and a local minimum for non-convex ones) because the gradient points in the exact true direction.

    - **Limitations**: Extremely slow and computationally prohibitive for large modern datasets, as it requires loading all data into memory for every single step.

### Mini-Batch Gradient Descent

- **How it works**: Instead of using the whole dataset or a single random sample, Mini-Batch Gradient Descent splits the dataset into small batches (e.g., 32, 64, 128 samples) and computes gradients on each batch. This is standard Stochastic Gradient Descent (SGD) as used in modern deep learning frameworks.

- **Characteristics**:
    - Provides a balance between the computational efficiency of SGD and the stability of Batch Gradient Descent.
    - The gradient path is noisier than Batch GD, which can actually help the model jump out of shallow local minima or saddle points.

### Momentum-based Optimizers

Standard mini-batch SGD can oscillate wildly when facing steep ravines in the loss landscape, moving slowly along the actual slope toward the minimum. Momentum-based optimizers fix this by adding velocity.

- **Classic Momentum**:

    - Borrows physics concepts: think of a ball rolling down a hill, building up velocity. It adds a fraction ($\beta$) of the previous update vector to the current gradient update, smoothing out oscillations and accelerating progress through flat areas.

- **Nesterov Accelerated Gradient (NAG)**

    - An extension of classic Momentum that "looks ahead." Instead of calculating the gradient at the current position, NAG computes the gradient at the approximate future position ($\theta - \beta v_{t-1}$). This allows the optimizer to anticipate when it is about to overshoot a slope and apply a braking effect early.

### Adaptive Optimizers

Adaptive optimizers adjust the learning rate dynamically for individual parameters based on historical gradients (frequent features get smaller updates, rare features get larger updates).

- AdaGrad (Adaptive Gradient Algorithm):

    - Accumulates the sum of squared past gradients. Parameters with large historical gradients get smaller learning rates.

    - *Limitation*: The running sum keeps growing monotonically, causing the effective learning rate to shrink continuously until training stops prematurely.

- RMSprop (Root Mean Square Propagation):

    - Designed to resolve AdaGrad's diminishing learning rates by replacing the sum of squared gradients with an exponential moving average (discounting older gradients). This keeps the learning rate responsive throughout training.

- Adam (Adaptive Moment Estimation):

    - Combines the best properties of both Momentum (first moment: running average of gradients) and RMSprop (second moment: running average of uncentered squared gradients), complete with bias correction terms for early training steps. It is the classic general-purpose default optimizer.

- AdamW (Adam with Decoupled Weight Decay):

    - Standard L2 regularization implemented in Adam couples weight decay directly with the gradient updates, which performs suboptimally with adaptive learning rates. AdamW decouples weight decay from the gradient update, applying it directly to the weights. It has become the gold standard for training modern architectures like Transformers.

## How to choose the correct optimizer

Choosing the right optimizer depends on your model architecture, data scale, and training budget:

1. **Default Choice for General Prototyping**: Adam or AdamW. If you are unsure where to start, AdamW is typically the safest bet because it converges quickly and handles sparse gradients well across various architectures (CNNs, MLPs).

2. **State-of-the-Art Transformers & LLMs**: AdamW. It is mandatory for training attention-based models (BERT, GPT, Vision Transformers) because proper weight decay regularization is critical for their generalization.

3. **Computer Vision & Fine-Tuning Classic CNNs**: SGD with Momentum (coupled with a good learning rate schedule like cosine annealing). While Adam is fast, well-tuned SGD with momentum often achieves slightly better generalization and higher final accuracy on classic image classification benchmarks.

4. **Recurrent Neural Networks (RNNs) & Reinforcement Learning**: RMSprop or Adam. RMSprop excels at handling non-stationary objectives and recurrent structures where gradients can fluctuate wildly over time.

## Python Examples

```python
import tensorflow as tf
from tensorflow.keras import layers, models
import numpy as np

# 1. Generate some synthetic training data (e.g., binary classification task)
# 1000 samples, 20 features each
X_train = np.random.randn(1000, 20).astype(np.float32)
y_train = np.random.randint(0, 2, size=(1000, 1)).astype(np.float32)

# 2. Build the Artificial Neural Network (ANN) architecture
model = models.Sequential([
    layers.Input(shape=(20,)),              # Input layer expecting 20 features
    layers.Dense(64, activation='relu'),    # Hidden layer 1 with ReLU activation
    layers.Dense(32, activation='relu'),    # Hidden layer 2 with ReLU activation
    layers.Dense(1, activation='sigmoid')   # Output layer for binary classification
])

# 3. Choose and instantiate your optimizer
# Here we use AdamW, but you can swap this with SGD, RMSprop, etc.
optimizer = tf.keras.optimizers.AdamW(
    learning_rate=0.001, 
    weight_decay=0.01
)

# 4. Compile the model by attaching the optimizer, loss function, and metrics
model.compile(
    optimizer=optimizer,
    loss='binary_crossentropy',
    metrics=['accuracy']
)

# 5. Train the ANN model using the optimizer
history = model.fit(
    X_train, 
    y_train, 
    epochs=5,           # Number of complete passes through the dataset
    batch_size=32,      # Mini-batch size
    validation_split=0.2 # Use 20% of training data for validation
)

# 6. Evaluate or make predictions with the trained model
loss, accuracy = model.evaluate(X_train, y_train)
print(f"Training Accuracy: {accuracy * 100:.2f}%")
```

### Swapping Out Different Optimizers

Because Keras decouples the optimizer definition from the model architecture, you can easily swap out optimizers by changing the optimizer in the code above:

>[!note]
>for a full list of optimizers and how to add them look at the [Karas Documentaion](https://keras.io/api/optimizers/)

- SGD with Momentum:

    ```python
    optimizer = tf.keras.optimizers.SGD(learning_rate=0.01, momentum=0.9)
    ```

- Nesterov Accelerated Gradient (NAG)

    ```python
    optimizer = tf.keras.optimizers.SGD(learning_rate=0.01, momentum=0.9, nesterov=True)
    ```

- RMSprop:

    ```python
    optimizer = tf.keras.optimizers.RMSprop(learning_rate=0.001, rho=0.9)
    ```

- Standard Adam:

    ```python
    optimizer = tf.keras.optimizers.Adam(learning_rate=0.001)
    ```