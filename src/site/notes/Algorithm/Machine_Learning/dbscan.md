---
{"dg-publish":true,"permalink":"/algorithm/machine-learning/dbscan/"}
---

```python
import numpy as np
from sklearn.metrics.pairwise import euclidean_distances

class DBSCAN:
    def __init__(self, eps=0.5, min_samples=5):
        """
        DBSCAN Clustering
        Time Complexity: O(n^2) in worst case, O(n log n) with spatial indexing
        Space Complexity: O(n)
        where n is number of samples
        """
        self.eps = eps
        self.min_samples = min_samples
        self.labels = None
        self.core_samples = None
    
    def fit(self, X):
        n_samples = X.shape[0]
        self.labels = np.full(n_samples, -1)
        self.core_samples = np.zeros(n_samples, dtype=bool)
        cluster_id = 0
        
        # Compute pairwise distances
        distances = euclidean_distances(X)
        
        for i in range(n_samples):
            if self.labels[i] != -1:
                continue
            
            # Find neighbors
            neighbors = self._find_neighbors(distances, i)
            
            if len(neighbors) < self.min_samples:
                self.labels[i] = -1  # Mark as noise
            else:
                self.core_samples[i] = True
                self._expand_cluster(X, distances, i, neighbors, cluster_id)
                cluster_id += 1
    
    def _find_neighbors(self, distances, i):
        return np.where(distances[i] <= self.eps)[0]
    
    def _expand_cluster(self, X, distances, i, neighbors, cluster_id):
        self.labels[i] = cluster_id
        i = 0
        while i < len(neighbors):
            neighbor = neighbors[i]
            
            if self.labels[neighbor] == -1:
                self.labels[neighbor] = cluster_id
            
            elif self.labels[neighbor] == -1:
                self.labels[neighbor] = cluster_id
                new_neighbors = self._find_neighbors(distances, neighbor)
                
                if len(new_neighbors) >= self.min_samples:
                    self.core_samples[neighbor] = True
                    neighbors = np.concatenate([neighbors, new_neighbors])
            
            i += 1
    
    def predict(self, X):
        if self.labels is None:
            raise ValueError("Model not fitted yet")
        
        distances = euclidean_distances(X)
        predictions = np.zeros(X.shape[0])
        
        for i in range(X.shape[0]):
            # Find nearest core sample
            min_dist = float('inf')
            nearest_core = -1
            
            for j in range(len(self.core_samples)):
                if self.core_samples[j]:
                    dist = distances[i, j]
                    if dist < min_dist:
                        min_dist = dist
                        nearest_core = j
            
            if min_dist <= self.eps:
                predictions[i] = self.labels[nearest_core]
            else:
                predictions[i] = -1
        
        return predictions

class DBSCANWithKDTree(DBSCAN):
    def __init__(self, eps=0.5, min_samples=5):
        super().__init__(eps, min_samples)
    
    def fit(self, X):
        from sklearn.neighbors import KDTree
        n_samples = X.shape[0]
        self.labels = np.full(n_samples, -1)
        self.core_samples = np.zeros(n_samples, dtype=bool)
        cluster_id = 0
        
        # Build KDTree for efficient neighbor search
        tree = KDTree(X)
        
        for i in range(n_samples):
            if self.labels[i] != -1:
                continue
            
            # Find neighbors using KDTree
            neighbors = tree.query_radius(X[i:i+1], r=self.eps)[0]
            
            if len(neighbors) < self.min_samples:
                self.labels[i] = -1  # Mark as noise
            else:
                self.core_samples[i] = True
                self._expand_cluster(X, tree, i, neighbors, cluster_id)
                cluster_id += 1
    
    def _expand_cluster(self, X, tree, i, neighbors, cluster_id):
        self.labels[i] = cluster_id
        i = 0
        while i < len(neighbors):
            neighbor = neighbors[i]
            
            if self.labels[neighbor] == -1:
                self.labels[neighbor] = cluster_id
            
            elif self.labels[neighbor] == -1:
                self.labels[neighbor] = cluster_id
                new_neighbors = tree.query_radius(X[neighbor:neighbor+1], r=self.eps)[0]
                
                if len(new_neighbors) >= self.min_samples:
                    self.core_samples[neighbor] = True
                    neighbors = np.concatenate([neighbors, new_neighbors])
            
            i += 1

# Example usage
if __name__ == "__main__":
    # Generate sample data
    np.random.seed(42)
    n_samples = 300
    X = np.random.randn(n_samples, 2)
    
    # Add some clusters
    X[:100] += 5
    X[100:200] -= 5
    X[200:] += np.array([5, -5])
    
    # Add some noise
    X = np.vstack([X, np.random.randn(50, 2) * 10])
    
    # Basic DBSCAN
    dbscan = DBSCAN(eps=1.0, min_samples=5)
    dbscan.fit(X)
    print("Number of clusters:", len(np.unique(dbscan.labels)) - 1)
    print("Number of noise points:", np.sum(dbscan.labels == -1))
    
    # DBSCAN with KDTree
    dbscan_kd = DBSCANWithKDTree(eps=1.0, min_samples=5)
    dbscan_kd.fit(X)
    print("\nNumber of clusters (with KDTree):", len(np.unique(dbscan_kd.labels)) - 1)
    print("Number of noise points (with KDTree):", np.sum(dbscan_kd.labels == -1))
```

## Explanation
DBSCAN is a density-based clustering algorithm that groups together points that are closely packed together and marks points that lie alone in low-density regions as outliers.

### How it works:
1. For each point, find its neighbors within eps distance
2. If a point has at least min_samples neighbors, it's a core point
3. Expand cluster by adding reachable points
4. Points not reachable from any core point are noise

### Time Complexity:
- Best Case: O(n log n) with spatial indexing
- Average Case: O(n log n) with spatial indexing
- Worst Case: O(n^2)

### Space Complexity: O(n)

### Applications:
- Anomaly detection
- Image segmentation
- Geographic data analysis
- Customer segmentation
- Document clustering

### Advantages:
- Can find clusters of arbitrary shape
- Robust to noise
- No need to specify number of clusters
- Works well with spatial data
- Handles outliers effectively

### Disadvantages:
- Sensitive to parameters
- Not suitable for high-dimensional data
- Performance depends on distance metric
- Memory intensive for large datasets
- May struggle with varying densities 