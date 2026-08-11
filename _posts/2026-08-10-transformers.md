---
layout: post
title: "Transformers"
subtitle: "The attention-based architecture behind modern language models"
date: 2026-08-10 10:45:13 -0400
tags:	deeplearning nlp machinelearning transformers python
---

# Transformers

The Transformer is a neural network architecture introduced in the 2017 paper *"Attention Is All You Need"* by Vaswani et al. It replaced recurrence and convolution with a mechanism called **self-attention**, allowing models to process entire sequences in parallel while still capturing long-range dependencies. Transformers are the foundation behind models like BERT, GPT, and most modern large language models.

At a high level, a Transformer is built from a handful of core components:
1. Input embeddings
2. Positional encoding
3. Multi-head self-attention
4. Feed-forward networks
5. Layer normalization and residual connections

Here is a brief about each of the core components:

**Input Embeddings**: Raw tokens (words, subwords, or characters) are mapped into dense vectors of fixed size. These embeddings give the model a numerical representation of each token that can be learned during training.

**Positional Encoding**: Since the Transformer has no inherent sense of order (unlike RNNs), positional information is injected into the embeddings. The original paper used fixed sinusoidal functions, though many modern variants use learned positional embeddings instead.

**Multi-Head Self-Attention**: Self-attention lets every token in a sequence "look at" every other token and weigh how relevant they are to each other. Multiple attention heads run in parallel, each learning to focus on different types of relationships (e.g., syntactic vs. semantic), and their outputs are concatenated and projected back to the model dimension.

**Feed-Forward Networks**: After attention, each position is passed through a simple fully connected network (typically two linear layers with a non-linearity in between). This adds additional representational capacity independently at each position.

**Layer Normalization and Residual Connections**: Residual (skip) connections around both the attention and feed-forward sub-layers, combined with layer normalization, stabilize training and allow much deeper networks to be trained effectively.

Here is a simplified example of scaled dot-product attention using numpy.

```python
import numpy as np

def softmax(x, axis=-1):
    x = x - np.max(x, axis=axis, keepdims=True)
    e_x = np.exp(x)
    return e_x / np.sum(e_x, axis=axis, keepdims=True)

def scaled_dot_product_attention(Q, K, V, mask=None):
    d_k = Q.shape[-1]

    # Compute attention scores
    scores = np.matmul(Q, K.transpose(-1, -2)) / np.sqrt(d_k)

    if mask is not None:
        scores = np.where(mask == 0, -1e9, scores)

    # Convert scores to probabilities
    weights = softmax(scores, axis=-1)

    # Weighted sum of values
    output = np.matmul(weights, V)
    return output, weights

# Toy example: sequence of 4 tokens, embedding dim of 8
np.random.seed(0)
seq_len, d_model = 4, 8

Q = np.random.randn(seq_len, d_model)
K = np.random.randn(seq_len, d_model)
V = np.random.randn(seq_len, d_model)

output, weights = scaled_dot_product_attention(Q, K, V)

print("Attention weights:\n", np.round(weights, 2))
print("\nOutput:\n", np.round(output, 2))
```

The attention weights matrix shows, for each token (row), how much it attends to every other token (column) in the sequence — the core computation repeated across heads and layers in a full Transformer.

## Why Transformers matter
Transformers reshaped deep learning for several reasons:
- Parallelization: Unlike RNNs, all tokens in a sequence are processed simultaneously, making training on large datasets far more efficient on GPUs/TPUs.
- Long-range dependencies: Self-attention connects any two tokens directly, regardless of their distance in the sequence, avoiding the vanishing gradient issues of recurrent models.
- Transfer learning: Pretrained Transformers (BERT, GPT, T5, etc.) can be fine-tuned on downstream tasks with relatively little task-specific data.
- Scalability: The architecture scales predictably with more data, parameters, and compute, which underpins the rise of large language models.
- Versatility: Beyond NLP, Transformers now power vision (ViT), audio, and multi-modal models.

### References:
[Attention Is All You Need (Vaswani et al., 2017)](https://arxiv.org/abs/1706.03762)
[The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
