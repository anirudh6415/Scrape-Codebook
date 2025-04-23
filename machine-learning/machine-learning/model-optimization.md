# Model Optimization

## **Regularization**

**What:** Regularization techniques are used to prevent overfitting by adding a penalty to the loss function. This penalty discourages the model from becoming overly complex by penalizing large coefficients or weights.

### **L1 Regularization (Lasso)**

* **What:** Adds the sum of the absolute values of the coefficients as a penalty term to the loss function.
* **Why:** Helps with feature selection, as L1 regularization can drive some coefficients to exactly zero, effectively removing them.
* How:&#x20;

$$
L = \text{Loss Function} + \lambda \sum_{i=1}^{n} |w_i|
$$

### **L2 Regularization (Ridge)**

* **What:** Adds the sum of the squared values of the coefficients as a penalty term to the loss function.
*   **Why:** Prevents large weights but does not eliminate features entirely.\


    $$
    L = \text{Loss Function} + \lambda \sum_{i=1}^{n} w_i^2
    $$

### **Elastic Net (Combination of L1 and L2)**

* **What:** Combines both L1 and L2 penalties, offering the benefits of both Lasso and Ridge regularization.
*   **Why:** Useful when there are multiple correlated features.\


    $$
    L = \text{Loss Function} + \lambda_1 \sum_{i=1}^{n} |w_i| + \lambda_2 \sum_{i=1}^{n} w_i^2
    $$

**Code Example:**

```python
from sklearn.linear_model import Ridge, Lasso, ElasticNet

# L2 Regularization (Ridge)
ridge = Ridge(alpha=1.0)
ridge.fit(X_train, y_train)

# L1 Regularization (Lasso)
lasso = Lasso(alpha=0.1)
lasso.fit(X_train, y_train)

# ElasticNet Regularization (Combination of L1 and L2)
elastic_net = ElasticNet(alpha=0.1, l1_ratio=0.5)
elastic_net.fit(X_train, y_train)
```

***

## **Hyperparameter Tuning**

**What:**\
Hyperparameter tuning is the process of selecting the optimal values for a model’s hyperparameters. Hyperparameters are parameters that are set before training the model (e.g., learning rate, number of estimators, depth of trees).

### **Grid Search**

* **What:** Exhaustively search a specified hyperparameter grid to find the best combination.
* **Why:** It’s useful when you have a limited number of hyperparameters and want to test all possible combinations exhaustively.
* **How:**\
  Grid search tests all possible combinations of a set of predefined hyperparameters.

**Code Example (Grid Search):**

```python
from sklearn.model_selection import GridSearchCV
from sklearn.ensemble import RandomForestClassifier

# Define hyperparameter grid
param_grid = {'n_estimators': [10, 50, 100], 'max_depth': [5, 10, 15]}

# Initialize grid search
grid_search = GridSearchCV(estimator=RandomForestClassifier(), param_grid=param_grid, cv=5)
grid_search.fit(X_train, y_train)

# Best parameters
print(grid_search.best_params_)
```

### **Random Search**

* **What:** Randomly selects hyperparameter values within a predefined range or list of values.
* **Why:** It is more efficient than Grid Search for large hyperparameter spaces because it randomly explores different combinations.
* **How:**\
  Random search selects random combinations of hyperparameters and evaluates the model's performance.

**Code Example (Random Search):**

```python
from sklearn.model_selection import RandomizedSearchCV
from sklearn.ensemble import RandomForestClassifier
from scipy.stats import randint

# Define hyperparameter distribution
param_dist = {'n_estimators': randint(10, 200), 'max_depth': randint(5, 20)}

# Initialize random search
random_search = RandomizedSearchCV(estimator=RandomForestClassifier(), param_distributions=param_dist, n_iter=100, cv=5)
random_search.fit(X_train, y_train)

# Best parameters
print(random_search.best_params_)
```

### **Bayesian Optimization**

* **What:** Uses probabilistic models to select the best set of hyperparameters, based on past evaluation results.
* **Why:** More efficient than Grid and Random search, especially in high-dimensional spaces.
* **How:**\
  Bayesian optimization uses a Gaussian process or similar model to predict the performance of different combinations of hyperparameters.

**Code Example (Using `skopt` library):**

```python
from skopt import BayesSearchCV
from sklearn.ensemble import RandomForestClassifier

# Define hyperparameter search space
search_space = {'n_estimators': (10, 100), 'max_depth': (5, 20)}

# Initialize Bayesian search
bayes_search = BayesSearchCV(estimator=RandomForestClassifier(), search_spaces=search_space, n_iter=50)
bayes_search.fit(X_train, y_train)

# Best parameters
print(bayes_search.best_params_)
```

***

## **Early Stopping**

**What:**\
Early stopping is a technique used to prevent overfitting during training by stopping the training process once the model’s performance on the validation set starts to degrade (indicating overfitting).

**Why:**\
It helps prevent the model from continuing to learn noise from the training data and generalizes better on unseen data.

**How:**\
During training, monitor a performance metric (e.g., validation loss or accuracy). Stop training when the performance stops improving for a specified number of iterations (patience).

**Code Example:**

```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import GradientBoostingClassifier

# Split data into training and validation sets
X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.2, random_state=42)

# Initialize Gradient Boosting model with early stopping
model = GradientBoostingClassifier(n_estimators=1000)
model.fit(X_train, y_train)

# Early stopping on validation score
model.set_params(n_iter_no_change=10, validation_fraction=0.1)
model.fit(X_train, y_train)
```

***

## **Learning Curves**

**What:**\
Learning curves represent how the model’s performance improves over time (with increasing training examples or epochs). They show both training and validation performance.

**Why:**\
Learning curves help diagnose whether a model is overfitting, underfitting, or has optimal learning behavior. By analyzing the curves, you can decide whether to adjust hyperparameters, add more data, or try other techniques like regularization.

**How:**\
Plot training and validation error against the number of training samples or epochs.

**Code Example:**

```python
import matplotlib.pyplot as plt
from sklearn.model_selection import learning_curve
from sklearn.ensemble import RandomForestClassifier

# Load data
X, y = load_iris(return_X_y=True)

# Initialize model
model = RandomForestClassifier()

# Learning curve (using training set size)
train_sizes, train_scores, val_scores = learning_curve(model, X, y, cv=5)

# Plot learning curves
plt.plot(train_sizes, train_scores.mean(axis=1), label='Training Error')
plt.plot(train_sizes, val_scores.mean(axis=1), label='Validation Error')
plt.xlabel('Training Size')
plt.ylabel('Error')
plt.legend()
plt.show()
```
