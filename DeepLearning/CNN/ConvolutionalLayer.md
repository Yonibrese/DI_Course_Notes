# Confolutional Layer

The Convolutional Layer is the core engine of a CNN. While a traditional neural network treats inputs as flat vectors, a convolutional layer operates directly on spatial grids (like 2D images with height, width, and color channels).

## Key concepts

### Filters (Kernels) and Weights

- A filter is a small matrix of weights (e.g., $3 \times 3$ or $5 \times 5$). Unlike manually engineered filters in traditional computer vision (like Sobel edge detectors), **CNN filters are learnable parameters**.

- During training, backpropagation adjusts these weights so the network automatically discovers which features matter most (e.g., vertical lines, textures, color gradients) for the specific task.

### Handling Channels & Depth

- Real-world color images are 3D tensors with dimensions: Height $\times$ Width $\times$ Channels **(RGB)**.

- A convolutional filter matches this depth. If the input image has 3 channels, a single filter is actually a stack of 3 matrices ($3 \times K_h \times K_w$).

- The filter slides across the 2D spatial grid, performing element-wise multiplications across all channels simultaneously, summing the results into a single scalar value per location. This collapses the channel dimension to produce a **single 2D feature map**.

- The filter slides across the 2D spatial grid, performing element-wise multiplications across all channels simultaneously, summing the results into a single scalar value per location. This collapses the channel dimension to produce a single 2D feature map.

- By using multiple filters (e.g., 32 or 64 filters), the layer outputs a stack of 2D feature maps, creating a new multi-channel tensor for the next layer.

### Stride and Padding

- Stride ($S$): The number of pixels the filter shifts between each local receptive field. A stride of 1 moves the filter pixel by pixel; a stride of 2 skips every other pixel, downsampling the spatial dimensions.

- Padding ($P$): Adding a border of zeros around the input matrix

    - Valid padding ($P=0$): No padding; the output shrinks because filters cannot hang over the border.

    - Same padding: Padding is added so the output spatial dimensions match the input dimensions.

The spatial output size of a convolution can be calculated using the formula:

$$O = \left\lfloor \frac{W - K + 2P}{S} \right\rfloor + 1$$

(where $W$ is input width/height, $K$ is kernel size, $P$ is padding, and $S$ is stride).

### The Activation Function (ReLU)

- Linear combinations alone cannot solve complex problems. After the convolution operation, an element-wise activation function—typically ReLU (Rectified Linear Unit)—is applied:
    
    $$\text{ReLU}(x) = \max(0, x)$$

- This replaces all negative values with zero, introducing non-linearity and allowing the network to learn complex patterns and decision boundaries.

## Python implamentaion

The examples below demonstrate how to initialize and pass data through a single 2D Convolutional layer in both `pytorch` and `TensorFlow`/`Karas`

Important Conceptual Difference:

- **PyTorch** *expects tensors* in `NCHW` format: (`Batch`, `Channels`, `Height`, `Width`).

- **TensorFlow/Keras** *expects tensors* in `NHWC` format: (`Batch`, `Height`, `Width`, `Channels`).

### Pytorch example

```python
import torch
import torch.nn as nn

# Create a dummy image tensor: Batch Size = 1, Channels = 3 (RGB), Height = 32, Width = 32
input_tensor = torch.randn(1, 3, 32, 32)

# Define a 2D Convolutional layer
# in_channels=3, out_channels=16 (16 filters), kernel_size=3, stride=1, padding=1
conv_layer_pt = nn.Conv2d(in_channels=3, out_channels=16, kernel_size=3, stride=1, padding=1)

# Pass input through the layer
output_tensor_pt = conv_layer_pt(input_tensor)

print("PyTorch Input Shape: ", input_tensor.shape)
print("PyTorch Output Shape:", output_tensor_pt.shape) 
# Expected output shape: torch.Size([1, 16, 32, 32])
```

### TensorFlow & Keras Implementation

```python
import tensorflow as tf

# Create a dummy image tensor: Batch Size = 1, Height = 32, Width = 32, Channels = 3 (RGB)
input_tensor = tf.random.normal([1, 32, 32, 3])

# Define a 2D Convolutional layer
# filters=16, kernel_size=3x3, strides=1, padding='same', ReLU activation included
conv_layer_tf = tf.keras.layers.Conv2D(
    filters=16, 
    kernel_size=(3, 3), 
    strides=(1, 1), 
    padding='same', 
    activation='relu'
)

# Pass input through the layer
output_tensor_tf = conv_layer_tf(input_tensor)

print("TensorFlow Input Shape: ", input_tensor.shape)
print("TensorFlow Output Shape:", output_tensor_tf.shape)
# Expected output shape: (1, 32, 32, 16)
```

