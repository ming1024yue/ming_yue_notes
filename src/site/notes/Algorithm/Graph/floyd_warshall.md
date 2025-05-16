---
{"dg-publish":true,"permalink":"/algorithm/graph/floyd-warshall/"}
---

```python
import numpy as np

class FloydWarshall:
    def __init__(self):
        """
        Floyd-Warshall Algorithm
        Time Complexity: O(V^3)
        Space Complexity: O(V^2)
        where V is number of vertices
        """
        self.distances = None
        self.next_nodes = None
        self.has_negative_cycle = False
    
    def find_all_pairs_shortest_paths(self, graph):
        """
        Find shortest paths between all pairs of vertices
        Returns True if no negative cycles, False otherwise
        """
        n_vertices = len(graph)
        
        # Initialize distance and next matrices
        self.distances = np.full((n_vertices, n_vertices), np.inf)
        self.next_nodes = np.full((n_vertices, n_vertices), -1)
        
        # Set initial distances
        for u in range(n_vertices):
            for v, weight in graph[u]:
                self.distances[u][v] = weight
                self.next_nodes[u][v] = v
        
        # Set diagonal to 0
        np.fill_diagonal(self.distances, 0)
        
        # Main algorithm
        for k in range(n_vertices):
            for i in range(n_vertices):
                for j in range(n_vertices):
                    if self.distances[i][k] + self.distances[k][j] < self.distances[i][j]:
                        self.distances[i][j] = self.distances[i][k] + self.distances[k][j]
                        self.next_nodes[i][j] = self.next_nodes[i][k]
        
        # Check for negative cycles
        for i in range(n_vertices):
            if self.distances[i][i] < 0:
                self.has_negative_cycle = True
                return False
        
        return True
    
    def get_path(self, start, end):
        """Reconstruct path from start to end vertex"""
        if self.next_nodes is None:
            raise ValueError("Algorithm not run yet")
        
        if self.next_nodes[start][end] == -1:
            return []
        
        path = [start]
        current = start
        
        while current != end:
            current = self.next_nodes[current][end]
            path.append(current)
        
        return path
    
    def get_all_paths(self):
        """Get paths between all pairs of vertices"""
        if self.next_nodes is None:
            raise ValueError("Algorithm not run yet")
        
        paths = []
        n_vertices = len(self.next_nodes)
        
        for i in range(n_vertices):
            for j in range(n_vertices):
                if i != j:
                    paths.append((i, j, self.get_path(i, j)))
        
        return paths

class FloydWarshallWithPathReconstruction(FloydWarshall):
    def __init__(self):
        super().__init__()
        self.paths = None
    
    def find_all_pairs_shortest_paths(self, graph):
        if not super().find_all_pairs_shortest_paths(graph):
            return False
        
        # Reconstruct all paths
        n_vertices = len(graph)
        self.paths = [[[] for _ in range(n_vertices)] for _ in range(n_vertices)]
        
        for i in range(n_vertices):
            for j in range(n_vertices):
                if i != j and self.next_nodes[i][j] != -1:
                    self.paths[i][j] = self.get_path(i, j)
        
        return True
    
    def get_path(self, start, end):
        if self.paths is None:
            raise ValueError("Algorithm not run yet")
        return self.paths[start][end]

# Example usage
if __name__ == "__main__":
    # Create sample graph
    graph = [
        [(1, 3), (2, 6)],  # 0 -> 1 (3), 0 -> 2 (6)
        [(2, 2)],          # 1 -> 2 (2)
        [(0, 1), (1, 4)]   # 2 -> 0 (1), 2 -> 1 (4)
    ]
    
    # Basic Floyd-Warshall
    fw = FloydWarshall()
    if fw.find_all_pairs_shortest_paths(graph):
        print("All-pairs shortest distances:")
        print(fw.distances)
        print("\nShortest paths:")
        for i, j, path in fw.get_all_paths():
            print(f"{i} -> {j}: {path}")
    else:
        print("Graph contains negative cycle")
    
    # Floyd-Warshall with path reconstruction
    fw_path = FloydWarshallWithPathReconstruction()
    if fw_path.find_all_pairs_shortest_paths(graph):
        print("\nAll-pairs shortest distances (with path reconstruction):")
        print(fw_path.distances)
        print("\nShortest paths (with path reconstruction):")
        for i, j, path in fw_path.get_all_paths():
            print(f"{i} -> {j}: {path}")
    else:
        print("\nGraph contains negative cycle")
```

## Explanation
Floyd-Warshall algorithm is used to find shortest paths between all pairs of vertices in a weighted graph, even when the graph contains negative weight edges.

### How it works:
1. Initialize distance and next matrices
2. Set initial distances from direct edges
3. For each intermediate vertex k:
   - For each pair of vertices i, j:
     - Update distance if path through k is shorter
4. Check for negative cycles
5. Reconstruct paths using next matrix

### Time Complexity:
- Best Case: O(V^3)
- Average Case: O(V^3)
- Worst Case: O(V^3)

### Space Complexity: O(V^2)

### Applications:
- Network routing
- Traffic engineering
- Game theory
- Resource allocation
- Circuit design

### Advantages:
- Finds all-pairs shortest paths
- Can handle negative weights
- Detects negative cycles
- Works for directed graphs
- Simple implementation

### Disadvantages:
- High time complexity
- Memory intensive
- Not suitable for large graphs
- May need to store all paths
- Sensitive to edge weights
