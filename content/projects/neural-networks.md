---
title: Introduction to Neural Networks
description: Mathematics Behind Neural Networks
authors:
  - name: Pierre Graef
    to: https://twitter.com/benjamincanac
    avatar:
      src: https://i.pravatar.cc/128?u=2
date: 2024-09-25T00:00:00.000Z
badge:
  label: Machine Learning, Mathematics, Data Science
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

- **Sigmoid**:
  :latex{latex="\[\sigma(x) = \frac{1}{1 + e^{-x}} \]"}
  
  It constrains outputs between 0 and 1, but suffers from saturation at extreme values.

- **ReLU (Rectified Linear Unit)**:
  ::latex{latex="\[f(x) = \max(0, x)\]"}
  ::

  It's simple to compute and helps avoid gradient saturation issues, but it may cause "dead" neurons.

- **Tanh (Hyperbolic Tangent)**:
  ::latex{latex="\[\tanh(x) = \frac{e^{x} - e^{-x}}{e^{x} + e^{-x}}\]"}
  ::

  Values range between -1 and 1, but like sigmoid, it can also saturate.

### 3. Learning Algorithm: Backpropagation

Training a neural network involves using the backpropagation algorithm. This process relies on computing the gradient of the errors with respect to the network's weights through the **gradient descent** algorithm. Here are the main steps:

1. **Forward pass**: The data passes through the network layer by layer, and the output is calculated.

2. **Error calculation**: A loss function 
::latex{latex="\[J(\theta)\]"} 
::
 , such as Mean Squared Error (MSE) or Cross-Entropy, is used to evaluate performance:

   ::latex{latex="\[J(\theta) = \frac{1}{n} \sum\_^{n} (y\_i - \hat{y}\_i)^2\]"}
   ::

   where (y\_i) is the true value, and (\hat{y}\_i) is the prediction.

3. **Backpropagation**: Through differentiation, the gradient of the loss function is computed for each weight using the **chain rule**:

   ::latex{latex="\[\frac{\partial J}{\partial w\_} = \frac{\partial J}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial z\_j} \cdot \frac{\partial z\_j}{\partial w\_}\]"}
   ::

   These gradients are then used to update the weights using gradient descent:
   ::latex{latex="\[w = w - \eta \cdot \nabla J(w)\]"}
   ::

   where 
   ::latex{latex="\[\eta\]"} 
   :: 
   is the learning rate.


### 4. Common Problems and Solutions

- **Overfitting**: This occurs when the model learns the training data too well, at the cost of generalization. Techniques such as **L2 regularization** (Ridge) or **Dropout** are often used to mitigate this.

 **L2 Regularization**:
  ::latex{latex="\[J\_{\text{reg}}(\theta) = J(\theta) + \lambda \sum\_^{n} \theta\_j^2\]"}
  ::
  where 
  ::latex{latex="\[\lambda\]"}
  :: 
  is a hyperparameter controlling the regularization penalty.

- **Vanishing Gradients**: In deep networks, gradients can become extremely small, slowing down training. Activation functions like ReLU or techniques like **batch normalization** can mitigate this issue.

### 5. Advanced Network Architectures

- **Convolutional Neural Networks (CNNs)**: Particularly useful for image processing, CNNs use convolutions to extract local features before classification.

The convolution operation is defined as:
  ::latex{latex="\[(f * g)(t) = \int_{-\infty}^{\infty} f(\tau)g(t - \tau) d\tau\]"}
  ::

  In CNNs, this operation is discretized to work with digital images.
- **Recurrent Neural Networks (RNNs)**: These networks are well-suited for sequential data like time series or text, as they have recurrent connections that allow them to "remember" past states.

A simple RNN, the state update is given by:
  ::latex{latex="\[h_t = \sigma(W_h h_{t-1} + W_x x_t + b)\]"}
  ::
  where 
  ::latex{latex="\[h_t\]"}
  ::
  is the hidden state at time (t), and 
  ::latex{latex="\[x_t\]"}
  ::
  is the input at time (t).

## Conclusion

Neural networks are powerful tools for solving complex problems, but they require a solid understanding of their internal workings to tune them effectively. Challenges like overfitting or vanishing gradients must be addressed during design and training.
