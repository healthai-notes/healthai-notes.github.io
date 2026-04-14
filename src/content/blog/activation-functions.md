---
title: Sigmoid, ReLU, GELU, And Other Activation Terms
description: An introduction to sigmoid, ReLU, GELU, tanh, softmax, and the surrounding terminology used in neural networks.
pubDate: 2026-04-14
updatedDate: 2026-04-14
heroTag: ML Basics
draft: false
---

import ActivationCurves from '../../components/ActivationCurves.astro';

If you are new to neural networks, activation-function vocabulary can sound more complicated than it is. Terms like `sigmoid`, `ReLU`, `GELU`, and `softmax` are mostly names for different ways a model transforms a number before passing it deeper into the network.

The short version is this: a linear stack of layers is still just a linear function. To make neural networks learn more interesting patterns, each layer needs a **nonlinearity**. That nonlinearity is usually introduced by an **activation function**.

## The core idea: what an activation function does

A neuron or hidden unit first computes a weighted sum such as `w1x1 + w2x2 + b`. The activation function then transforms that intermediate value. Different activations change how strongly the neuron responds, how gradients flow during training, and how easy the network is to optimize.

| Term | Main idea | Where it commonly shows up |
| --- | --- | --- |
| `sigmoid` | Squeezes values into the range 0 to 1 | Binary output probabilities, older hidden layers |
| `tanh` | Squeezes values into the range -1 to 1 | Older recurrent nets, some hidden layers |
| `ReLU` | Outputs 0 for negatives and stays linear for positives | Default hidden-layer choice in many networks |
| `GELU` | Smoothly gates values instead of hard-cutting them | Transformers and many modern large models |
| `softmax` | Turns a vector into a probability distribution | Multi-class output layers |

<ActivationCurves />

## Sigmoid

The sigmoid function maps any real number to a value between 0 and 1. That makes it intuitive when you want to interpret a model output as a probability, especially for binary classification.

- Useful when the output means "how likely is class 1?"
- Easy to interpret because the range is bounded
- Less common in deep hidden layers because gradients can get very small

That last issue is part of the **vanishing gradient** problem. When the function saturates near 0 or 1, its slope becomes tiny, so backpropagation carries only a weak training signal.

## Tanh

`tanh` is similar in shape to sigmoid but centered around zero, with outputs in the range -1 to 1. Zero-centered activations can make optimization easier than sigmoid in some settings, which is why tanh historically replaced sigmoid in many hidden layers.

Even so, tanh can still suffer from vanishing gradients when values land in the flat parts of the curve.

## ReLU

ReLU stands for **Rectified Linear Unit**. It is conceptually simple: negative values become 0, positive values pass through unchanged.

- Cheap to compute
- Helps optimization in deep networks compared with sigmoid or tanh
- Introduces sparsity because many activations become exactly 0

The main drawback is the **dying ReLU** problem. If a neuron falls into a regime where it outputs 0 for almost everything, it may stop learning effectively because its gradient is also 0 on the negative side.

## Leaky ReLU and related variants

To soften ReLU's hard zeroing behavior, people use variants such as `Leaky ReLU`, `PReLU`, and `ELU`.

- `Leaky ReLU` keeps a small negative slope instead of returning exact zeros
- `PReLU` learns that negative slope during training
- `ELU` and similar functions smooth the negative region further

These variants exist mostly to preserve the practical benefits of ReLU while reducing the risk of inactive neurons.

## GELU

GELU stands for **Gaussian Error Linear Unit**. Instead of sharply clipping all negative values like ReLU, GELU scales inputs more smoothly. In practice, you can think of it as letting values through in a graded way rather than making a hard on/off decision.

This smoother behavior is one reason GELU became common in transformer architectures and large language models. You do not usually choose GELU because it is easier to explain; you choose it because empirically it works well in modern deep architectures.

## Softmax is related, but slightly different

People often mention `softmax` in the same breath, but it usually plays a different role. Sigmoid, ReLU, tanh, and GELU are commonly used inside the network. Softmax is typically used at the output layer for multi-class classification.

If a model must choose among several classes, softmax converts raw scores into numbers that sum to 1. That is why it is commonly paired with cross-entropy loss.

## Other relevant terminology

- **Activation**: the output of a neuron after applying the activation function
- **Logit**: a raw model score before sigmoid or softmax
- **Nonlinearity**: any transformation that prevents the network from collapsing into one linear map
- **Backpropagation**: the algorithm used to send gradient information backward through the network
- **Gradient**: how much a parameter should change to reduce loss
- **Saturation**: a region where changing the input barely changes the output
- **Vanishing gradient**: gradients become too small to train early layers well

## Which one should you remember first?

If you only want a practical mental model, remember this:

- Use `sigmoid` to think about binary probabilities
- Use `softmax` to think about multi-class probabilities
- Use `ReLU` as the classic default hidden-layer activation
- Use `GELU` as the smoother activation you will often see in transformers

## Bottom line

These terms are not four unrelated inventions. They are different answers to the same question: *how should a network transform intermediate values so it can learn useful, nonlinear behavior and still train well?*

Once that framing is clear, the vocabulary becomes much less intimidating. The names are just labels for tradeoffs between interpretability, smoothness, stability, and training dynamics.
