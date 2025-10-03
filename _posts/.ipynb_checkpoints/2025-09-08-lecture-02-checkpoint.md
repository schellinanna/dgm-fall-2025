---
layout: distill
title: Lecture 02
description: A Brief History of Deep Learning
date: 2025-09-08

lecturers:
  - name: Ben Lengerich
    url: "https://adaptinfer.org/"
    
authors:
    - name: Aster Bi
    - url: "#"
    - name: Mridula Geddam
    - url: "#"

editors:
  - name: Ben Lengerich # editor's full name
    url: "https://adaptinfer.org" # optional URL to the editor's homepage

abstract: >
    This lecture gives a brief history of DL, from Perceptron to GPT-4.

---


## Reminders
- If you intend to scribe for a class lecture, only sign up once.
- All the recorded lectures are in Kaltura.
- **HW1** is posted on the course website. It is due on **September 19** via Canvas.
- Assignment: Implement a perceptron in Python (basic one-layer neural network).

---

## Recap
Formally, a computer program is said to learn from experience $ℇ$ with respect to some task $𝒯$ and performance measure $𝒫$ if its performance at $𝒯$ as measured by $𝒫$ improves with $ℇ$.

<figure>
  <img src="{{ '../../assets/img/notes/lecture-02/Recap.png' }}" alt="" />
</figure>

---

## Structured data
- Stacking feature vectors will result in a feature matrix.
- $n$ indexes = number of samples.
- $m$ indexes = number of features.
## Unstructured data
- Images (full of pixels that are not labeled).
- **Convolutional Neural Networks**:
  - 1st entry = number of samples.
  - 2nd entry = number of channels (1 = grayscale, 3 = color).
  - 3rd and 4th = height and width.
  - NCHW representation.

---

## Machine Learning Jargon
- **Training a model** = fitting a model = parameterizing a model = learning from data.
- **Training example** = record, instance, sample.
- **Feature** = observation, predictor, variable, independent variable, input, attribute, covariate.
- **Target** = outcome, ground truth, output, response variable, dependent variable, label.
- **Output/Prediction** = model’s estimate of the target.

---

## History of Machine Learning

<figure>
  <img src="{{ '../../assets/img/notes/lecture-02/lec2-1.png' }}" alt="" />
</figure>

- **Perceptrons**: Simple, one-layer network (early optimism).
- **KNN**: With unlimited data, ML systems approach k-nearest neighbors. Key = lookup of most similar sample.
- **Decision tree**: Deterministic, interpretable models.
- **Neural networks**: Comeback after skepticism; concern was too many parameters vs. samples.
- **AI Winter (1990s–2000s)**: Neural nets didn’t perform well, but research continued.
- **Deep neural networks**: Hardware + large datasets allowed training of large models → accuracy improved.
- **Overparameterized models**: Inductive biases guide solutions even without classical guarantees.

<figure>
  <img src="{{ '../../assets/img/notes/lecture-02/lec2-2.png' }}" alt="" />
</figure>


---

## Artificial Neurons and Perceptrons
- Neural model first discussed in 1943 (McCulloch & Pitts).
    <figure>
        <img src="{{ '../../assets/img/notes/lecture-02/lec2-2.png' }}" alt="" />
    </figure>
  - Mimicked neuroscience behaviors.
  - Inputs with weights (+1, –1) + threshold.
  - Excitatory vs inhibitory signals.
- **Perceptrons**:
    <figure>
        <img src="{{ '../../assets/img/notes/lecture-02/lec2-3.png' }}" alt="" />
    </figure>
  - Horizontal = input, vertical = output.
  - Apply activation functions (tanh, sigmoid).
  - Consider regression problem $f: X \rightarrow Y$ for scalar $Y$
  - Let $Y \sim \mathcal{N}(f(x), \Sigma^2)$
  - Then $\arg\max_{w} \log \prod_i P(y_i \mid x_i; w) = \arg\min_{w} \sum_i \tfrac{1}{2}(y_i - f(x_i; w))^2$
    - We can find $\arg\max_{w} \log \prod_i P(y_i \mid x_i; w)$ by iterating:
      $$
      w_d = w_d + \eta \sum_i (y_i - o_i) \, o_i (1 - o_i) \, x^i_d
      $$

- Residual = $(y_i - o_i)$.
    - Function maximized when $o=0.5$: $o_i(1-o_i)$.
    - Residual + uncertainty $\to$ larger update with respect to $x$.

- **Can a Perceptron represent XOR?**
- Answer: No.
- If there were, then there would be constants $w_1$ and $w_2$ such that:
  - When \(x_1 = x_2\), then \(\sigma(w_1 x_1 + w_2 x_2) < \theta\)
  - When \(x_1 \neq x_2\), then \(\sigma(w_1 x_1 + w_2 x_2) \geq \theta\)
- Case 1: $x_1 = 1, x_2 = 0$
  - Eq. (1): $\sigma(w_1) \geq \theta$
- Case 2: $x_1 = 0, x_2 = 1$
  - Eq. (2): $\sigma(w_2) \geq \theta$
- Case 3: $x_1 = 1, x_2 = 1$
  - Eq. (3): $\sigma(w_1 + w_2) < \theta$
- Contradiction: Eq. (1) + Eq. (2) contradicts Eq. (3).


---

## Backpropagation

## Neural networks as computation graphs

- Neural networks are function compositions that can be represented as computation graphs:

<figure>
  <img src="{{ '../../assets/img/notes/lecture-02/fig-computation-graph.png' }}" alt="Computation graph showing input, intermediate computations, and outputs" />
</figure>

- By applying the chain rule, and working in reverse order, we get:

$$
\frac{\partial f_n}{\partial x}
= \sum_{i_1 \in \pi(n)} \frac{\partial f_n}{\partial f_{i_1}} \frac{\partial f_{i_1}}{\partial x}
= \sum_{i_1 \in \pi(n)} \frac{\partial f_n}{\partial f_{i_1}}
   \sum_{i_2 \in \pi(i_1)} \frac{\partial f_{i_1}}{\partial f_{i_2}} \frac{\partial f_{i_2}}{\partial x}
= \dots
$$

- Invented by several groups.
- Formalized by Rumelhart, Hinton, and Williams (1986).
- Sparked a new AI wave.
---

## About the term “Deep Learning”
> “Representation learning is a set of methods that allows a machine to be fed with raw data and to automatically discover the representations needed for detection or classification. Deep learning methods are representation-learning methods with multiple levels of representation [...]”
— LeCun, Bengio, & Hinton (2015)

---

## Activation Functions

<figure>
  <img src="{{ '../../assets/img/notes/lecture-02/lec2-7.png' }}" alt="" />
</figure>


- If all layers are linear → the model is still linear.
- **ReLU (Rectified Linear Unit):** most common.
- **Sigmoid:** bounded between 0 and 1, interpretable as probability.

---

## Hardware

<figure>
  <img src="{{ '../../assets/img/notes/lecture-02/lec2-8.png' }}" alt="" />
</figure>

- **CPU:**
  - Cache is local memory.
  - Control is centralized, instructions pass through the core.
- **GPU:**
  - Optimized for matrix multiplication.
  - Cores share cache + control.
  - Fast parallel computation, optimized for one path.
  - Data transfer at high speed.

---

## Large-Scale Unsupervised Learning
**From GPT-1 (2018) to GPT-4:**
- **Architecture / Scale:**
  - Parameters: 1.5B → >1T
  - Layers: 12 → >96
  - Attention heads: 12 → >96
  - Embedding dimension: 768 → >12,288
  - Context length: 512 → 128k
  - Vocabulary: 40k → >50k tokens
- **Tokenizer:** Multimodal (includes image patches).
- **Mixture-of-Experts** architecture.
- **Training data:** 5GB (BookCorpus) → ~50TB (13T tokens).
- **Reinforcement Learning for alignment.**

---

## Open Directions
- Verifiable rewards (code/math correctness).
- Vision-language multimodal models.
- Large-scale reinforcement learning.
- Model uncertainty and hallucinations.
- Model editing and interpretability.
- Model synthesis and communication.
