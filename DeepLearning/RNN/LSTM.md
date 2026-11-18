# Long Short-Term Memory

Long Short-Term Memory (LSTM) networks are a specialized type of Recurrent Neural Network (RNN) designed to handle sequential data while overcoming the notorious vanishing and exploding gradient problems that plague standard RNNs

## Why Standard RNNs Fall Short (The problem)

Traditional RNNs process sequences step-by-step, passing a hidden state vector $h_t$ from one time step to the next. During backpropagation through time (BPTT), gradients are multiplied repeatedly across many time steps. If the sequence is long, these gradients either shrink exponentially to zero (vanishing gradient), causing the network to forget early context, or explode to infinity. Consequently, standard RNNs struggle to learn long-term dependencies (e.g., connecting a subject at the beginning of a long paragraph to a verb at the end). 

>[!note]
>Explained more in the [Vanishing gradiant problem guide](vanishing_gradient_problem.md)

## How LSTMs Work: The Core Architecture

An LSTM cell introduces a Cell State ($C_t$), acting like a conveyor belt that runs straight down the entire chain with only minor linear interactions. This allows information to flow easily along it without being unnecessarily degraded.

To regulate what enters and leaves this conveyor belt, LSTMs use three distinct structures called gates built from sigmoid neural net layers and pointwise multiplication operations:

### The Forget Gate ($f_t$)

- **Purpose**: Decides what information to *throw away* from the previous cell state ($C_{t-1}$).

- **Mechanism**: It looks at the previous hidden state ($h_{t-1}$) and current input ($x_t$), passing them through a sigmoid function to output numbers between $0$ and $1$ for each number in the cell state $C_{t-1}$. A value of $0$ means "completely get rid of this," while $1$ means "keep this entirely."

- **Equation**: $f_t = \sigma(W_f \cdot [h_{t-1}, x_t] + b_f)$

### The Input Gate ($i_t$) & Candidate State ($\tilde{C}_t$)

- **Purpose**: Decides what new information we are going to store in the cell state.

- **Mechanism**:

    - The input gate (a sigmoid layer) decides which values we'll update: $i_t = \sigma(W_i \cdot [h_{t-1}, x_t] + b_i)$.

    - A `tanh` layer creates a vector of new candidate values, $\tilde{C}_t$, that could be added to the state: $\tilde{C}_t = \tanh(W_c \cdot [h_{t-1}, x_t] + b_c)$.

- **Updating the Cell State**: The old state $C_{t-1}$ is updated into the new cell state $C_t$ by multiplying the old state by $f_t$ (forgetting what's irrelevant) and adding $i_t * \tilde{C}_t$ (adding the scaled new information):
$C_t = f_t * C_{t-1} + i_t * \tilde{C}_t$

### The Output Gate ($o_t$)

- **Purpose**: Decides what the next hidden state ($h_t$)—and thus the output—will be, based heavily on the filtered cell state.

- **Mechanism**: A sigmoid layer decides what parts of the cell state make it to the output, followed by pushing the cell state through a `tanh` (to push values between $-1$ and $1$) and multiplying it by the sigmoid gate output.

- **Equations**:
$$
o_t = \sigma(W_o \cdot [h_{t-1}, x_t] + b_o)
$$
$$
h_t = o_t * \tanh(C_t)
$$

## python example Time-Series Forecasting

Below is a complete, runnable example using `tf.keras` to build, compile, and train an LSTM model on synthetic 3D sequential data (shape: `[samples, timesteps, features]`).

### Import our Packages

```python
import numpy as np
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import LSTM, Dense
```

### Generate synthetic sequential data

- Shape: 1,000 samples, 10 timesteps per sample, 5 features per timestep

```python
num_samples = 1000
timesteps = 10
features = 5

X = np.random.random((num_samples, timesteps, features))
# Binary classification target (e.g., trend up/down)
y = np.randint(2, size=(num_samples, 1))
```

### Build the LSTM model using the Sequential API

```python
model = Sequential([
    # LSTM layer with 64 hidden units
    # return_sequences=False because we only want a single output at the end of the sequence
    LSTM(64, activation='tanh', input_shape=(timesteps, features)),
    
    # Dense hidden layer for feature extraction
    Dense(32, activation='relu'),
    
    # Output layer with a sigmoid activation for binary classification
    Dense(1, activation='sigmoid')
])
```

### Compile the model

```python
model.compile(
    optimizer='adam',
    loss='binary_crossentropy',
    metrics=['accuracy']
)

# Display model architecture summary
model.summary()
```

### Train the Model

```python
print("\nTraining LSTM model...")
model.fit(
    X, y,
    epochs=5,
    batch_size=32,
    validation_split=0.2
)
```

### Generate predictions on new sequence data

```python
sample_input = np.random.random((1, timesteps, features))
prediction = model.predict(sample_input)
print(f"\nPrediction probability: {prediction[0][0]:.4f}")
```

### Key Keras LSTM Parameters to Know

- `units`: Dimensionality of the output space (number of memory cells/neurons in the layer).

- `return_sequences`: Set to `True` if you are stacking another LSTM layer on top and need to pass the full sequence of outputs (`[batch, timesteps, units]`) instead of just the final time step's output (`[batch, units]`).

- `return_state`: Set to `True` if you also need to explicitly extract the final hidden state ($h_t$) and cell memory state ($C_t$) alongside the outputs.
