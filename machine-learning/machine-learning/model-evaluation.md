# Model Evaluation

### 1. Classification

| Metric    | What (Description)                        | How (Formula or Code)                            | Why (Rationale)                                 | So What (Interpretation)                         |
| --------- | ----------------------------------------- | ------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------ |
| Accuracy  | Proportion of correctly predicted samples | `(TP + TN) / (TP + TN + FP + FN)`                | Simple overview of performance                  | High accuracy means most predictions are correct |
| Precision | TP among predicted positives              | `TP / (TP + FP)`                                 | Measures exactness, useful when FP is costly    | High precision = low false alarms                |
| Recall    | TP among actual positives                 | `TP / (TP + FN)`                                 | Measures completeness, useful when FN is costly | High recall = low false negatives                |
| F1 Score  | Harmonic mean of precision and recall     | `2 * (P * R) / (P + R)`                          | Balances precision and recall                   | Good for imbalanced classes                      |
| ROC-AUC   | Area under ROC curve                      | `sklearn.metrics.roc_auc_score(y_true, y_probs)` | Threshold independent measure of separability   | Higher = better distinction between classes      |
| PR-AUC    | Area under Precision-Recall curve         | `sklearn.metrics.average_precision_score()`      | Focused on imbalanced datasets                  | More informative than ROC in imbalanced cases    |
| Log Loss  | Penalizes wrong confident predictions     | `-1/N * sum(y*log(p) + (1-y)*log(1-p))`          | Confidence-aware measure                        | Lower = better calibrated predictions            |

***

### 2. Regression

| Metric      | What (Description)                  | How (Formula or Code)           | Why (Rationale)                        | So What (Interpretation)                   |
| ----------- | ----------------------------------- | ------------------------------- | -------------------------------------- | ------------------------------------------ |
| MAE         | Mean Absolute Error                 | `np.mean(abs(y_true - y_pred))` | Direct error magnitude                 | Lower MAE = better accuracy                |
| MSE         | Mean Squared Error                  | `np.mean((y_true - y_pred)**2)` | Penalizes large errors more            | Higher sensitivity to large errors         |
| RMSE        | Root Mean Squared Error             | `np.sqrt(MSE)`                  | Puts error in original units           | Useful for understanding real-world impact |
| R2 Score    | Coefficient of Determination        | `1 - (SS_res / SS_tot)`         | Indicates % variance explained         | Closer to 1 = better model                 |
| Adjusted R2 | R2 penalized for number of features | `1 - (1-R2)*(n-1)/(n-p-1)`      | Avoids overfitting with more variables | Better metric for multivariate regression  |

***

### 3. Clustering

| Metric           | What (Description)                         | How (Formula or Code)                            | Why (Rationale)                         | So What (Interpretation)         |
| ---------------- | ------------------------------------------ | ------------------------------------------------ | --------------------------------------- | -------------------------------- |
| Silhouette Score | Compactness vs. separation                 | `(b - a) / max(a, b)`                            | Measures cluster quality                | Close to 1 = well-clustered      |
| Davies-Bouldin   | Ratio of intra/inter-cluster distances     | `mean(max((Si + Sj)/Mij))`                       | Lower = better separated clusters       | Lower DBI = better clustering    |
| Inertia (SSE)    | Sum of squared distances to centroids      | `sum((x_i - centroid)^2)`                        | Evaluates compactness                   | Lower inertia = tighter clusters |
| Elbow Method     | Plot of inertia vs. k                      | `plt.plot(k, inertia)`                           | Helps choose optimal number of clusters | Elbow point = best k             |
| ARI              | Similarity between cluster and true labels | `sklearn.metrics.adjusted_rand_score()`          | Adjusts for chance                      | Closer to 1 = better agreement   |
| NMI              | Shared info between clusters & true labels | `sklearn.metrics.normalized_mutual_info_score()` | Scales from 0 to 1                      | Higher = better match            |

***

### 4. Dimensionality Reduction

| Metric             | What (Description)                        | How (Formula or Code)                | Why (Rationale)                                  | So What (Interpretation)                         |
| ------------------ | ----------------------------------------- | ------------------------------------ | ------------------------------------------------ | ------------------------------------------------ |
| Explained Variance | Variance captured by components           | `pca.explained_variance_ratio_`      | Measures how much info is retained               | Higher = better preservation of structure        |
| Reconstruction Err | Error between original and projected data | `mean((X - X_reconstructed)^2)`      | Measures loss of information                     | Lower = better fidelity                          |
| Trustworthiness    | Preservation of local structure           | `sklearn.manifold.trustworthiness()` | Measures local relationships                     | Higher = better local mapping                    |
| KL Divergence      | Used in t-SNE to match distributions      | `sum(p * log(p/q))`                  | Measures divergence between high-dim and low-dim | Lower = better preservation of density structure |

***

### 5. Anomaly Detection

| Metric             | What (Description)                        | How (Formula or Code)                  | Why (Rationale)                            | So What (Interpretation)                        |
| ------------------ | ----------------------------------------- | -------------------------------------- | ------------------------------------------ | ----------------------------------------------- |
| Precision/Recall   | As in classification (outliers=positive)  | See classification formulas            | Imbalanced problems; need specific metrics | High = correct detection of anomalies           |
| F1 Score           | Harmonic mean of precision and recall     | `2 * (P * R) / (P + R)`                | Balances detection accuracy                | Higher = better anomaly detection               |
| ROC-AUC            | Area under ROC for anomaly class          | `roc_auc_score()`                      | Measures rank ordering                     | Higher = better separation of anomalies         |
| PR-AUC             | Area under PR curve                       | `average_precision_score()`            | Better for skewed data                     | High = better precision-recall trade-off        |
| Reconstruction Err | Used in autoencoders                      | `mean((x - x_hat)^2)`                  | High = potential anomaly                   | Identify anomalies by high reconstruction error |
| Isolation Depth    | Avg. tree path length in Isolation Forest | `isolation_forest.decision_function()` | Shorter = more likely anomaly              | Lower score = more isolated (anomalous)         |

### Cross-Validation in Model Evaluation

**What:** Cross-validation is a statistical method used to estimate the performance of a machine learning model on an independent dataset. It helps ensure that the model generalizes well to unseen data and doesn't simply overfit the training set.

**Why:**

1. **Overfitting Prevention:** Cross-validation helps reduce the risk of overfitting by testing the model on multiple different subsets of the data.
2. **Reliable Performance Estimate:** Provides a more reliable estimate of model performance compared to a single train-test split.
3. **Better Use of Data:** Utilizes all available data by training the model on different subsets and testing on others.

**How:** The idea is to split the data into multiple subsets (folds), train the model on a subset, and validate it on another. This process is repeated several times to understand how the model will perform on unseen data.

**Common Types of Cross-Validation:**

1.  **K-Fold Cross-Validation:**

    * The data is split into **K** subsets or folds. The model is trained on K-1 folds and validated on the remaining fold. This process is repeated K times with a different fold as the test set.
    * **Advantages:** Works well for most cases; provides a good estimate of the model’s performance.
    * **Disadvantages:** Computationally expensive when K is large, especially for large datasets.

    **Example:**

    * Split the data into 5 folds.
    * Train on folds 1, 2, 3, and 4, validate on fold 5.
    * Repeat the process until all folds have been used for validation.
2. **Stratified K-Fold Cross-Validation:**
   * A variation of K-fold cross-validation where each fold has the same proportion of each class label (ideal for imbalanced datasets).
3. **Leave-One-Out Cross-Validation (LOOCV):**
   * A special case of K-fold where K equals the number of data points. For each iteration, the model is trained on all data points except one, which is used as the test set.
   * **Advantages:** It uses all data for training but can be computationally expensive for large datasets.
4. **Leave-P-Out Cross-Validation:**
   * A generalized version of LOOCV where **P** observations are used as the test set, and the rest are used for training.
5. **Time Series Cross-Validation:**
   * Used for time series data where the data is split based on temporal order. Future data cannot be used to predict past data, so the splits are made to respect this.

***

#### Code Examples

**1. K-Fold Cross-Validation (Standard)**

```python
pythonCopy codefrom sklearn.model_selection import KFold, cross_val_score
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris

# Load data
data = load_iris()
X, y = data.data, data.target

# Initialize model
model = RandomForestClassifier(n_estimators=100)

# KFold Cross-validation (5 folds)
kf = KFold(n_splits=5, shuffle=True, random_state=42)

# Cross-validation scores
cv_scores = cross_val_score(model, X, y, cv=kf)
print(f'Cross-validation scores: {cv_scores}')
print(f'Mean CV score: {cv_scores.mean()}')
```

**2. Stratified K-Fold Cross-Validation (for Imbalanced Classes)**

```python
pythonCopy codefrom sklearn.model_selection import StratifiedKFold
from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier

# Load data
data = load_iris()
X, y = data.data, data.target

# Initialize model
model = RandomForestClassifier(n_estimators=100)

# Stratified KFold (5 folds)
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

# Cross-validation scores
cv_scores = cross_val_score(model, X, y, cv=skf)
print(f'Stratified CV scores: {cv_scores}')
print(f'Mean CV score: {cv_scores.mean()}')
```

**3. Leave-One-Out Cross-Validation (LOOCV)**

```python
pythonCopy codefrom sklearn.model_selection import LeaveOneOut
from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier

# Load data
data = load_iris()
X, y = data.data, data.target

# Initialize model
model = RandomForestClassifier(n_estimators=100)

# Leave-One-Out Cross-Validation
loo = LeaveOneOut()

# Cross-validation scores
cv_scores = cross_val_score(model, X, y, cv=loo)
print(f'LOO CV scores: {cv_scores}')
print(f'Mean LOO CV score: {cv_scores.mean()}')
```

**4. Time Series Cross-Validation**

```python
pythonCopy codefrom sklearn.model_selection import TimeSeriesSplit
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris

# Load data
data = load_iris()
X, y = data.data, data.target

# Initialize model
model = RandomForestClassifier(n_estimators=100)

# TimeSeries Split (5 folds)
tscv = TimeSeriesSplit(n_splits=5)

# Cross-validation scores
cv_scores = cross_val_score(model, X, y, cv=tscv)
print(f'TimeSeries CV scores: {cv_scores}')
print(f'Mean TimeSeries CV score: {cv_scores.mean()}')
```

***

#### So What:

* **Validation across Multiple Folds:** Cross-validation provides a more reliable estimate of the model's ability to generalize, as each data point gets tested at least once.
* **Reduces Overfitting:** Training and testing on different subsets of the data ensures that the model doesn't just memorize the training data.
* **Hyperparameter Tuning:** Cross-validation is commonly used in conjunction with hyperparameter tuning to find the optimal configuration of a model.

### Further Cheat Sheet

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>
