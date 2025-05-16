---
{"dg-publish":true,"permalink":"/algorithm/graph/bellman-ford/"}
---



# Bellman-Ford Algorithm

```python
import numpy as np

class BellmanFord:
    def __init__(self):
        """
        Bellman-Ford Algorithm
        Time Complexity: O(V * E)
        Space Complexity: O(V)
        where V is number of vertices and E is number of edges
        """
        self.distances = None
        self.predecessors = None
        self.has_negative_cycle = False
    
    def find_shortest_paths(self, graph, start):
        """
        Find shortest paths from start vertex to all other vertices
        Returns True if no negative cycles, False otherwise
        """
        n_vertices = len(graph)
        self.distances = np.full(n_vertices, np.inf)
        self.predecessors = np.full(n_vertices, -1)
        self.distances[start] = 0
        
        # Relax all edges V-1 times
        for _ in range(n_vertices - 1):
            for u in range(n_vertices):
                for v, weight in graph[u]:
                    if self.distances[u] + weight < self.distances[v]:
                        self.distances[v] = self.distances[u] + weight
                        self.predecessors[v] = u
        
        # Check for negative cycles
        for u in range(n_vertices):
            for v, weight in graph[u]:
                if self.distances[u] + weight < self.distances[v]:
                    self.has_negative_cycle = True
                    return False
        
        return True
    
    def get_path(self, end):
        """Reconstruct path from start to end vertex"""
        if self.predecessors is None:
            raise ValueError("Algorithm not run yet")
        
        path = []
        current = end
        
        while current != -1:
            path.append(current)
            current = self.predecessors[current]
        
        return list(reversed(path))
    
    def get_all_paths(self):
        """Get paths from start to all vertices"""
        if self.predecessors is None:
            raise ValueError("Algorithm not run yet")
        
        paths = []
        for end in range(len(self.predecessors)):
            paths.append(self.get_path(end))
        return paths

class BellmanFordWithEarlyTermination(BellmanFord):
    def __init__(self):
        super().__init__()
    
    def find_shortest_paths(self, graph, start):
        n_vertices = len(graph)
        self.distances = np.full(n_vertices, np.inf)
        self.predecessors = np.full(n_vertices, -1)
        self.distances[start] = 0
        
        # Relax all edges V-1 times
        for i in range(n_vertices - 1):
            updated = False
            for u in range(n_vertices):
                for v, weight in graph[u]:
                    if self.distances[u] + weight < self.distances[v]:
                        self.distances[v] = self.distances[u] + weight
                        self.predecessors[v] = u
                        updated = True
            
            # Early termination if no updates
            if not updated:
                break
        
        # Check for negative cycles
        for u in range(n_vertices):
            for v, weight in graph[u]:
                if self.distances[u] + weight < self.distances[v]:
                    self.has_negative_cycle = True
                    return False
        
        return True

# Example usage
if __name__ == "__main__":
    # Create sample graph with negative weights
    graph = [
        [(1, 4), (2, 5)],  # 0 -> 1 (4), 0 -> 2 (5)
        [(2, -3)],         # 1 -> 2 (-3)
        [(3, 2)],          # 2 -> 3 (2)
        [(1, -1)]          # 3 -> 1 (-1)
    ]
    
    # Basic Bellman-Ford
    bf = BellmanFord()
    if bf.find_shortest_paths(graph, 0):
        print("Shortest distances:", bf.distances)
        print("Shortest paths:")
        for i, path in enumerate(bf.get_all_paths()):
            print(f"0 -> {i}: {path}")
    else:
        print("Graph contains negative cycle")
    
    # Bellman-Ford with early termination
    bf_early = BellmanFordWithEarlyTermination()
    if bf_early.find_shortest_paths(graph, 0):
        print("\nShortest distances (with early termination):", bf_early.distances)
        print("Shortest paths (with early termination):")
        for i, path in enumerate(bf_early.get_all_paths()):
            print(f"0 -> {i}: {path}")
    else:
        print("\nGraph contains negative cycle")
```

## Explanation
Bellman-Ford algorithm is used to find shortest paths from a single source vertex to all other vertices in a weighted graph, even when the graph contains negative weight edges.

### How it works:
1. Initialize distances to infinity and predecessors to -1
2. Set distance to start vertex as 0
3. Relax all edges V-1 times
4. Check for negative cycles
5. Reconstruct paths using predecessors

### Time Complexity:
- Best Case: O(V * E)
- Average Case: O(V * E)
- Worst Case: O(V * E)

### Space Complexity: O(V)

### Applications:
- Network routing
- Currency arbitrage
- Traffic engineering
- Game theory
- Resource allocation

### Advantages:
- Can handle negative weights
- Detects negative cycles
- Works for directed graphs
- Simple implementation
- Robust to edge order

### Disadvantages:
- Slower than Dijkstra's
- Not suitable for large graphs
- Memory intensive
- May need multiple passes
- Sensitive to edge order
