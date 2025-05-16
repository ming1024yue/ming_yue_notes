---
{"dg-publish":true,"permalink":"/algorithm/graph/prim/"}
---

```python
import heapq

def prim(graph, start):
    """
    Prim's Algorithm for Minimum Spanning Tree
    Time Complexity: O(E log V)
    Space Complexity: O(V + E)
    where V is number of vertices and E is number of edges
    """
    # Initialize
    mst = []
    visited = {start}
    edges = []
    
    # Add edges from start vertex
    for neighbor, weight in graph[start].items():
        heapq.heappush(edges, (weight, start, neighbor))
    
    # Process edges
    while edges and len(visited) < len(graph):
        weight, u, v = heapq.heappop(edges)
        if v not in visited:
            visited.add(v)
            mst.append((u, v, weight))
            
            # Add edges from new vertex
            for neighbor, weight in graph[v].items():
                if neighbor not in visited:
                    heapq.heappush(edges, (weight, v, neighbor))
    
    return mst

def prim_with_path(graph, start, end):
    """
    Prim's Algorithm with path reconstruction
    Time Complexity: O(E log V)
    Space Complexity: O(V + E)
    """
    mst = prim(graph, start)
    
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

def prim_components(graph):
    """
    Prim's Algorithm for finding connected components
    Time Complexity: O(E log V)
    Space Complexity: O(V + E)
    """
    visited = set()
    components = []
    
    for vertex in graph:
        if vertex not in visited:
            # Start new component
            component = []
            edges = []
            
            # Add edges from start vertex
            for neighbor, weight in graph[vertex].items():
                heapq.heappush(edges, (weight, vertex, neighbor))
            
            # Process edges
            while edges:
                weight, u, v = heapq.heappop(edges)
                if v not in visited:
                    visited.add(v)
                    component.append(v)
                    
                    # Add edges from new vertex
                    for neighbor, weight in graph[v].items():
                        if neighbor not in visited:
                            heapq.heappush(edges, (weight, v, neighbor))
            
            components.append(component)
    
    return components

def prim_total_weight(graph, start):
    """
    Prim's Algorithm for total weight calculation
    Time Complexity: O(E log V)
    Space Complexity: O(V + E)
    """
    mst = prim(graph, start)
    return sum(weight for _, _, weight in mst)

# Example usage
if __name__ == "__main__":
    # Test case
    graph = {
        'A': {'B': 4, 'C': 4},
        'B': {'A': 4, 'C': 2},
        'C': {'A': 4, 'B': 2, 'D': 3, 'E': 2, 'F': 4},
        'D': {'C': 3, 'F': 3},
        'E': {'C': 2, 'F': 3},
        'F': {'C': 4, 'D': 3, 'E': 3}
    }
    
    # Basic Prim
    mst = prim(graph, 'A')
    print("Minimum Spanning Tree:")
    for u, v, weight in mst:
        print(f"{u} - {v}: {weight}")
    
    # Path finding
    path = prim_with_path(graph, 'A', 'F')
    print("\nPath from A to F in MST:")
    for u, v in path:
        print(f"{u} -> {v}")
    
    # Connected components
    components = prim_components(graph)
    print("\nConnected Components:")
    for i, component in enumerate(components):
        print(f"Component {i+1}: {component}")
    
    # Total weight
    total_weight = prim_total_weight(graph, 'A')
    print(f"\nTotal Weight of MST: {total_weight}")
```

## Explanation
Prim's algorithm is a greedy algorithm that finds a minimum spanning tree for a connected weighted graph.

### How it works:
1. Start with any vertex
2. Grow MST by adding minimum weight edge
3. Add vertex to visited set
4. Add edges from new vertex to priority queue
5. Repeat until all vertices are visited

### Time Complexity:
- Best Case: O(E log V)
- Average Case: O(E log V)
- Worst Case: O(E log V)

### Space Complexity: O(V + E)

### Applications:
- Network design
- Circuit design
- Transportation networks
- Cluster analysis
- Image segmentation

### Advantages:
- Faster for dense graphs
- Simpler implementation
- Works for connected graphs
- Can handle negative weights
- Memory efficient

### Disadvantages:
- Slower for sparse graphs
- Requires priority queue
- Not suitable for all problems
- May be memory intensive
- Requires additional data structure 