# Training Techniques

## Gradient Descent

Gradient Descent is an **optimization algorithm** used to minimize a **loss (or cost) function** by iteratively adjusting the model parameters (like weights in linear regression or neural networks).

#### **Goal**

Find the parameters that **minimize the error** between the model's predictions and the actual values.

#### **How It Works**

1. **Start** with initial values for your parameters (often randomly).
2. **Compute the gradient (i.e., derivative) of the loss function with respect to each parameter.**
3. **Update the parameters** by moving in the **opposite direction of the gradient**.

#### **Intuition**

Imagine standing on a hilly landscape. Your goal is to reach the lowest point (minimum). At every step, you look around (compute the slope) and take a step downhill. That’s gradient descent!

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

### **Batch Gradient Descent**

**What**: Computes the gradient using the **entire dataset**.

**Why**: Provides a stable and accurate gradient estimate.

**How**:

```python
# Pseudocode
for epoch in range(epochs):
    gradient = compute_gradient(X, y)
    weights -= learning_rate * gradient
```

**So What**:

* Very memory-intensive, slow on large datasets.
* Stable convergence but may get stuck in local minima.

***

### **Stochastic Gradient Descent (SGD)**

**What**: Computes gradient for **each data point individually**.

**Why**: Faster and can escape local minima due to its noisy updates.

**How**:

```python
for epoch in range(epochs):
    for i in range(len(X)):
        gradient = compute_gradient(X[i], y[i])
        weights -= learning_rate * gradient
```

**So What**:

* Faster per update but can be noisy.
* Good for large datasets and online learning.

***

### **Mini-Batch Gradient Descent**

**What**: Computes gradient on **small batches** of data.

**Why**: Combines advantages of batch and stochastic methods.

**How**:

```python
for epoch in range(epochs):
    for batch in batches(X, y, batch_size):
        gradient = compute_gradient(batch_X, batch_y)
        weights -= learning_rate * gradient
```

**So What**:

* Faster than batch, more stable than SGD.
* Most widely used technique in deep learning.

***

## **Learning Rate Scheduling**

**What**: Adjusts the learning rate during training.

**Why**: Helps converge faster and avoid overshooting minima.

**How**:

*   **Step Decay**:

    ```python
    lr = initial_lr * (drop_rate ** floor(epoch / drop_every))
    ```
* **Exponential Decay**:

$$
lr= lr_0 \cdot e^{-k \cdot epoch}
$$

* **Cosine Annealing**, **Warm Restarts**, **ReduceLROnPlateau** (PyTorch, Keras)

**So What**:

* Prevents divergence in early epochs.
* Helps escape saddle points and fine-tune convergence.

***

## **Epochs vs Iterations**

**What**:

* **Epoch**: One full pass through the entire training dataset.
* **Iteration**: One update step (i.e., one batch processed).

**Why**: Important to understand training duration and resource planning.

**How**:

$$
\text{Iterations per epoch }= \frac{\text{Number of training samples}}{\text{Batch size}}
$$



**So What**:

* Adjust batch size to balance convergence speed and memory usage.
* Too many epochs may overfit; too few may underfit.

***

## **Class Imbalance Handling**

### **Class Weights**

**What: Assigns a higher loss to the minority class.**

**Why: Prevents the majority class from dominating the loss function.**

**How**:

```python
from sklearn.utils.class_weight import compute_class_weight
weights = compute_class_weight('balanced', classes=np.unique(y), y=y)
```

**So What**:

* Simple and effective for classification imbalance.

***

### **SMOTE (Synthetic Minority Oversampling Technique)**

**What**: Generates synthetic samples for minority class.

**Why**: Balances the dataset without losing any data.

**How**:

```python
from imblearn.over_sampling import SMOTE
X_res, y_res = SMOTE().fit_resample(X, y)
```

**So What**:

* Improves model generalization for minority class.
* May introduce noise if not used carefully.

