# Unsupervised Learning

### _K-Means_ Clustering&#x20;

> K-Means groups data into **k** clusters by minimizing intra-cluster distance and maximizing inter-cluster distance.
>
> **Algorithm Steps:**
>
> 1. Initialize `k` cluster centroids randomly.
> 2. Assign each point to the closest centroid.
> 3. Recalculate centroids based on assigned points.
> 4. Repeat Steps 2–3 until convergence (centroids don’t change).

**Assumptions / Conditions:**

| Assumption         | Description                                        |
| ------------------ | -------------------------------------------------- |
| Spherical Clusters | Clusters are assumed to be convex and isotropic    |
| Same Cluster Size  | Performs poorly when clusters vary in size/density |
| No Overlap         | Sensitive to overlapping clusters                  |
| k is known         | Needs `k` to be specified beforehand               |

**Common Issues:**

* Sensitive to initialization → Use **k-means++**
* Poor performance with non-spherical clusters
* Not robust to outliers

```python
from sklearn.cluster import KMeans
from sklearn.datasets import make_blobs
import matplotlib.pyplot as plt

X, _ = make_blobs(n_samples=300, centers=4, cluster_std=0.6)
model = KMeans(n_clusters=4, random_state=42)
y_kmeans = model.fit_predict(X)

plt.scatter(X[:, 0], X[:, 1], c=y_kmeans, cmap='viridis')
plt.scatter(model.cluster_centers_[:, 0], model.cluster_centers_[:, 1], s=200, c='red')
plt.title("KMeans Clustering")
plt.show()

```

### PCA (Principal Component Analysis)

> PCA reduces dimensionality by projecting data to a lower-dimensional space that captures the most variance.
>
> Steps:
>
> 1. Standardize data
> 2. Compute covariance matrix
> 3. Compute eigenvectors/eigenvalues
> 4. Select top-k eigenvectors → principal components

**Assumptions / Conditions:**

| Assumption                 | Description                                       |
| -------------------------- | ------------------------------------------------- |
| Linearity                  | Assumes linear relationships among features       |
| Large Variance = Important | Assumes high variance implies informative feature |
| Mean Centered              | Data should be zero-mean                          |
| Uncorrelated PCs           | PCs are orthogonal                                |

**Use Cases:**

* Noise reduction
* Visualization (2D/3D projection)
* Preprocessing before ML

```python
from sklearn.decomposition import PCA
from sklearn.datasets import load_iris
import matplotlib.pyplot as plt

X, _ = load_iris(return_X_y=True)
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X)

plt.scatter(X_pca[:, 0], X_pca[:, 1])
plt.title("PCA Projection")
plt.show()

```

### _t-SNE (t-distributed Stochastic Neighbor Embedding)_

> t-SNE projects high-dimensional data to a lower dimension (typically 2 or 3) using probability distributions to preserve **local structure**.
>
> **Properties:**
>
> * Nonlinear & stochastic
> * Good for visualizing clusters
> * Doesn’t preserve global distances
> * Sensitive to perplexity and initialization
>
> **Common Parameters:**
>
> * `perplexity` (related to number of neighbors)
> * `n_iter` (iterations)
> * `learning_rate`

```python
from sklearn.manifold import TSNE
from sklearn.datasets import load_digits
import matplotlib.pyplot as plt

X, y = load_digits(return_X_y=True)
tsne = TSNE(n_components=2, perplexity=30, random_state=42)
X_tsne = tsne.fit_transform(X)

plt.scatter(X_tsne[:, 0], X_tsne[:, 1], c=y, cmap='tab10')
plt.title("t-SNE Visualization")
plt.show()

```

### Anomaly Detection

> Detect data points that deviate significantly from the norm.
>
> 1. **Isolation Forest** – Randomly partitions data. Anomalies require fewer splits.
> 2. **One-Class SVM** – Learns a boundary to encapsulate the majority of the data.
> 3. **Elliptic Envelope** – Assumes data is Gaussian and finds an ellipse around it.
> 4. **Reconstruction Error (Autoencoders)** – Outliers have high reconstruction error.

**Assumptions / Conditions:**

| Assumption                 | Description                             |
| -------------------------- | --------------------------------------- |
| Majority of Data is Normal | Models assume anomalies are rare        |
| Features are informative   | Irrelevant features degrade performance |

```python
from sklearn.ensemble import IsolationForest
from sklearn.datasets import make_blobs
import matplotlib.pyplot as plt

X, _ = make_blobs(n_samples=300, centers=1, cluster_std=0.5)
X = np.concatenate([X, np.random.uniform(low=-4, high=4, size=(20, 2))])  # Add anomalies

model = IsolationForest(contamination=0.05)
y_pred = model.fit_predict(X)

plt.scatter(X[:, 0], X[:, 1], c=y_pred, cmap='coolwarm')
plt.title("Anomaly Detection using Isolation Forest")
plt.show()

```
