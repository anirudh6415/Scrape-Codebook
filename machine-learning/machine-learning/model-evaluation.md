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

### Further Cheat Sheet

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>
