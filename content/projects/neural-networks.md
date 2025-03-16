---
title: Neural Networks
description: Mathematics Behind Neural Networks
authors:
  - name: Pierre Graef
    to: https://twitter.com/benjamincanac
    avatar:
      src: https://i.pravatar.cc/128?u=2
image:
  src: img/article_neural_networks/cover_neural_networks.png
date: 2024-10-10T00:00:00.000Z
---

## Neural Networks: A Technical Overview

Neural networks are models inspired by the human brain, designed to recognize complex patterns in data. They are at the core of deep learning, a branch of machine learning, and are used in various applications such as image recognition, text generation, and more.

### 1. Structure of a Neural Network

A neural network is composed of several layers:

- **Input layer**: Each neuron in this layer represents a feature of the dataset.
- **Hidden layers**: These intermediate layers capture non-linear relationships between input features.
- **Output layer**: This layer provides the model's predictions or results.

### 2. Activation Functions

An activation function introduces the non-linearity required to model complex relationships. Below are some common activation functions and their mathematical expressions:

- **Sigmoid**
  :latex{latex="\[\sigma(x) = \frac{1}{1 + e^{-x}} \]"}
  It constrains outputs between 0 and 1, but suffers from saturation at extreme values.
- **ReLU (Rectified Linear Unit)**
  :latex{latex="\[f(x) = \max(0, x)\]"}
  It's simple to compute and helps avoid gradient saturation issues, but it may cause "dead" neurons.
- **Tanh (Hyperbolic Tangent)**
  :latex{latex="\[\tanh(x) = \frac{e^{x} - e^{-x}}{e^{x} + e^{-x}}\]"}
  Values range between -1 and 1, but like sigmoid, it can also saturate.

### 3. Learning Algorithm: Backpropagation

Training a neural network involves using the backpropagation algorithm. This process relies on computing the gradient of the errors with respect to the network's weights through the **gradient descent** algorithm. Here are the main steps:

**Forward pass:** The data passes through the network layer by layer, and the output is calculated.

::inline
**Error calculation**: A loss function :latex{latex="\(J(\theta)\)"} , such as Mean Squared Error (MSE) or Cross-Entropy, is used to evaluate performance, where :latex{latex="\(y_i\)"} is the true value, and :latex{latex="\(\hat{y}_i \)"} is the prediction :
::

::latex{latex="\[J(\theta) = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2\]"}
::

**Backpropagation**: Through differentiation, the gradient of the loss function is computed for each weight using the **chain rule** :

::latex
---
latex: \[ \frac{\partial J}{\partial w_{ij}} = \frac{\partial J}{\partial
  \hat{y}} \cdot \frac{\partial \hat{y}}{\partial z_j} \cdot \frac{\partial
  z_j}{\partial w_{ij}} \]
---
::

These gradients are then used to update the weights using gradient descent:

::latex{latex="\[w = w - \eta \cdot \nabla J(w)\]"}
::

::inline
where

  :::latex{latex="\(\eta\)"}
  :::

is the learning rate.
::

### 4. Common Problems and Solutions

**Overfitting**: This occurs when the model learns the training data too well, at the cost of generalization. Techniques such as **L2 regularization** (Ridge) or **Dropout** are often used to mitigate this.

L2 Regularization:

::latex
---
latex: \[ J_{\text{reg}}(\theta) = J(\theta) + \lambda \sum_{j=1}^{n} \theta_j^2 \]
---
::

::inline
where

  :::latex{latex="\(\lambda\)"}
  :::

is a hyperparameter controlling the regularization penalty.
::

**Vanishing Gradients**: In deep networks, gradients can become extremely small, slowing down training. Activation functions like ReLU or techniques like **batch normalization** can mitigate this issue.

### 5. Advanced Network Architectures

- **Convolutional Neural Networks (CNNs)**: Particularly useful for image processing, CNNs use convolutions to extract local features before classification.

The convolution operation is defined as:

::latex
---
latex: \[ (f * g)(t) = \int_{-\infty}^{\infty} f(\tau)g(t - \tau) d\tau \]
---
::

In CNNs, this operation is discretized to work with digital images.

- **Recurrent Neural Networks (RNNs)**: These networks are well-suited for sequential data like time series or text, as they have recurrent connections that allow them to "remember" past states.

A simple RNN, the state update is given by:

::latex{latex="\[h_t = \sigma(W_h h_{t-1} + W_x x_t + b)\]"}
::

::inline
where

  :::latex{latex="\(h_t\)"}
  :::

is the hidden state at time

  :::latex{latex="\(t\)"}
  :::

, and

  :::latex{latex="\(x_t\)"}
  :::

is the input at time

  :::latex{latex="\(t\)"}
  :::

.
::

## Conclusion

Neural networks are powerful tools for solving complex problems, but they require a solid understanding of their internal workings to tune them effectively. Challenges like overfitting or vanishing gradients must be addressed during design and training.
