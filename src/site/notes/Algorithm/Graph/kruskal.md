---
{"dg-publish":true,"permalink":"/algorithm/graph/kruskal/"}
---

```python
class DisjointSet:
    def __init__(self, vertices):
        self.parent = {v: v for v in vertices}
        self.rank = {v: 0 for v in vertices}
    
    def find(self, item):
        if self.parent[item] != item:
            self.parent[item] = self.find(self.parent[item])
        return self.parent[item]
    
    def union(self, set1, set2):
        root1 = self.find(set1)
        root2 = self.find(set2)
        
        if root1 != root2:
            if self.rank[root1] > self.rank[root2]:
                self.parent[root2] = root1
            else:
                self.parent[root1] = root2
                if self.rank[root1] == self.rank[root2]:
                    self.rank[root2] += 1

def kruskal(graph):
    """
    Kruskal's Algorithm for Minimum Spanning Tree
    Time Complexity: O(E log E)
    Space Complexity: O(V + E)
    where V is number of vertices and E is number of edges
    """
    # Get all vertices
    vertices = set()
    for u, v, _ in graph:
        vertices.add(u)
        vertices.add(v)
    
    # Initialize disjoint set
    ds = DisjointSet(vertices)
    
    # Sort edges by weight
    sorted_edges = sorted(graph, key=lambda x: x[2])
    
    # Initialize MST
    mst = []
    total_weight = 0
    
    # Process edges
    for u, v, weight in sorted_edges:
        if ds.find(u) != ds.find(v):
            ds.union(u, v)
            mst.append((u, v, weight))
            total_weight += weight
    
    return mst, total_weight

def kruskal_with_path(graph, start, end):
    """
    Kruskal's Algorithm with path reconstruction
    Time Complexity: O(E log E)
    Space Complexity: O(V + E)
    """
    mst, _ = kruskal(graph)
    
    # Build adjacency list from MST
    adj = {}
    for u, v, _ in mst:
        if u not in adj:
            adj[u] = []
        if v not in adj:
            adj[v] = []
        adj[u].append(v)
        adj[v].append(u)
    
    # Find path using DFS
    visited = set()
    path = []
    
    def dfs(current, target):
        if current == target:
            return True
        visited.add(current)
        for neighbor in adj.get(current, []):
            if neighbor not in visited:
                path.append((current, neighbor))
                if dfs(neighbor, target):
                    return True
                path.pop()
        return False
    
    if dfs(start, end):
        return path
    return None

def kruskal_components(graph):
    """
    Kruskal's Algorithm for finding connected components
    Time Complexity: O(E log E)
    Space Complexity: O(V + E)
    """
    # Get all vertices
    vertices = set()
    for u, v, _ in graph:
        vertices.add(u)
        vertices.add(v)
    
    # Initialize disjoint set
    ds = DisjointSet(vertices)
    
    # Process edges
    for u, v, _ in graph:
        ds.union(u, v)
    
    # Find components
    components = {}
    for v in vertices:
        root = ds.find(v)
        if root not in components:
            components[root] = []
        components[root].append(v)
    
    return list(components.values())

# Example usage
if __name__ == "__main__":
    # Test case
    graph = [
        ('A', 'B', 4),
        ('A', 'C', 4),
        ('B', 'C', 2),
        ('C', 'D', 3),
        ('C', 'E', 2),
        ('C', 'F', 4),
        ('D', 'F', 3),
        ('E', 'F', 3)
    ]
    
    # Basic Kruskal
    mst, total_weight = kruskal(graph)
    print("Minimum Spanning Tree:")
    for u, v, weight in mst:
        print(f"{u} - {v}: {weight}")
    print(f"Total Weight: {total_weight}")
    
    # Path finding
    path = kruskal_with_path(graph, 'A', 'F')
    print("\nPath from A to F in MST:")
    for u, v in path:
        print(f"{u} -> {v}")
    
    # Connected components
    components = kruskal_components(graph)
    print("\nConnected Components:")
    for i, component in enumerate(components):
        print(f"Component {i+1}: {component}")
```

## Explanation
Kruskal's algorithm is a greedy algorithm that finds a minimum spanning tree for a connected weighted graph.

### How it works:
1. Sort all edges in non-decreasing order of weight
2. Initialize disjoint set data structure
3. Process edges in sorted order:
   - If edge connects two different components, add it to MST
   - Use union-find to track components
4. Stop when MST has V-1 edges

### Time Complexity:
- Best Case: O(E log E)
- Average Case: O(E log E)
- Worst Case: O(E log E)

### Space Complexity: O(V + E)

### Applications:
- Network design
- Circuit design
- Transportation networks
- Cluster analysis
- Image segmentation

### Advantages:
- Always finds MST
- Works for disconnected graphs
- Easy to implement
- Can handle negative weights
- Parallelizable

### Disadvantages:
- Slower than Prim's for dense graphs
- Requires sorting
- Not suitable for all problems
- May be memory intensive
- Requires additional data structure 