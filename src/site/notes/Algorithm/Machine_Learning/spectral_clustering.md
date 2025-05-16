---
{"dg-publish":true,"permalink":"/algorithm/machine-learning/spectral-clustering/"}
---

```python
import numpy as np
from sklearn.metrics.pairwise import euclidean_distances
from scipy.sparse.linalg import eigsh
from scipy.sparse import csr_matrix
from sklearn.cluster import KMeans

class SpectralClustering:
    def __init__(self, n_clusters=2, affinity='rbf', gamma=1.0, n_neighbors=10):
        """
        Spectral Clustering
        Time Complexity: O(n^3) for eigen decomposition, O(n^2) for affinity matrix
        Space Complexity: O(n^2)
        where n is number of samples
        """
        self.n_clusters = n_clusters
        self.affinity = affinity
        self.gamma = gamma
        self.n_neighbors = n_neighbors
        self.labels = None
        self.affinity_matrix = None
        self.laplacian = None
    
    def fit(self, X):
        # Compute affinity matrix
        self.affinity_matrix = self._compute_affinity_matrix(X)
        
        # Compute Laplacian matrix
        self.laplacian = self._compute_laplacian(self.affinity_matrix)
        
        # Compute eigenvectors
        eigenvalues, eigenvectors = self._compute_eigenvectors(self.laplacian)
        
        # Perform k-means on eigenvectors
        kmeans = KMeans(n_clusters=self.n_clusters)
        self.labels = kmeans.fit_predict(eigenvectors)
    
    def _compute_affinity_matrix(self, X):
        if self.affinity == 'rbf':
            # Radial basis function kernel
            distances = euclidean_distances(X)
            return np.exp(-self.gamma * distances**2)
        elif self.affinity == 'nearest_neighbors':
            # k-nearest neighbors graph
            distances = euclidean_distances(X)
            n_samples = X.shape[0]
            affinity = np.zeros((n_samples, n_samples))
            
            for i in range(n_samples):
                # Find k-nearest neighbors
                indices = np.argsort(distances[i])[1:self.n_neighbors + 1]
                affinity[i, indices] = 1
                affinity[indices, i] = 1
            
            return affinity
        else:
            raise ValueError(f"Unknown affinity type: {self.affinity}")
    
    def _compute_laplacian(self, affinity_matrix):
        # Compute degree matrix
        degree_matrix = np.diag(np.sum(affinity_matrix, axis=1))
        
        # Compute Laplacian matrix
        laplacian = degree_matrix - affinity_matrix
        
        # Normalize Laplacian
        degree_sqrt = np.sqrt(degree_matrix)
        degree_sqrt_inv = np.linalg.inv(degree_sqrt)
        normalized_laplacian = degree_sqrt_inv @ laplacian @ degree_sqrt_inv
        
        return normalized_laplacian
    
    def _compute_eigenvectors(self, laplacian):
        # Convert to sparse matrix for efficiency
        sparse_laplacian = csr_matrix(laplacian)
        
        # Compute smallest k eigenvectors
        eigenvalues, eigenvectors = eigsh(sparse_laplacian, k=self.n_clusters, which='SM')
        
        # Sort eigenvectors by eigenvalues
        idx = np.argsort(eigenvalues)
        eigenvectors = eigenvectors[:, idx]
        
        return eigenvalues[idx], eigenvectors
    
    def predict(self, X):
        if self.labels is None:
            raise ValueError("Model not fitted yet")
        
        # Compute affinity matrix for new data
        affinity_matrix = self._compute_affinity_matrix(X)
        
        # Project new data onto eigenvectors
        projection = affinity_matrix @ self.laplacian
        
        # Use k-means to predict cluster assignments
        kmeans = KMeans(n_clusters=self.n_clusters)
        return kmeans.fit_predict(projection)

class SpectralClusteringWithScipy(SpectralClustering):
    def __init__(self, n_clusters=2, affinity='rbf', gamma=1.0, n_neighbors=10):
        super().__init__(n_clusters, affinity, gamma, n_neighbors)
    
    def _compute_eigenvectors(self, laplacian):
        # Use scipy's implementation for better performance
        from scipy.linalg import eigh
        
        # Compute eigenvectors
        eigenvalues, eigenvectors = eigh(laplacian)
        
        # Sort eigenvectors by eigenvalues
        idx = np.argsort(eigenvalues)
        eigenvectors = eigenvectors[:, idx]
        
        return eigenvalues[idx], eigenvectors

# Example usage
if __name__ == "__main__":
    # Generate sample data
    np.random.seed(42)
    n_samples = 200
    X = np.random.randn(n_samples, 2)
    
    # Add some clusters
    X[:50] += 5
    X[50:100] -= 5
    X[100:150] += np.array([5, -5])
    
    # Basic Spectral Clustering
    spectral = SpectralClustering(n_clusters=3, affinity='rbf', gamma=0.1)
    spectral.fit(X)
    print("Number of clusters:", len(np.unique(spectral.labels)))
    
    # Spectral Clustering with Scipy
    spectral_scipy = SpectralClusteringWithScipy(n_clusters=3, affinity='rbf', gamma=0.1)
    spectral_scipy.fit(X)
    print("\nNumber of clusters (with Scipy):", len(np.unique(spectral_scipy.labels)))
```

## Explanation
Spectral Clustering is a technique that uses the eigenvalues of a similarity matrix to perform dimensionality reduction before clustering in fewer dimensions.

### How it works:
1. Construct similarity graph from data
2. Compute Laplacian matrix
3. Compute eigenvalues and eigenvectors
4. Perform k-means on eigenvectors
5. Assign cluster labels

### Time Complexity:
- Best Case: O(n^2)
- Average Case: O(n^3)
- Worst Case: O(n^3)

### Space Complexity: O(n^2)

### Applications:
- Image segmentation
- Social network analysis
- Community detection
- Document clustering
- Bioinformatics

### Advantages:
- Can find non-convex clusters
- Works well with sparse data
- Robust to noise
- Can handle complex shapes
- Good for graph-based data

### Disadvantages:
- Computationally expensive
- Memory intensive
- Sensitive to parameters
- Not suitable for large datasets
- Requires careful parameter tuning 