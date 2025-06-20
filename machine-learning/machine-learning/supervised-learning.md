# Supervised Learning

### Regression vs Classification

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><a href="https://www.google.com/url?sa=i&#x26;url=https%3A%2F%2Fmedium.com%2Fenjoy-algorithm%2Fclassification-and-regression-problems-in-machine-learning-83b8fc9ab958&#x26;psig=AOvVaw0jVhI-3Wgx9i5kk11pFoNf&#x26;ust=1745116193600000&#x26;source=images&#x26;cd=vfe&#x26;opi=89978449&#x26;ved=0CBcQjhxqFwoTCOCJu-GG44wDFQAAAAAdAAAAABAE">source</a></p></figcaption></figure>

### Binary vs MultiClass  vs MultiLabel classification

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p><a href="https://www.microsoft.com/en-us/research/uploads/prod/2017/12/40250.jpg">source</a> </p></figcaption></figure>

### Linear Regression

> Linear Relationship between dependent variable (Outcome/target) variable and one or more independent (predictor) variables.

#### Assumptions / Conditions

<table><thead><tr><th width="204.88909912109375">Assumption</th><th>Description</th></tr></thead><tbody><tr><td>Linearity</td><td>Relationship between features and target is linear</td></tr><tr><td>Homoscedasticity</td><td>Constant variance of errors</td></tr><tr><td>No Multicollinearity</td><td>Features are not highly correlated</td></tr><tr><td>Independence</td><td>Observations are independent</td></tr><tr><td>Normality</td><td>Residuals are normally distributed</td></tr></tbody></table>

#### Target

$$
y = \beta_0 + \beta_1x_1 + \beta_2x_2 + \dots + \beta_nx_n + \epsilon
$$

#### Predicted

$$
\hat{y} = \hat{\beta_0} + \hat{\beta_1}x_1 + \hat{\beta_2}x_2 + \dots + \hat{\beta_n}x_n
$$

#### MSE (Cost function)

$$
J(\theta) = \frac{1}{n} \sum_{i=1}^{n} (\hat{y}_i - y_i)^2
$$

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><a href="https://www.google.com/url?sa=i&#x26;url=https%3A%2F%2Fwww.grammarly.com%2Fblog%2Fai%2Fwhat-is-linear-regression%2F&#x26;psig=AOvVaw0pnHvr-AfbJx31Pjh2MC1H&#x26;ust=1745114558325000&#x26;source=images&#x26;cd=vfe&#x26;opi=89978449&#x26;ved=0CBcQjhxqFwoTCPDhs72A44wDFQAAAAAdAAAAABAK">A simple line of best fit minimizing squared error</a> </p></figcaption></figure>

#### Code Sample

```python
from sklearn.linear_model import LinearRegression
from sklearn.datasets import make_regression

# Create dummy data
X, y = make_regression(n_samples=100, n_features=1, noise=15)

# Fit the model
model = LinearRegression()
model.fit(X, y)
y_pred = model.predict(X)

```

#### Loss Functions

* Mean Squared Error (MSE)
* Mean Absolute Error (MAE)

Evaluation Metrics

* R² Score
* Root Mean Squared Error
* MAE

Optimization Techniques

* **Gradient Descent**
* **Normal Equation** (Analytical Solution)
* L1 (Lasso) & L2 (Ridge) Regularization

#### Common Issues

<table><thead><tr><th width="215.21514892578125">Issue</th><th>Description</th></tr></thead><tbody><tr><td>Overfitting</td><td>Especially with too many features</td></tr><tr><td>Underfitting</td><td>When model is too simple</td></tr><tr><td>Outliers</td><td>Can distort the line</td></tr><tr><td>Collinearity</td><td>Leads to unstable coefficients</td></tr><tr><td>Assumption Violations</td><td>Leads to incorrect inferences</td></tr></tbody></table>

Further Reading: [https://mlu-explain.github.io/linear-regression/](https://mlu-explain.github.io/linear-regression/)

***

### Logistic Regression

> Predicts the probability of categorical variables (Classes) based on Input features.
>
> It models the relationship using the **logit (log-odds)** function instead of a straight line.
>
> _Common application_ : _Outcome can belong to **one or two classes.**_ [<sup><sub>**Binary classification**<sub></sup>](https://en.wikipedia.org/wiki/Binary_classification)
>
> Also extended to: predict multiple classes/labels ([OneVsRestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.multiclass.OneVsRestClassifier.html)) . [<sup><sub>**Mulit class classification**<sub></sup>](https://en.wikipedia.org/wiki/Multiclass_classification)<sup><sub>**,**<sub></sup> [<sup><sub>**Mulit label classification**<sub></sup>](https://en.wikipedia.org/wiki/Multi-label_classification)

#### Assumptions / Conditions

| Assumption                 | Description                                         |
| -------------------------- | --------------------------------------------------- |
| Binary/Multinomial Outcome | The dependent variable is categorical               |
| Linearity of Logit         | Linear relationship between predictors and log-odds |
| No Multicollinearity       | Features should not be highly correlated            |
| Large Sample Size          | Especially for rare outcomes                        |
| Independent Observations   | Each observation is assumed to be independent       |

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p><a href="https://medium.com/data-science/logistic-regression-explained-in-7-minutes-f648bf44d53e">source</a></p></figcaption></figure>

```python
from sklearn.linear_model import LogisticRegression
from sklearn.datasets import make_classification

# Create dummy data
X, y = make_classification(n_samples=100, n_features=2, n_classes=2, random_state=42)

# Fit the model
model = LogisticRegression()
model.fit(X, y)
y_pred = model.predict(X)

```

#### Loss Functions

* Binary Cross Entropy (Log Loss)
* Categorical Cross Entropy (for multi-class)

Evaluation Metrics

* Accuracy
* Precision, Recall, F1 Score
* ROC-AUC
* Log Loss

Optimization Techniques

* Gradient Descent
* L2 Regularization (Ridge-like)
* L1 Regularization (Lasso-like)

#### Common Issues

| Issue             | Description                                 |
| ----------------- | ------------------------------------------- |
| Overfitting       | Happens with high dimensional feature space |
| Non-linearity     | Poor fit if decision boundary isn’t linear  |
| Imbalanced Data   | Skews performance, needs resampling         |
| Multicollinearity | Makes coefficients unstable                 |

Further Reading: [https://mlu-explain.github.io/logistic-regression/](https://mlu-explain.github.io/logistic-regression/)

***

### Decision Tree

> A decision tree is  used for both _classification and regression_ tasks.&#x20;
>
> It splits data into subsets on the value of input features and then forms a tree structure to make decision.&#x20;
>
> Each node represents a decision.
>
> Each branch represents an outcome of that decision.
>
> Each leaf node represents a finial decision i.e. classification.
>
> Structure: Root Node -> Branches -> leaf node -> branches->.......................

#### Assumptions / Conditions

<table><thead><tr><th>Assumption</th><th width="341.709228515625">Description</th></tr></thead><tbody><tr><td>Features are Independent</td><td>Each feature is considered separately for splitting.</td></tr><tr><td>Sufficient Data</td><td>Pruning or constraints are required to avoid overfitting.</td></tr><tr><td>Data is Clean</td><td>Noisy data may lead to overly complex trees.</td></tr><tr><td>Recursive Binary Splits</td><td>Splits are made recursively, usually into two branches.</td></tr></tbody></table>

Key concepts: [<sub>refer sklearn</sub>](https://scikit-learn.org/stable/modules/tree.html)

<table><thead><tr><th width="155.7176513671875">Concept</th><th width="234.02191162109375">Description	</th><th width="297.10406494140625">How It Works / Why It Matters</th></tr></thead><tbody><tr><td>Entropy</td><td>Measures disorder/uncertainty in the dataset. </td><td>High entropy = more mixed classes. <br>We want to reduce entropy with every split to get purer subsets.</td></tr><tr><td>Information Gain</td><td>How much entropy decreases after splitting.</td><td>Used to decide which feature to split on; the feature that reduces uncertainty the most is chosen.</td></tr><tr><td>Gini Impurity</td><td>Measures the chance of misclassifying a sample.</td><td>Lower Gini = purer split. Fast to compute and works well in practice.</td></tr><tr><td>Gain ratio</td><td>Adjusts Information Gain by penalizing high-cardinality features.</td><td>Prevents bias towards features with many unique values.</td></tr><tr><td>Chi-Square</td><td>Statistical test to see if split is meaningful.</td><td>Bigger chi-square value = more significant split.</td></tr><tr><td>Max depth</td><td>Maximum depth of the tree.</td><td>Shallow trees generalize better, avoid overfitting.</td></tr><tr><td>Min sample split</td><td>Minimum samples required to split a node.</td><td>Prevents splits on small, noisy sets.</td></tr><tr><td>Min sample leaf</td><td>Minimum samples required to be at a leaf node.</td><td>Smooths predictions and reduces variance.</td></tr><tr><td>Max Features</td><td>Number of features to consider when looking for the best split.</td><td>Adds randomness, helps in ensemble methods like Random Forest.</td></tr><tr><td>Pruning (Post or Pre)</td><td>Removes unnecessary branches.</td><td>Reduces model complexity and improves generalization by trimming low-information parts of the tree.</td></tr><tr><td>Overfitting</td><td>When the tree memorizes training data.</td><td>Deep trees with few constraints tend to overfit. Fix using pruning or regularization.</td></tr><tr><td>Underfitting</td><td>When the tree is too simple to learn from the data.</td><td>Usually caused by too shallow trees or too strict constraints.</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p><a href="https://www.datacamp.com/tutorial/decision-tree-classification-python">source</a></p></figcaption></figure>

```python
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.datasets import load_iris
import matplotlib.pyplot as plt

# Load dataset
X, y = load_iris(return_X_y=True)

# Fit the model
model = DecisionTreeClassifier(max_depth=3)
model.fit(X, y)

# Visualize
plt.figure(figsize=(12, 8))
plot_tree(model, filled=True, feature_names=["Sepal Length", "Sepal Width", "Petal Length", "Petal Width"])
plt.show()
```

#### Loss Functions

* Gini Impurity
* Entropy (for classification)
* MSE / MAE (for regression)

#### Evaluation Metrics

| Classification        | Regression |
| --------------------- | ---------- |
| Accuracy              | R² Score   |
| Precision, Recall, F1 | RMSE, MAE  |
| Confusion Matrix      | MSE        |

#### Optimization Techniques

* **Pruning** (pre-pruning, post-pruning)
* **Max Depth, Min Samples Split/Leaf**
* **Feature Selection Criteria**

#### Common Issues

| Issue                                  | Description                                               |
| -------------------------------------- | --------------------------------------------------------- |
| Overfitting                            | Deep trees may fit noise in training data                 |
| Instability                            | Small changes in data can result in a very different tree |
| Bias towards features with more levels | May prefer variables with many categories                 |
| Low Generalization                     | Can perform poorly on unseen data without pruning         |

Further Reading: [https://mlu-explain.github.io/decision-tree/](https://mlu-explain.github.io/decision-tree/)

***

### Ensemble Learning/Methods

> **Idea of combining multiple models ---> leads to better performance.**
>
> By aggregating predictions of different models, we can reduce variance and/or bais and imporve genralization, making it more performance effiecent.&#x20;
>
> Types: Bagging , Boosting, Stacking

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

### When to use Bagging vs Boosting

Bagging: When you have a high variance model that is overfitting on training data.&#x20;

Boosting: When you have a model with high bias and/or high variance, you need to improve.

### Random Forest&#x20;

> Multiple decision trees are combined to make predictions are Random forest  (Bagging- Bootstrap Aggregating)
>
> Each tree is trained on a **random subset of the data (with replacement)**.
>
> At each split, a **random subset of features** is selected.
>
> For classification, the final output is the **majority vote**.
>
> For regression, the final output is the **average prediction**.

#### Assumptions / Conditions

| Assumption / Condition      | Description                                              |
| --------------------------- | -------------------------------------------------------- |
| Independence of features    | Features should be informative and not highly correlated |
| Sufficient data             | More data = better diversity in trees                    |
| No need for feature scaling | Random Forest is scale-invariant                         |

#### Cost Function / Objective

Random Forest has no **global cost function** like MSE or cross-entropy. Each tree is built to **minimize impurity** (Gini or Entropy), and the final prediction is based on **ensemble voting or averaging**.

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Load dataset
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Train the model
model = RandomForestClassifier(n_estimators=100, max_depth=3, random_state=42)
model.fit(X_train, y_train)

# Predict and evaluate
y_pred = model.predict(X_test)
print("Accuracy:", accuracy_score(y_test, y_pred))

```

#### Common Issues

| Issue                       | Description                                              |
| --------------------------- | -------------------------------------------------------- |
| Overfitting                 | With too many deep trees on noisy data                   |
| Interpretability            | Hard to interpret full forest compared to a single tree  |
| Bias in Imbalanced Datasets | Tends to favor majority class                            |
| Large Size                  | Can be slow or memory intensive with too many estimators |

#### Further Reading: [MLU Explain - Random Forest](https://mlu-explain.github.io/random-forest/)

### XGBoost (Extreme Gradient Boosting)

> **XGBoost** is a fast, regularized, and scalable implementation of Gradient Boosted Decision Trees (GBDT).\
> It builds trees **sequentially**, where each tree tries to correct the errors made by the previous ones.

#### Assumptions / Conditions

| Assumption / Condition      | Description                                      |
| --------------------------- | ------------------------------------------------ |
| Additive Model              | New trees correct residuals of previous ensemble |
| No need for feature scaling | Works well with raw or normalized data           |
| Clean data preferred        | Sensitive to outliers and noise                  |
| Independent features help   | Redundant features may affect performance        |

```python
import xgboost as xgb
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Load dataset
X, y = load_breast_cancer(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Train the model
model = xgb.XGBClassifier(use_label_encoder=False, eval_metric='logloss', max_depth=3, n_estimators=100)
model.fit(X_train, y_train)

# Predict and evaluate
y_pred = model.predict(X_test)
print("Accuracy:", accuracy_score(y_test, y_pred))

```

#### Common Issues

| Issue                     | Description                                        |
| ------------------------- | -------------------------------------------------- |
| Overfitting               | Happens with too many trees or no regularization   |
| High memory usage         | Can be computationally expensive on large datasets |
| Sensitive to noisy labels | Can overfit if data is not clean                   |
| Harder to interpret       | Compared to single decision trees                  |

#### Further Reading: [Official XGBoost Docs](https://xgboost.readthedocs.io/)

### K-Nearest Neighbour

> kNN is a non-parmetric model becuase it makes assumptions that similar data points are  locted close to each other in feature space.&#x20;
>
> New labels or values of new data points would be inferred based on its closest neighbors.
>
> It is based on  Euclidean, Manhattan, Minkowski or hamming distance

| Assumption / Condition        | Description                                                                |
| ----------------------------- | -------------------------------------------------------------------------- |
| Instance-based learning       | No explicit training; relies on storing and comparing instances.           |
| Local similarity assumption   | Assumes that similar points exist in close proximity in the feature space. |
| Sensitivity to feature scale  | Distance-based metrics require features to be on similar scales.           |
| Impact of irrelevant features | Irrelevant or noisy features can distort distance calculations.            |

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score

# Load dataset
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Feature scaling
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# Train kNN model
knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X_train, y_train)

# Predict and evaluate
y_pred = knn.predict(X_test)
print("Accuracy:", accuracy_score(y_test, y_pred))

```

| Common Issue                   | Description                                                                |
| ------------------------------ | -------------------------------------------------------------------------- |
| Poor scalability               | Slow prediction with large datasets (must compare to all training points). |
| Curse of dimensionality        | High-dimensional data can weaken the meaning of "closeness".               |
| Requires feature normalization | Performance highly depends on the proper scaling of features.              |

