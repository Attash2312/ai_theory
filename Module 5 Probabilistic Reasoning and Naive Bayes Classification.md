# Module 5: Probabilistic Reasoning and Naive Bayes Classification

## Table of Contents
1. [Probability Fundamentals](#probability-fundamentals)
2. [Naive Bayes Classifier](#naive-bayes-classifier)
3. [Artificial Neural Networks (ANN) - Optional](#artificial-neural-networks-ann---optional)
4. [Solved Exam Questions](#solved-exam-questions)

---

# Probability Fundamentals

## Core Terms

| Term | Meaning |
|------|---------|
| Experiment | Random process (e.g., draw a card) |
| Sample Space ($\Omega$) | Set of all possible outcomes |
| Event ($A$) | Subset of $\Omega$ |

## Basic Probability Rules

- $0 \le P(A) \le 1$
- $P(\Omega) = 1$
- Complement: $P(\neg A) = 1 - P(A)$
- Addition rule: $P(A \cup B) = P(A) + P(B) - P(A \cap B)$

## Conditional Probability

The probability of $A$ given that $B$ occurred:

$$P(A|B) = \frac{P(A \cap B)}{P(B)} \quad (P(B) > 0)$$

## Prior, Posterior, Likelihood

| Quantity | Meaning |
|----------|---------|
| Prior $P(H)$ | Belief in hypothesis/class before seeing data |
| Likelihood $P(E|H)$ | Probability of evidence given hypothesis |
| Posterior $P(H|E)$ | Updated belief after seeing evidence |

## Bayes’ Theorem

$$P(H|E) = \frac{P(E|H)\,P(H)}{P(E)}$$

Where $P(E)$ is the evidence/normalizing term.

# Naive Bayes Classifier

## Introduction

```
NAIVE BAYES CLASSIFIER:
───────────────────────
• Probabilistic classifier based on Bayes' Theorem
• "Naive" because it assumes features are INDEPENDENT
• Simple, fast, and surprisingly effective
• Works well for text classification, spam detection, etc.
```

## Bayes' Theorem

```
                    P(B|A) × P(A)
        P(A|B) = ─────────────────
                       P(B)

Where:
• P(A|B) = Posterior probability (what we want)
• P(A)   = Prior probability of A
• P(B|A) = Likelihood
• P(B)   = Evidence (normalizing constant)
```

## Naive Bayes Formula for Classification

```
For classification with features X₁, X₂, ..., Xₙ:

P(Class|X₁, X₂, ..., Xₙ) ∝ P(Class) × P(X₁|Class) × P(X₂|Class) × ... × P(Xₙ|Class)

                         ∝ P(Class) × ∏ P(Xᵢ|Class)
                                      i=1

Choose class with MAXIMUM posterior probability.
```

## Step-by-Step Process

```
┌─────────────────────────────────────────────────────────────┐
│            NAIVE BAYES CLASSIFICATION STEPS                  │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. CALCULATE PRIOR PROBABILITIES                            │
│     P(Class) = Count(Class) / Total samples                 │
│                                                              │
│  2. CALCULATE LIKELIHOODS                                    │
│     P(Feature=value | Class) = Count(value in Class)        │
│                                 ─────────────────────        │
│                                    Count(Class)              │
│                                                              │
│  3. APPLY BAYES' FORMULA                                     │
│     For each class, compute:                                 │
│     P(Class) × ∏P(Featureᵢ|Class)                           │
│                                                              │
│  4. NORMALIZE (optional) AND CHOOSE                          │
│     Select class with highest probability                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Naive Bayes Solved Example 1: Play Tennis

### Problem

Given the following training data, predict whether to play tennis when:
- Outlook = Sunny
- Temperature = Cool
- Humidity = High
- Wind = Strong

```
┌────┬──────────┬─────────────┬──────────┬────────┬──────────┐
│ No │ Outlook  │ Temperature │ Humidity │  Wind  │ Play?    │
├────┼──────────┼─────────────┼──────────┼────────┼──────────┤
│  1 │ Sunny    │ Hot         │ High     │ Weak   │ No       │
│  2 │ Sunny    │ Hot         │ High     │ Strong │ No       │
│  3 │ Overcast │ Hot         │ High     │ Weak   │ Yes      │
│  4 │ Rain     │ Mild        │ High     │ Weak   │ Yes      │
│  5 │ Rain     │ Cool        │ Normal   │ Weak   │ Yes      │
│  6 │ Rain     │ Cool        │ Normal   │ Strong │ No       │
│  7 │ Overcast │ Cool        │ Normal   │ Strong │ Yes      │
│  8 │ Sunny    │ Mild        │ High     │ Weak   │ No       │
│  9 │ Sunny    │ Cool        │ Normal   │ Weak   │ Yes      │
│ 10 │ Rain     │ Mild        │ Normal   │ Weak   │ Yes      │
│ 11 │ Sunny    │ Mild        │ Normal   │ Strong │ Yes      │
│ 12 │ Overcast │ Mild        │ High     │ Strong │ Yes      │
│ 13 │ Overcast │ Hot         │ Normal   │ Weak   │ Yes      │
│ 14 │ Rain     │ Mild        │ High     │ Strong │ No       │
└────┴──────────┴─────────────┴──────────┴────────┴──────────┘
```

### Solution

```
STEP 1: CALCULATE PRIOR PROBABILITIES
──────────────────────────────────────

Total samples = 14
Play = Yes: 9 samples
Play = No: 5 samples

P(Yes) = 9/14 = 0.643
P(No)  = 5/14 = 0.357


STEP 2: CALCULATE LIKELIHOODS
─────────────────────────────

For Play = Yes (9 samples):
┌─────────────────────────────────────────────────────────────┐
│ P(Outlook=Sunny | Yes) = 2/9 = 0.222                        │
│ P(Temperature=Cool | Yes) = 3/9 = 0.333                     │
│ P(Humidity=High | Yes) = 3/9 = 0.333                        │
│ P(Wind=Strong | Yes) = 3/9 = 0.333                          │
└─────────────────────────────────────────────────────────────┘

For Play = No (5 samples):
┌─────────────────────────────────────────────────────────────┐
│ P(Outlook=Sunny | No) = 3/5 = 0.600                         │
│ P(Temperature=Cool | No) = 1/5 = 0.200                      │
│ P(Humidity=High | No) = 4/5 = 0.800                         │
│ P(Wind=Strong | No) = 3/5 = 0.600                           │
└─────────────────────────────────────────────────────────────┘


STEP 3: APPLY BAYES' FORMULA
────────────────────────────

Query: Outlook=Sunny, Temperature=Cool, Humidity=High, Wind=Strong

P(Yes | Query) ∝ P(Yes) × P(Sunny|Yes) × P(Cool|Yes) × P(High|Yes) × P(Strong|Yes)
               = 0.643 × 0.222 × 0.333 × 0.333 × 0.333
               = 0.643 × 0.222 × 0.333 × 0.333 × 0.333
               = 0.00529

P(No | Query) ∝ P(No) × P(Sunny|No) × P(Cool|No) × P(High|No) × P(Strong|No)
              = 0.357 × 0.600 × 0.200 × 0.800 × 0.600
              = 0.02057


STEP 4: COMPARE AND CLASSIFY
────────────────────────────

P(Yes | Query) = 0.00529
P(No | Query)  = 0.02057

Since P(No | Query) > P(Yes | Query):

┌─────────────────────────────────────────┐
│ PREDICTION: NO (Don't play tennis)      │
│                                         │
│ Confidence:                             │
│ P(No) = 0.02057 / (0.02057 + 0.00529)  │
│       = 0.02057 / 0.02586               │
│       = 0.795 = 79.5%                   │
└─────────────────────────────────────────┘
```

---

## Naive Bayes Solved Example 2: Spam Classification

### Problem

Classify email as Spam or Ham using word frequencies:

```
Training Data:
┌─────────┬───────┬──────┬───────┬───────┬────────┐
│ Email   │ "free"│"win" │"money"│"hello"│ Class  │
├─────────┼───────┼──────┼───────┼───────┼────────┤
│ Email 1 │  Yes  │ Yes  │  Yes  │  No   │ Spam   │
│ Email 2 │  Yes  │ No   │  Yes  │  No   │ Spam   │
│ Email 3 │  No   │ Yes  │  No   │  Yes  │ Ham    │
│ Email 4 │  No   │ No   │  No   │  Yes  │ Ham    │
│ Email 5 │  Yes  │ Yes  │  No   │  No   │ Spam   │
│ Email 6 │  No   │ No   │  Yes  │  Yes  │ Ham    │
└─────────┴───────┴──────┴───────┴───────┴────────┘

Classify: Email with "free"=Yes, "win"=No, "money"=Yes, "hello"=No
```

### Solution

```
STEP 1: PRIOR PROBABILITIES
───────────────────────────
P(Spam) = 3/6 = 0.5
P(Ham)  = 3/6 = 0.5


STEP 2: LIKELIHOODS
───────────────────

For Spam (3 samples):
P(free=Yes | Spam)  = 3/3 = 1.0
P(win=No | Spam)    = 1/3 = 0.333
P(money=Yes | Spam) = 2/3 = 0.667
P(hello=No | Spam)  = 3/3 = 1.0

For Ham (3 samples):
P(free=Yes | Ham)   = 0/3 = 0.0  ⚠️ Problem!
P(win=No | Ham)     = 2/3 = 0.667
P(money=Yes | Ham)  = 1/3 = 0.333
P(hello=No | Ham)   = 0/3 = 0.0  ⚠️ Problem!


STEP 3: LAPLACE SMOOTHING (to handle zero probabilities)
────────────────────────────────────────────────────────

Add 1 to each count, add k (number of values) to denominator:

For Spam (3 + 2 = 5 in denominator):
P(free=Yes | Spam)  = (3+1)/(3+2) = 4/5 = 0.8
P(win=No | Spam)    = (1+1)/(3+2) = 2/5 = 0.4
P(money=Yes | Spam) = (2+1)/(3+2) = 3/5 = 0.6
P(hello=No | Spam)  = (3+1)/(3+2) = 4/5 = 0.8

For Ham (3 + 2 = 5 in denominator):
P(free=Yes | Ham)   = (0+1)/(3+2) = 1/5 = 0.2
P(win=No | Ham)     = (2+1)/(3+2) = 3/5 = 0.6
P(money=Yes | Ham)  = (1+1)/(3+2) = 2/5 = 0.4
P(hello=No | Ham)   = (0+1)/(3+2) = 1/5 = 0.2


STEP 4: CALCULATE POSTERIORS
────────────────────────────

P(Spam | Query) ∝ 0.5 × 0.8 × 0.4 × 0.6 × 0.8 = 0.0768
P(Ham | Query)  ∝ 0.5 × 0.2 × 0.6 × 0.4 × 0.2 = 0.0048


STEP 5: CLASSIFY
────────────────

┌─────────────────────────────────────────┐
│ PREDICTION: SPAM                        │
│                                         │
│ P(Spam) = 0.0768 / (0.0768 + 0.0048)   │
│         = 0.941 = 94.1%                 │
└─────────────────────────────────────────┘
```

---

## Mock Exam Pattern (Q5 Style): Keyword Table → Naive Bayes Classification

### Template

1) Choose a vocabulary of keywords (features) $w_1,\dots,w_n$.
2) Convert each document into a **binary vector** (present/absent) or counts.
3) Compute priors $P(C)$ and likelihoods $P(w_i\mid C)$ (use **Laplace smoothing** if needed).
4) Compute scores:
  $$P(C\mid x) \propto P(C)\prod_i P(x_i\mid C)$$
5) Pick the class with larger score (or normalized posterior).

### Mini Worked Example (Binary Features)

**Classes:** Positive (Pos) vs Negative (Neg)

**Keywords:** {good, bad, boring}

Training summary (already counted):

| Keyword present? | Count in Pos docs | Count in Neg docs |
|------------------|-------------------|-------------------|
| good             | 4                 | 1                 |
| bad              | 1                 | 4                 |
| boring           | 1                 | 3                 |

Assume $N_{Pos}=5$ docs, $N_{Neg}=5$ docs, so:
- $P(Pos)=P(Neg)=0.5$

Use Laplace smoothing for binary features (present/absent):
$$P(good=Yes\mid Pos)=\frac{4+1}{5+2}=\frac{5}{7},\quad P(good=Yes\mid Neg)=\frac{1+1}{5+2}=\frac{2}{7}$$

**Classify:** Review contains {good=Yes, bad=No, boring=No}

Compute two unnormalized scores:

$$Score(Pos)=P(Pos)\cdot P(good=Yes\mid Pos)\cdot P(bad=No\mid Pos)\cdot P(boring=No\mid Pos)$$
$$Score(Neg)=P(Neg)\cdot P(good=Yes\mid Neg)\cdot P(bad=No\mid Neg)\cdot P(boring=No\mid Neg)$$

Then compare the two scores. The class with higher score is the prediction.

Note: If the exam does not ask for “No” probabilities, you can compare using only the “Yes” likelihoods as an approximation (but full method is more correct).

## Naive Bayes Solved Example 3: Medical Diagnosis

### Problem

```
Predict if a patient has Disease D based on symptoms:

┌─────────┬─────────┬───────┬──────────┬─────────┐
│ Patient │ Fever   │ Cough │ Fatigue  │ Disease │
├─────────┼─────────┼───────┼──────────┼─────────┤
│    1    │  Yes    │  Yes  │   Yes    │   Yes   │
│    2    │  Yes    │  Yes  │   No     │   Yes   │
│    3    │  Yes    │  No   │   Yes    │   Yes   │
│    4    │  No     │  Yes  │   Yes    │   No    │
│    5    │  No     │  No   │   No     │   No    │
│    6    │  No     │  Yes  │   No     │   No    │
│    7    │  Yes    │  No   │   No     │   Yes   │
│    8    │  No     │  No   │   Yes    │   No    │
└─────────┴─────────┴───────┴──────────┴─────────┘

Query: Fever=Yes, Cough=Yes, Fatigue=Yes
```

### Solution

```
STEP 1: PRIOR PROBABILITIES
───────────────────────────

Total = 8 patients
Disease = Yes: 4 patients
Disease = No: 4 patients

P(Disease=Yes) = 4/8 = 0.5
P(Disease=No)  = 4/8 = 0.5


STEP 2: COUNT OCCURRENCES
─────────────────────────

For Disease = Yes (4 patients: 1, 2, 3, 7):
┌────────────────────────────────────────┐
│ Fever=Yes:   4 out of 4 → 4/4 = 1.0   │
│ Cough=Yes:   2 out of 4 → 2/4 = 0.5   │
│ Fatigue=Yes: 2 out of 4 → 2/4 = 0.5   │
└────────────────────────────────────────┘

For Disease = No (4 patients: 4, 5, 6, 8):
┌────────────────────────────────────────┐
│ Fever=Yes:   0 out of 4 → 0/4 = 0.0   │
│ Cough=Yes:   2 out of 4 → 2/4 = 0.5   │
│ Fatigue=Yes: 2 out of 4 → 2/4 = 0.5   │
└────────────────────────────────────────┘


STEP 3: APPLY LAPLACE SMOOTHING (for zero probability)
──────────────────────────────────────────────────────

For Disease = No:
P(Fever=Yes | No) = (0+1)/(4+2) = 1/6 = 0.167

Updated likelihoods with Laplace smoothing:

Disease = Yes:
P(Fever=Yes | Yes)   = (4+1)/(4+2) = 5/6 = 0.833
P(Cough=Yes | Yes)   = (2+1)/(4+2) = 3/6 = 0.5
P(Fatigue=Yes | Yes) = (2+1)/(4+2) = 3/6 = 0.5

Disease = No:
P(Fever=Yes | No)   = (0+1)/(4+2) = 1/6 = 0.167
P(Cough=Yes | No)   = (2+1)/(4+2) = 3/6 = 0.5
P(Fatigue=Yes | No) = (2+1)/(4+2) = 3/6 = 0.5


STEP 4: CALCULATE POSTERIORS
────────────────────────────

P(Yes | Query) ∝ P(Yes) × P(Fever=Yes|Yes) × P(Cough=Yes|Yes) × P(Fatigue=Yes|Yes)
               = 0.5 × 0.833 × 0.5 × 0.5
               = 0.104

P(No | Query) ∝ P(No) × P(Fever=Yes|No) × P(Cough=Yes|No) × P(Fatigue=Yes|No)
              = 0.5 × 0.167 × 0.5 × 0.5
              = 0.021


STEP 5: NORMALIZE AND CLASSIFY
──────────────────────────────

P(Yes | Query) = 0.104 / (0.104 + 0.021) = 0.104 / 0.125 = 0.832
P(No | Query)  = 0.021 / (0.104 + 0.021) = 0.021 / 0.125 = 0.168

┌─────────────────────────────────────────┐
│ PREDICTION: DISEASE = YES               │
│ Confidence: 83.2%                       │
│                                         │
│ Interpretation: Patient likely has the  │
│ disease based on all three symptoms.    │
└─────────────────────────────────────────┘
```

---

## Laplace Smoothing Explained

```
┌─────────────────────────────────────────────────────────────┐
│                  LAPLACE SMOOTHING                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Problem: Zero probability makes entire product zero!        │
│                                                              │
│  If P(feature|class) = 0, then:                             │
│  P(class|features) = ... × 0 × ... = 0                      │
│                                                              │
│  Solution: Add-1 Smoothing (Laplace Smoothing)              │
│                                                              │
│                    count(feature, class) + 1                 │
│  P(feature|class) = ────────────────────────────            │
│                      count(class) + k                        │
│                                                              │
│  Where k = number of possible values for that feature        │
│                                                              │
│  Example: Binary features (Yes/No) → k = 2                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Naive Bayes: Advantages and Disadvantages

```
┌─────────────────────────────────────────────────────────────┐
│                     ADVANTAGES                               │
├─────────────────────────────────────────────────────────────┤
│ ✓ Simple and easy to implement                              │
│ ✓ Fast training and prediction                              │
│ ✓ Works well with high-dimensional data                     │
│ ✓ Performs well with small training sets                    │
│ ✓ Handles missing values gracefully                         │
│ ✓ Good for text classification (spam, sentiment)            │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                    DISADVANTAGES                             │
├─────────────────────────────────────────────────────────────┤
│ ✗ Assumes feature independence (rarely true)                │
│ ✗ Zero probability problem (needs smoothing)                │
│ ✗ Not suitable for regression                               │
│ ✗ Probability estimates may be inaccurate                   │
│ ✗ Sensitive to irrelevant features                          │
└─────────────────────────────────────────────────────────────┘
```

---

# Artificial Neural Networks (ANN) - Optional

## Introduction

```
ARTIFICIAL NEURAL NETWORKS:
───────────────────────────
• Computational models inspired by biological neurons
• Consist of interconnected nodes (neurons) in layers
• Learn patterns through training (adjusting weights)
• Can approximate any continuous function
• Used for classification, regression, pattern recognition
```

## Biological vs Artificial Neuron

```
BIOLOGICAL NEURON:                 ARTIFICIAL NEURON:
                                   
    Dendrites                          Inputs (x₁, x₂, ..., xₙ)
        ↓                                      ↓
    Cell Body  ──→  Axon              Weighted Sum → Activation → Output
        ↑                                      ↓
    Synapses                           Weights (w₁, w₂, ..., wₙ)


┌─────────────────────────────────────────────────────────────┐
│                ARTIFICIAL NEURON MODEL                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│    x₁ ──w₁──┐                                               │
│             │                                                │
│    x₂ ──w₂──┼──→ [Σ + b] ──→ [f(·)] ──→ y                  │
│             │     (sum)    (activation)  (output)           │
│    x₃ ──w₃──┘                                               │
│                                                              │
│    y = f(Σ wᵢxᵢ + b) = f(w₁x₁ + w₂x₂ + w₃x₃ + b)          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Components of ANN

```
┌─────────────────────────────────────────────────────────────┐
│                    ANN COMPONENTS                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. INPUT LAYER                                              │
│     • Receives input features                                │
│     • No computation, just passes data                       │
│     • Number of nodes = number of features                   │
│                                                              │
│  2. HIDDEN LAYER(S)                                          │
│     • Performs intermediate computations                     │
│     • Extracts features/patterns                             │
│     • Can have multiple hidden layers (deep learning)        │
│                                                              │
│  3. OUTPUT LAYER                                             │
│     • Produces final prediction                              │
│     • Number of nodes depends on task                        │
│       - Binary classification: 1 node                        │
│       - Multi-class (k classes): k nodes                     │
│                                                              │
│  4. WEIGHTS                                                  │
│     • Connection strengths between neurons                   │
│     • Learned during training                                │
│                                                              │
│  5. BIAS                                                     │
│     • Additional parameter for each neuron                   │
│     • Allows shifting activation function                    │
│                                                              │
│  6. ACTIVATION FUNCTION                                      │
│     • Introduces non-linearity                               │
│     • Determines neuron's output                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Network Architecture

```
       INPUT LAYER      HIDDEN LAYER      OUTPUT LAYER
       
          x₁ ─────────○─────────┐
              ╲      ╱          │
               ╲    ╱           ○────→ y₁
                ╲  ╱           ╱│
          x₂ ────╳────○───────╱ │
                ╱  ╲           ╲│
               ╱    ╲           ○────→ y₂
              ╱      ╲          │
          x₃ ─────────○─────────┘
       
       (3 inputs)   (2 hidden)   (2 outputs)
       
       Notation: This is a 3-2-2 network
```

---

## Training Basics (Hidden Layer, Loss, Optimizer)

### Why Hidden Layers Matter

- A network **without hidden layers** (single layer perceptron) can only learn **linearly separable** patterns.
- Hidden layers + non-linear activations let the network learn **non-linear decision boundaries**.
- Intuition: hidden units learn intermediate features (e.g., edges → shapes → objects).

### Loss Function (What Training Tries to Minimize)

The **loss** measures how wrong the network is.

Common choices:
- **Mean Squared Error (MSE)** (often for regression):
  $$\text{MSE}=\frac{1}{n}\sum_{i=1}^n (y_i-\hat{y}_i)^2$$
- **Binary Cross-Entropy** (common for sigmoid output in binary classification):
  $$L= -\big(y\log(\hat{y})+(1-y)\log(1-\hat{y})\big)$$

### Optimizer (How We Update Weights)

An optimizer updates weights to reduce loss.

- **Gradient Descent / SGD:**
  $$w \leftarrow w - \eta\,\nabla_w L$$
  where $\eta$ is the learning rate.
- **Adam (idea):** adapts the learning rate per parameter using running averages of gradients.

### Training Loop (Exam-Friendly Summary)

1) Forward pass (compute outputs)
2) Compute loss
3) Backpropagation (compute gradients)
4) Optimizer step (update weights)
5) Repeat for epochs

---

## Activation Functions

### 1. Step Function (Threshold)

```
           │
         1 ┤     ┌──────────
           │     │
         0 ┤─────┘
           │
           └─────┼─────────→
                 0
                 
f(x) = { 1  if x ≥ 0
       { 0  if x < 0

Use: Original perceptron, binary classification
```

### 2. Sigmoid Function

```
           │        ────────
         1 ┤      ╱
           │    ╱
        0.5┤   │
           │  ╱
         0 ┤──
           └─────┼─────────→
                 0

f(x) = 1 / (1 + e⁻ˣ)

Range: (0, 1)
Use: Binary classification, output probabilities
Derivative: f'(x) = f(x) × (1 - f(x))
```

### 3. Tanh Function

```
           │        ────────
         1 ┤      ╱
           │    ╱
         0 ┤───┼───
           │  ╱
        -1 ┤──
           └─────┼─────────→
                 0

f(x) = (eˣ - e⁻ˣ) / (eˣ + e⁻ˣ)

Range: (-1, 1)
Use: Hidden layers, zero-centered output
Derivative: f'(x) = 1 - f(x)²
```

### 4. ReLU (Rectified Linear Unit)

```
           │        ╱
           │      ╱
           │    ╱
           │  ╱
         0 ┤──────
           └─────┼─────────→
                 0

f(x) = max(0, x)

Range: [0, ∞)
Use: Hidden layers (most popular)
Derivative: f'(x) = { 1 if x > 0
                    { 0 if x ≤ 0
```

### Comparison Table

```
┌────────────┬─────────────┬────────────────────────────────────┐
│ Function   │   Range     │ When to Use                        │
├────────────┼─────────────┼────────────────────────────────────┤
│ Step       │ {0, 1}      │ Simple binary, perceptron          │
│ Sigmoid    │ (0, 1)      │ Output layer (binary class)        │
│ Tanh       │ (-1, 1)     │ Hidden layers (zero-centered)      │
│ ReLU       │ [0, ∞)      │ Hidden layers (fast, simple)       │
│ Softmax    │ (0, 1)      │ Output layer (multi-class)         │
└────────────┴─────────────┴────────────────────────────────────┘
```

---

## Perceptron: Single Layer Neural Network

### Structure

```
┌─────────────────────────────────────────────────────────────┐
│                      PERCEPTRON                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│    x₁ ────w₁────┐                                           │
│                 │                                            │
│    x₂ ────w₂────┼──→ [Σ] ──→ [Step] ──→ y ∈ {0, 1}         │
│                 │                                            │
│    x₃ ────w₃────┘                                           │
│                 ↑                                            │
│               bias (b)                                       │
│                                                              │
│    Output: y = step(w₁x₁ + w₂x₂ + w₃x₃ + b)                │
│                                                              │
│    step(z) = 1 if z ≥ 0, else 0                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Perceptron Learning Algorithm

```
PERCEPTRON LEARNING ALGORITHM:
──────────────────────────────

1. Initialize weights w₁, w₂, ..., wₙ and bias b to small random values (or 0)
2. Set learning rate η (eta), typically 0.1 to 1.0
3. For each training example (x, target):
   a. Compute output: y = step(Σ wᵢxᵢ + b)
   b. Calculate error: error = target - y
   c. Update weights: wᵢ = wᵢ + η × error × xᵢ
   d. Update bias: b = b + η × error
4. Repeat until convergence or max iterations

Weight Update Rule:
┌─────────────────────────────────────────┐
│  Δwᵢ = η × (target - output) × xᵢ      │
│  wᵢ(new) = wᵢ(old) + Δwᵢ               │
└─────────────────────────────────────────┘
```

---

## Perceptron Solved Example 1: AND Gate

### Problem

Train a perceptron to learn the AND function:

```
┌─────┬─────┬────────┐
│ x₁  │ x₂  │ Target │
├─────┼─────┼────────┤
│  0  │  0  │   0    │
│  0  │  1  │   0    │
│  1  │  0  │   0    │
│  1  │  1  │   1    │
└─────┴─────┴────────┘

Initial: w₁ = 0, w₂ = 0, b = 0
Learning rate η = 1
```

### Solution

```
EPOCH 1:
────────

Training Example 1: x = (0, 0), target = 0
├── Sum = 0×0 + 0×0 + 0 = 0
├── Output = step(0) = 1  (assuming step(0) = 1)
├── Error = 0 - 1 = -1
├── Δw₁ = 1 × (-1) × 0 = 0
├── Δw₂ = 1 × (-1) × 0 = 0
├── Δb = 1 × (-1) = -1
└── New: w₁ = 0, w₂ = 0, b = -1

Training Example 2: x = (0, 1), target = 0
├── Sum = 0×0 + 0×1 + (-1) = -1
├── Output = step(-1) = 0
├── Error = 0 - 0 = 0
└── No update needed (error = 0)

Training Example 3: x = (1, 0), target = 0
├── Sum = 0×1 + 0×0 + (-1) = -1
├── Output = step(-1) = 0
├── Error = 0 - 0 = 0
└── No update needed

Training Example 4: x = (1, 1), target = 1
├── Sum = 0×1 + 0×1 + (-1) = -1
├── Output = step(-1) = 0
├── Error = 1 - 0 = 1
├── Δw₁ = 1 × 1 × 1 = 1
├── Δw₂ = 1 × 1 × 1 = 1
├── Δb = 1 × 1 = 1
└── New: w₁ = 1, w₂ = 1, b = 0


EPOCH 2:
────────

Training Example 1: x = (0, 0), target = 0
├── Sum = 1×0 + 1×0 + 0 = 0
├── Output = step(0) = 1
├── Error = 0 - 1 = -1
├── Δb = -1
└── New: w₁ = 1, w₂ = 1, b = -1

Training Example 2: x = (0, 1), target = 0
├── Sum = 1×0 + 1×1 + (-1) = 0
├── Output = step(0) = 1
├── Error = 0 - 1 = -1
├── Δw₂ = -1
├── Δb = -1
└── New: w₁ = 1, w₂ = 0, b = -2

Training Example 3: x = (1, 0), target = 0
├── Sum = 1×1 + 0×0 + (-2) = -1
├── Output = step(-1) = 0
├── Error = 0
└── No update

Training Example 4: x = (1, 1), target = 1
├── Sum = 1×1 + 0×1 + (-2) = -1
├── Output = step(-1) = 0
├── Error = 1 - 0 = 1
├── Δw₁ = 1, Δw₂ = 1, Δb = 1
└── New: w₁ = 2, w₂ = 1, b = -1

... (Continue until convergence)


FINAL RESULT (after convergence):
─────────────────────────────────

w₁ = 1, w₂ = 1, b = -1.5 (or similar)

Verification:
┌─────┬─────┬───────────────────┬────────┬────────┐
│ x₁  │ x₂  │ Sum = x₁+x₂-1.5   │ Output │ Target │
├─────┼─────┼───────────────────┼────────┼────────┤
│  0  │  0  │  -1.5             │   0    │   0 ✓  │
│  0  │  1  │  -0.5             │   0    │   0 ✓  │
│  1  │  0  │  -0.5             │   0    │   0 ✓  │
│  1  │  1  │  +0.5             │   1    │   1 ✓  │
└─────┴─────┴───────────────────┴────────┴────────┘

AND gate learned successfully!
```