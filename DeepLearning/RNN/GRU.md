# Gated Recurrent Units

The Gated Recurrent Unit (GRU), is a specialized variant of Recurrent Neural Networks (RNNs). Like Long Short-Term Memory (LSTM) networks, GRUs were designed to solve the vanishing gradient problem that plagues standard RNNs, enabling models to capture long-term dependencies in sequential data such as text, time-series, and audio.

However, GRUs streamline the architecture: they merge the cell state and hidden state, and they use two gates instead of the LSTM's three. This structural efficiency generally leads to faster training times and fewer parameters while maintaining comparable performance on many tasks.

## How GRUs Work: Architecture and Gates

A GRU controls the flow of information using two gating mechanisms: **the Reset Gate and the Update Gate.**

### The Update Gate ($z_t$)

The update gate helps the model determine how much of the past information (from previous time steps) needs to be passed along to the future. It acts similarly to the combined forget and input gates in an LSTM.

$$
z_t = \sigma(W_z \cdot [h_{t-1}, x_t] + b_z)
$$

- $x_t$: Input vector at the current time step.

- $h_{t-1}$: Hidden state from the previous time step.

- $\sigma$: Sigmoid activation function, squashing values between $0$ and $1$.

- A value close to $1$ retains the past information, while a value close to $0$ discards it.

### The Reset Gate ($r_t$)

The reset gate determines how much of the past information to forget. It dictates how the new input is combined with the previous memory.

$$
r_t = \sigma(W_r \cdot [h_{t-1}, x_t] + b_r)
$$

- If $r_t$ is close to $0$, the model ignores the previous hidden state and focuses solely on the current input $x_t$, effectively "resetting" the memory.

### Candidate Hidden State ($\tilde{h}_t$)

Using the reset gate, the network computes a candidate hidden state that encapsulates the current input combined with relevant past information:

$$
\tilde{h}_t = \tanh(W \cdot [r_t \odot h_{t-1}, x_t] + b)
$$

- $\odot$ denotes element-wise multiplication (Hadamard product).

- The tanh function squashes the output between $-1$ and $1$.

### Final Hidden State ($h_t$)

Finally, the update gate $z_t$ performs a linear interpolation between the previous hidden state $h_{t-1}$ and the candidate hidden state $\tilde{h}_t$:

$$
h_t = (1 - z_t) \odot h_{t-1} + z_t \odot \tilde{h}_t
$$

- If $z_t \approx 1$, the model retains old information and ignores the candidate state.

- If $z_t \approx 0$, the model updates its memory with the new candidate state $\tilde{h}_t$.

## GRU vs. LSTM: Key Differences

| Feature | LSTM | GRU |
| :---: | :---: | :---: |
| Gates | 3 (Input, Forget, Output) | 2 (Reset, Update) |
| State Vectors | Separate Cell State ($C_t$) and Hidden State ($h_t$) | Unified Hidden State ($h_t$) | 
| Parameters | More parameters (higher capacity) | Fewer parameters (faster training) |
| Performance | Better on very long, complex sequences | Often performs equally well on smaller datasets, trains faster |

## Python Example: Building a GRU Model with TensorFlow and Keras

Below is a complete, runnable example using TensorFlow/Keras. This example builds a sequence classification model using synthetic time-series/sequential data of shape `(samples, timesteps, features)`.

### Import our Packages

```python

import numpy as np
import tensorflow as tf
from tensorflow.keras.layers import Dense, Dropout, GRU
from tensorflow.keras.models import Sequential
```

### Generate synthetic sequential data

1000 samples, 20 timesteps per sample, 10 features per timestep

```python
np.random.seed(42)
X_train = np.random.randn(1000, 20, 10).astype(np.float32)
y_train = np.random.randint(0, 2, size=(1000, 1)).astype(np.float32)

# Validation data
X_val = np.random.randn(200, 20, 10).astype(np.float32)
y_val = np.random.randint(0, 2, size=(200, 1)).astype(np.float32)
```

### Build the GRU model

```python
model = Sequential(
    [
        # First GRU layer returning sequences for stacking, or set return_sequences=False for single output
        GRU(
            units=64,
            return_sequences=False,
            input_shape=(20, 10),
            activation="tanh",
            recurrent_activation="sigmoid",
        ),
        Dropout(0.3),  # Regularization to prevent overfitting
        Dense(units=32, activation="relu"),
        Dense(units=1, activation="sigmoid"),  # Binary classification output
    ]
)
```

### Compile the model

```python
model.compile(
    optimizer="adam", loss="binary_crossentropy", metrics=["accuracy"]
)

# Print model architecture summary
model.summary()
```

### Train the Model

```python
history = model.fit(
    X_train,
    y_train,
    validation_data=(X_val, y_val),
    epochs=10,
    batch_size=32,
)
```

### Evaluate on validation set

```python
loss, accuracy = model.evaluate(X_val, y_val)
print(f"\nValidation Loss: {loss:.4f}")
print(f"Validation Accuracy: {accuracy * 100:.2f}%")
```

### Key Considerations for Keras GRU Layers

- `return_sequences=False`: Set to `True` if you are stacking another recurrent layer on top (so it outputs a sequence for every time step) or doing sequence-to-sequence tasks. Set to `False` if you only need the final hidden state output (e.g., for sequence classification).

- `recurrent_activation`: Defaults to `"sigmoid"`, which governs the internal gates ($r_t$ and $z_t$). The primary activation governs the candidate state calculation and defaults to `"tanh"`.