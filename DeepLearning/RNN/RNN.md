# Recurrent Neural Network

Recurrent Neural Networks (RNNs) are a foundational class of artificial neural networks designed specifically for processing sequential data—data where the order of elements matters, such as text, speech, time-series measurements, and genomic sequences.

Unlike standard Feedforward Neural Networks (like MLPs or CNNs) which assume all inputs are independent of each other, RNNs possess a form of "memory." They process sequences one element at a time while maintaining an internal hidden state that captures information about everything seen so far.

##  How RNNs work

The defining characteristic of an RNN is a hidden state ($h_t$), which acts as the network's internal memory.

At every time step $t$, the RNN takes two things as input:

1. The current input data at step $t$ ($x_t$).

2. The hidden state from the previous time step ($h_{t-1}$).

These inputs are combined and passed through an activation function (typically $\tanh$) to compute the new hidden state:

$$
h_t = \tanh(W_{hh} h_{t-1} + W_{xh} x_t + b_h)
$$

Where:

- $W_{hh}$ and $W_{xh}$ are shared weight matrices.

- $b_h$ is the bias term.

The new hidden state $h_t$ is then used to generate the output at the current step ($y_t$) and is also passed forward as memory to the next time step ($t+1$). Because the same weight matrices ($W_{hh}$ and $W_{xh}$) are reused across every single time step, the network can process sequences of arbitrary length using a fixed number of parameters.

## Key concepts

### Parameter Sharing

In a standard Fully Connected (Dense) neural network, every single input feature at a specific position has its own unique weight. If you fed a sequence of 50 words into a standard dense layer, the model would learn one set of weights for word 1, a completely different set of weights for word 2, and so on.

An RNN takes the opposite approach. It uses the exact same weight matrices ($W_{xh}$ for inputs and $W_{hh}$ for hidden states) across every single time step of the sequence. Whether the network is evaluating the 2nd word of a sentence or the 200th word, it applies the exact same mathematical rules to update its memory.

#### Why Parameter Sharing Matters

**Temporal Invariance (Pattern Recognition Anywhere)**

In language, time-series data, or audio, meaning shouldn't depend on absolute position.

- Consider the sentence: *"The fierce dog barked"* versus *"Yesterday, out in the quiet park, a fierce dog barked"*.

- The relationship between the words **"fierce"** and **"dog"** (an adjective modifying a noun) is identical regardless of where they appear in the sentence.


Because an RNN uses the same weights at every step, it learns a universal transition rule. Once the network learns how the concept of an adjective modifies a noun, it can instantly apply that exact same logic at step 50. It achieves temporal invariance—recognizing patterns regardless of where they happen to occur in a sequence.

**Handling Sequences of Arbitrary Lengths**

Real-world data varies wildly in length. A customer service chat log might be 5 messages long or 500 messages long; a stock price history might cover 30 days or 5 years.

If a network required unique weights for every position, you would have to fix your sequence length in advance. If you trained a model on sentences of 20 words, it would utterly fail or crash if given a 21-word sentence. Parameter sharing frees the network from sequence-length constraints: the same fixed set of weights can unroll for 5 steps or 5,000 steps seamlessly.

**Massive Reduction in Parameters (Preventing Overfitting)**

Parameter efficiency is a game-changer for model training.

- If a model had to learn separate weights for every time step in a 100-step sequence, the parameter count would explode into the millions almost instantly, leading to severe **overfitting** (where the model memorizes specific training sequences instead of learning general rules).

- By reusing the same weights, an RNN keeps its parameter count remarkably small. The model only needs to learn how to update its hidden state from one step to the next, rather than learning a completely separate history for every distinct position in time.

**Computational Efficiency**

Fewer parameters mean fewer computations per forward and backward pass. This drastically reduces memory overhead and training time, allowing models to process long sequential datasets on standard hardware without running out of memory.

### Unrolling Through Time

Conceptually, visualizing an RNN involves "unrolling" it across time. A single recurrent cell is duplicated for each item in the sequence, showing how the hidden state flows from step 1 to step $T$.

When you write code for an RNN, you define a single, compact recurrent cell (e.g., a single `SimpleRNN` layer). However, to understand how data flows and how the network learns, it helps to conceptualize the network "**unrolled**" across time.

Unrolling means taking that single recursive loop and duplicating it horizontally for every time step in your sequence. If your input sequence has 20 words or 20 time steps, unrolling transforms the network into a chain of 20 identical modules linked sequentially:

1. At step 1: The cell takes input $x_1$ and an initial hidden state $h_0$ (usually initialized to zeros) to produce output $y_1$ and hidden state $h_1$.

2. At step 2: The cell takes input $x_2$ and passes the previous hidden state $h_1$ to compute $y_2$ and $h_2$.

3. At step $T$: This continues down the chain until the final step $T$, where the hidden state $h_T$ encapsulates a compressed memory of the entire sequence.

By unrolling the loop, an RNN temporarily looks and acts like a very deep feedforward neural network, where each "layer" represents a single point in time.

### Backpropagation Through Time (BPTT)

Training a standard feedforward neural network relies on standard backpropagation: errors are calculated at the output layer and passed backward through the network layers to update the weights.

In an RNN, because time replaces depth, training relies on an extension called Backpropagation Through Time (BPTT). Here is how it works step-by-step:

1. The Forward Pass

    The network processes the sequence from $t=1$ to $t=T$, generating predictions at each step (or a final prediction at the end) while passing the hidden states forward.

2. Calculating the Loss

    The network compares its final prediction (or predictions at each step) against the true target labels using a loss function (like Cross-Entropy or Mean Squared Error).

3. The Backward Pass (Flowing Backward Through Time)

    - The error at step $T$ calculates how much responsibility the final hidden state $h_T$ had in the mistake.

    - That error is then passed backward to step $T-1$, influencing $h_{T-1}$, and so on, traveling retrograde down the unrolled chain.

### The Vanishing Gradient Problem

Standard RNNs struggle to retain long-term dependencies. During backpropagation through time (BPTT), gradients can shrink exponentially as they flow backward over many time steps, making it difficult for the network to connect words or events separated by long gaps. (This limitation led to the development of specialized architectures like **LSTMs** and **GRUs**, which use gating mechanisms to preserve long-range memory).

## Python Implamentation

Here is a complete, runnable example using TensorFlow and Keras. We will build a simple `SimpleRNN` model to classify sequences (e.g., binary sentiment classification on padded sequences).

```python
import numpy as np
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Embedding, SimpleRNN, Dense

# 1. Generate Synthetic Sequential Data
# Let's assume we have 1,000 training samples, 
# each sequence is 20 time steps long, with integer-encoded vocabulary features.
num_samples = 1000
max_features = 10000  # Size of vocabulary
sequence_length = 20

# Random integer sequences (representing words/tokens)
X_train = np.random.randint(0, max_features, size=(num_samples, sequence_length))
# Binary labels (0 or 1) for each sequence
y_train = np.random.randint(0, 2, size=(num_samples, 1))

print(f"Input shape (X_train): {X_train.shape}")
print(f"Labels shape (y_train): {y_train.shape}")

# 2. Build the RNN Model
model = Sequential([
    # Embedding layer: maps integer word indices into dense vectors of fixed size (32)
    Embedding(input_dim=max_features, output_dim=32, input_length=sequence_length),
    
    # SimpleRNN layer: 64 hidden units
    SimpleRNN(units=64, activation='tanh'),
    
    # Output layer for binary classification
    Dense(units=1, activation='sigmoid')
])

# 3. Compile the Model
model.compile(
    optimizer='adam',
    loss='binary_crossentropy',
    metrics=['accuracy']
)

model.summary()

# 4. Train the Model
print("\nTraining the RNN model...")
model.fit(X_train, y_train, epochs=3, batch_size=32)

# 5. Make Predictions on New Sequences
sample_input = np.random.randint(0, max_features, size=(1, sequence_length))
prediction = model.predict(sample_input)
print(f"\nPrediction probability for sample: {prediction[0][0]:.4f}")
```

