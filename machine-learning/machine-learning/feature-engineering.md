# Feature Engineering

### **One-Hot Encoding**

* **What**: Converts categorical variables into binary vectors. Each category is represented by a binary value (0 or 1).
* **Why**: Most machine learning models cannot handle categorical strings directly; this transformation makes them numerically interpretable.
*   **How**:

    <figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

    **Code**:

    ```python
    from sklearn.preprocessing import OneHotEncoder
    import numpy as np

    encoder = OneHotEncoder(sparse=False)
    data = np.array(['Red', 'Blue', 'Red']).reshape(-1, 1)
    encoded_data = encoder.fit_transform(data)
    print(encoded_data)
    ```
* **So What**: Prevents models from assuming an ordinal relationship between categories, which can be a problem for models like linear regression.

***

### **Label Encoding**

* **What**: Assigns each category a unique integer value.
* **Why**: Useful for tree-based models, which can interpret label integers properly. But, it’s not ideal for linear models due to potential ordinal bias.
*   **How**:

    **Input**:

    ```python
    Color = ['Red', 'Blue', 'Red']
    ```

    **Output**:

    ```python
    Red = 0, Blue = 1, Red = 0
    ```

    **Code**:

    ```python
    from sklearn.preprocessing import LabelEncoder

    le = LabelEncoder()
    labels = ['Red', 'Blue', 'Red']
    encoded_labels = le.fit_transform(labels)
    print(encoded_labels)
    ```
* **So What**: Saves memory compared to one-hot but not ideal for models that expect ordinal data (e.g., linear regression).

***

### **Scaling – StandardScaler**

* **What**: Rescales features to have zero mean and unit variance.
* **Why**: Ensures all features contribute equally to the distance metrics or gradients, especially important for models like SVM, KNN, and Logistic Regression.
*   **How**:\
    ![](<../../.gitbook/assets/image (15).png>)

    **Code**:

    ```python
    from sklearn.preprocessing import StandardScaler

    scaler = StandardScaler()
    data = [[1, 2], [3, 4], [5, 6]]
    scaled_data = scaler.fit_transform(data)
    print(scaled_data)
    ```
* **So What**: Crucial for algorithms like SVM and KNN, which depend on distances between data points.

***

### **Scaling – Min-Max Normalization**

* **What**: Rescales features to a \[0, 1] range.
* **Why**: Prevents features with large ranges from dominating those with small ranges.
*   **How**:

    <img src="../../.gitbook/assets/image (14).png" alt="" data-size="original">



    **Code**:

    ```python
    from sklearn.preprocessing import MinMaxScaler

    scaler = MinMaxScaler()
    data = [[1, 2], [3, 4], [5, 6]]
    normalized_data = scaler.fit_transform(data)
    print(normalized_data)
    ```
* **So What**: Required for algorithms like Neural Networks and K-Means, where features need to be on the same scale.

***

### **Binning (Discretization)**

* **What**: Groups continuous values into discrete bins.
* **Why**: Helps handle non-linear relationships, reduce noise, or deal with skewed data.
*   **How**:

    **Example**: Age → Child (0–18), Adult (19–60), Senior (60+)

    **Code**:

    ```python
    import pandas as pd
    data = [5, 25, 35, 60, 75]
    bins = [0, 18, 60, 100]
    labels = ['Child', 'Adult', 'Senior']
    binned_data = pd.cut(data, bins=bins, labels=labels)
    print(binned_data)
    ```
* **So What**: Improves interpretability and avoids overfitting in some models by simplifying continuous features.

***

### **Polynomial Features**

* **What**: Generates interaction and higher-order terms from numeric features.
* **Why**: Captures non-linear relationships in linear models.
*   **How**:

    **Input**: `[x1, x2]`

    **Output**: `[x1, x2, x1², x2², x1·x2]`

    **Code**:

    ```python
    from sklearn.preprocessing import PolynomialFeatures

    poly = PolynomialFeatures(degree=2)
    data = [[1, 2], [3, 4], [5, 6]]
    poly_data = poly.fit_transform(data)
    print(poly_data)
    ```
* **So What**: Boosts performance on non-linear data without using complex models.

***

### **Feature Selection – RFE (Recursive Feature Elimination)**

* **What**: Iteratively removes less important features based on model weights.
* **Why**: Reduces overfitting and computation, improves generalization.
*   **How**:

    **Code**:

    ```python
    from sklearn.feature_selection import RFE
    from sklearn.linear_model import LinearRegression

    model = LinearRegression()
    selector = RFE(model, n_features_to_select=3)
    data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    selector = selector.fit(data, [1, 2, 3])
    print(selector.support_)
    ```
* **So what: It keeps** only relevant features, enhancing performance and reducing model complexity.

***

### **Feature Selection – Mutual Information**

* **What: Measures dependency between the feature and the target variable.**
* **Why**: Selects features with the highest information gain.
*   **How**:

    **Code**:

    ```python
    from sklearn.feature_selection import mutual_info_classif
    data = [[1, 2], [3, 4], [5, 6]]
    target = [0, 1, 0]
    mi = mutual_info_classif(data, target)
    print(mi)
    ```
* **So What**: Captures non-linear relationships; particularly useful for classification tasks.

***

### **Handling Missing Values**

* **What**: Replaces or removes NaNs in data.
* **Why**: Most models can’t handle missing data and will crash or perform poorly.
*   **How**:

    **Imputation**: Mean/Median/Mode **Deletion**: Drop rows or columns

    **Code**:

    ```python
    pythonCopy codefrom sklearn.impute import SimpleImputer
    data = [[1, 2], [3, None], [5, 6]]
    imputer = SimpleImputer(strategy='mean')
    imputed_data = imputer.fit_transform(data)
    print(imputed_data)
    ```
* **So What**: Maintains dataset usability, avoids information loss or bias.

***

### **Outlier Detection**

* **What**: Identifies and handles extreme values in data.
* **Why**: Outliers can skew model results, especially in regression and clustering.
*   **How**:\
    ![](<../../.gitbook/assets/image (17).png>)

    **Code (Z-Score)**:

    ```python
    pythonCopy codeimport numpy as np
    data = [1, 2, 3, 100, 5, 6]
    z_scores = (data - np.mean(data)) / np.std(data)
    print(z_scores)
    ```
* **So What**: Stabilizes training, improves generalization and robustness.

***

### **Feature Transformation (Log, Box-Cox)**

* **What**: Applies mathematical functions to stabilize variance and normalize distributions.
* **Why**: Some models assume normally distributed inputs (e.g., linear regression).
*   **How**:

    **Log Transformation**:

    ```python
    pythonCopy codeimport numpy as np
    data = [1, 2, 3, 10, 100]
    log_data = np.log1p(data)
    print(log_data)
    ```
* **So What**: Reduces skewness, improves model assumptions.

***

### **Interaction Features**

* **What**: Create new features based on relationships between two or more existing features.
* **Why**: Captures domain-specific relationships not visible individually.
*   **How**:

    **Example (BMI)**:

    ```python
    pythonCopy codedata = {'Weight': [70, 80], 'Height': [1.75, 1.80]}
    bmi = [w / (h ** 2) for w, h in zip(data['Weight'], data['Height'])]
    print(bmi)
    ```
* **So What**: Boosts accuracy by enriching the feature space with meaningful combinations.
