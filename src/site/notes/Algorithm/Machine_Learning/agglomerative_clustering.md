---
{"dg-publish":true,"permalink":"/algorithm/machine-learning/agglomerative-clustering/"}
---

```python
import numpy as np
from sklearn.metrics.pairwise import euclidean_distances
from scipy.cluster.hierarchy import linkage, dendrogram
import matplotlib.pyplot as plt

class AgglomerativeClustering:
    def __init__(self, n_clusters=2, linkage='ward'):
        """
        Agglomerative Hierarchical Clustering
        Time Complexity: O(n^3) in worst case, O(n^2 log n) with efficient implementation
        Space Complexity: O(n^2)
        where n is number of samples
        """
        self.n_clusters = n_clusters
        self.linkage = linkage
        self.labels = None
        self.children = None
        self.distances = None
    
    def fit(self, X):
        n_samples = X.shape[0]
        self.labels = np.arange(n_samples)
        self.children = []
        self.distances = []
        
        # Initialize distance matrix
        distances = euclidean_distances(X)
        np.fill_diagonal(distances, np.inf)
        
        # Initialize clusters
        clusters = [[i] for i in range(n_samples)]
        
        while len(clusters) > self.n_clusters:
            # Find closest clusters
            min_dist = np.inf
            min_i, min_j = -1, -1
            
            for i in range(len(clusters)):
                for j in range(i + 1, len(clusters)):
                    dist = self._compute_distance(clusters[i], clusters[j], distances)
                    if dist < min_dist:
                        min_dist = dist
                        min_i, min_j = i, j
            
            # Merge clusters
            new_cluster = clusters[min_i] + clusters[min_j]
            clusters.pop(min_j)
            clusters[min_i] = new_cluster
            
            # Update labels
            for idx in new_cluster:
                self.labels[idx] = min_i
            
            # Store merge information
            self.children.append((min_i, min_j))
            self.distances.append(min_dist)
    
    def _compute_distance(self, cluster1, cluster2, distances):
        if self.linkage == 'single':
            return np.min(distances[np.ix_(cluster1, cluster2)])
        elif self.linkage == 'complete':
            return np.max(distances[np.ix_(cluster1, cluster2)])
        elif self.linkage == 'average':
            return np.mean(distances[np.ix_(cluster1, cluster2)])
        elif self.linkage == 'ward':
            # Ward's method: minimize variance
            n1, n2 = len(cluster1), len(cluster2)
            return (n1 * n2) / (n1 + n2) * np.mean(distances[np.ix_(cluster1, cluster2)])
        else:
            raise ValueError(f"Unknown linkage type: {self.linkage}")
    
    def plot_dendrogram(self):
        if self.children is None or self.distances is None:
            raise ValueError("Model not fitted yet")
        
        # Create linkage matrix
        Z = np.column_stack([self.children, self.distances, np.zeros(len(self.distances))])
        
        # Plot dendrogram
        plt.figure(figsize=(10, 6))
        dendrogram(Z)
        plt.title('Hierarchical Clustering Dendrogram')
        plt.xlabel('Sample index')
        plt.ylabel('Distance')
        plt.show()

class AgglomerativeClusteringWithScipy(AgglomerativeClustering):
    def __init__(self, n_clusters=2, linkage='ward'):
        super().__init__(n_clusters, linkage)
    
    def fit(self, X):
        # Use scipy's implementation for better performance
        Z = linkage(X, method=self.linkage)
        
        # Extract cluster assignments
        from scipy.cluster.hierarchy import fcluster
        self.labels = fcluster(Z, t=self.n_clusters, criterion='maxclust') - 1
        
        # Store linkage information
        self.children = Z[:, :2].astype(int)
        self.distances = Z[:, 2]

# Example usage
if __name__ == "__main__":
    # Generate sample data
    np.random.seed(42)
    n_samples = 100
    X = np.random.randn(n_samples, 2)
    
    # Add some clusters
    X[:30] += 5
    X[30:60] -= 5
    X[60:90] += np.array([5, -5])
    
    # Basic Agglomerative Clustering
    agg = AgglomerativeClustering(n_clusters=3, linkage='ward')
    agg.fit(X)
    print("Number of clusters:", len(np.unique(agg.labels)))
    agg.plot_dendrogram()
    
    # Agglomerative Clustering with Scipy
    agg_scipy = AgglomerativeClusteringWithScipy(n_clusters=3, linkage='ward')
    agg_scipy.fit(X)
    print("\nNumber of clusters (with Scipy):", len(np.unique(agg_scipy.labels)))
    agg_scipy.plot_dendrogram()
```

## Explanation
Agglomerative Hierarchical Clustering is a bottom-up clustering approach where each observation starts in its own cluster, and pairs of clusters are merged as one moves up the hierarchy.

### How it works:
1. Start with each point as a separate cluster
2. Find the closest pair of clusters
3. Merge them into a new cluster
4. Update distances between clusters
5. Repeat until desired number of clusters is reached

### Time Complexity:
- Best Case: O(n^2 log n)
- Average Case: O(n^2 log n)
- Worst Case: O(n^3)

### Space Complexity: O(n^2)

### Applications:
- Gene expression analysis
- Document clustering
- Image segmentation
- Market segmentation
- Social network analysis

### Advantages:
- Produces hierarchical structure
- No need to specify number of clusters
- Works well with different distance metrics
- Provides dendrogram visualization
- Can handle non-convex clusters

### Disadvantages:
- Computationally expensive
- Memory intensive
- Sensitive to noise
- Not suitable for large datasets
- Results depend on linkage method 