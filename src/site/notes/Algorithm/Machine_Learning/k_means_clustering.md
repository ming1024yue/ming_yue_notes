---
{"dg-publish":true,"permalink":"/algorithm/machine-learning/k-means-clustering/"}
---

```python
import numpy as np
from sklearn.metrics.pairwise import euclidean_distances

class KMeans:
    def __init__(self, n_clusters=3, max_iter=300, tol=1e-4):
        """
        K-means Clustering
        Time Complexity: O(n * k * d * i)
        Space Complexity: O(n * d + k * d)
        where n is number of samples, k is number of clusters,
        d is number of features, and i is number of iterations
        """
        self.n_clusters = n_clusters
        self.max_iter = max_iter
        self.tol = tol
        self.centroids = None
        self.labels = None
    
    def fit(self, X):
        # Initialize centroids randomly
        n_samples = X.shape[0]
        random_indices = np.random.choice(n_samples, self.n_clusters, replace=False)
        self.centroids = X[random_indices]
        
        for _ in range(self.max_iter):
            # Assign labels
            distances = euclidean_distances(X, self.centroids)
            self.labels = np.argmin(distances, axis=1)
            
            # Update centroids
            new_centroids = np.zeros_like(self.centroids)
            for i in range(self.n_clusters):
                cluster_points = X[self.labels == i]
                if len(cluster_points) > 0:
                    new_centroids[i] = np.mean(cluster_points, axis=0)
            
            # Check convergence
            if np.linalg.norm(new_centroids - self.centroids) < self.tol:
                break
            
            self.centroids = new_centroids
    
    def predict(self, X):
        distances = euclidean_distances(X, self.centroids)
        return np.argmin(distances, axis=1)
    
    def score(self, X):
        distances = euclidean_distances(X, self.centroids)
        return -np.sum(np.min(distances, axis=1))

class KMeansPlusPlus(KMeans):
    def __init__(self, n_clusters=3, max_iter=300, tol=1e-4):
        super().__init__(n_clusters, max_iter, tol)
    
    def _initialize_centroids(self, X):
        n_samples = X.shape[0]
        centroids = np.zeros((self.n_clusters, X.shape[1]))
        
        # Choose first centroid randomly
        centroids[0] = X[np.random.randint(n_samples)]
        
        # Choose remaining centroids
        for i in range(1, self.n_clusters):
            distances = euclidean_distances(X, centroids[:i])
            min_distances = np.min(distances, axis=1)
            probabilities = min_distances / np.sum(min_distances)
            centroids[i] = X[np.random.choice(n_samples, p=probabilities)]
        
        return centroids
    
    def fit(self, X):
        self.centroids = self._initialize_centroids(X)
        super().fit(X)

class KMeansElbow:
    def __init__(self, max_clusters=10):
        self.max_clusters = max_clusters
        self.scores = None
    
    def fit(self, X):
        self.scores = []
        for k in range(1, self.max_clusters + 1):
            kmeans = KMeans(n_clusters=k)
            kmeans.fit(X)
            self.scores.append(kmeans.score(X))
    
    def plot_elbow(self):
        import matplotlib.pyplot as plt
        plt.plot(range(1, self.max_clusters + 1), self.scores, 'bx-')
        plt.xlabel('Number of clusters')
        plt.ylabel('Score')
        plt.title('Elbow Method')
        plt.show()

# Example usage
if __name__ == "__main__":
    # Generate sample data
    np.random.seed(42)
    X = np.random.randn(300, 2)
    X[:100] += 5
    X[100:200] -= 5
    X[200:] += np.array([5, -5])
    
    # Basic K-means
    kmeans = KMeans(n_clusters=3)
    kmeans.fit(X)
    print("Basic K-means Centroids:")
    print(kmeans.centroids)
    
    # K-means++
    kmeans_plus = KMeansPlusPlus(n_clusters=3)
    kmeans_plus.fit(X)
    print("\nK-means++ Centroids:")
    print(kmeans_plus.centroids)
    
    # Elbow method
    elbow = KMeansElbow(max_clusters=10)
    elbow.fit(X)
    elbow.plot_elbow()
```

## Explanation
K-means Clustering is an unsupervised learning algorithm that partitions data into k clusters.

### How it works:
1. Initialize k centroids randomly
2. Assign each point to nearest centroid
3. Update centroids to mean of points in cluster
4. Repeat until convergence

### Time Complexity:
- Best Case: O(n * k * d * i)
- Average Case: O(n * k * d * i)
- Worst Case: O(n * k * d * i)

### Space Complexity: O(n * d + k * d)

### Applications:
- Customer segmentation
- Image compression
- Document clustering
- Anomaly detection
- Market research

### Advantages:
- Simple implementation
- Fast convergence
- Works well for spherical clusters
- Easy to interpret
- Scalable to large datasets

### Disadvantages:
- Sensitive to initialization
- Requires k to be specified
- Assumes spherical clusters
- Not suitable for all shapes
- May converge to local optima 