---
{"dg-publish":true,"permalink":"/algorithm/graph/breadth-first-search/"}
---

# Breadth-First Search (BFS)

```python
from collections import deque

def bfs(graph, start):
    """
    Breadth-First Search
    Time Complexity: O(V + E)
    Space Complexity: O(V)
    where V is number of vertices and E is number of edges
    """
    # Initialize
    visited = {start}
    queue = deque([start])
    parent = {start: None}
    distance = {start: 0}
    
    # BFS loop
    while queue:
        vertex = queue.popleft()
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
                parent[neighbor] = vertex
                distance[neighbor] = distance[vertex] + 1
    
    return parent, distance

def bfs_path(graph, start, end):
    """
    BFS with path reconstruction
    Time Complexity: O(V + E)
    Space Complexity: O(V)
    """
    parent, _ = bfs(graph, start)
    
    # Reconstruct path
    if end not in parent:
        return None
    
    path = []
    current = end
    while current is not None:
        path.append(current)
        current = parent[current]
    
    return path[::-1]

def bfs_components(graph):
    """
    BFS for finding connected components
    Time Complexity: O(V + E)
    Space Complexity: O(V)
    """
    visited = set()
    components = []
    
    for vertex in graph:
        if vertex not in visited:
            # Start new component
            component = []
            queue = deque([vertex])
            visited.add(vertex)
            
            # BFS for this component
            while queue:
                v = queue.popleft()
                component.append(v)
                for neighbor in graph[v]:
                    if neighbor not in visited:
                        visited.add(neighbor)
                        queue.append(neighbor)
            
            components.append(component)
    
    return components

def bfs_levels(graph, start):
    """
    BFS with level information
    Time Complexity: O(V + E)
    Space Complexity: O(V)
    """
    visited = {start}
    queue = deque([start])
    levels = {start: 0}
    
    while queue:
        vertex = queue.popleft()
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
                levels[neighbor] = levels[vertex] + 1
    
    return levels

# Example usage
if __name__ == "__main__":
    # Test case
    graph = {
        'A': ['B', 'C'],
        'B': ['A', 'D', 'E'],
        'C': ['A', 'F'],
        'D': ['B'],
        'E': ['B', 'F'],
        'F': ['C', 'E']
    }
    
    # Basic BFS
    parent, distance = bfs(graph, 'A')
    print("BFS Parent and Distance:")
    for vertex in graph:
        print(f"Vertex: {vertex}, Parent: {parent[vertex]}, Distance: {distance[vertex]}")
    
    # Path finding
    path = bfs_path(graph, 'A', 'F')
    print("\nPath from A to F:", path)
    
    # Connected components
    components = bfs_components(graph)
    print("\nConnected Components:", components)
    
    # Level information
    levels = bfs_levels(graph, 'A')
    print("\nLevels from A:")
    for vertex, level in levels.items():
        print(f"Vertex: {vertex}, Level: {level}")
```

## Explanation
Breadth-First Search (BFS) is a graph traversal algorithm that explores all vertices at the present depth level before moving on to vertices at the next depth level.

### How it works:
1. Start from a source vertex
2. Visit all neighbors of the current vertex
3. Move to next level of vertices
4. Repeat until all vertices are visited

### Time Complexity:
- Best Case: O(V + E)
- Average Case: O(V + E)
- Worst Case: O(V + E)

### Space Complexity: O(V)

### Applications:
- Shortest path in unweighted graphs
- Web crawling
- Social networking
- Network broadcasting
- Connected components

### Advantages:
- Finds shortest path in unweighted graphs
- Complete and optimal
- Works for directed and undirected graphs
- Can handle disconnected graphs
- Easy to implement

### Disadvantages:
- Not suitable for weighted graphs
- Memory intensive for large graphs
- May be slow for dense graphs
- Requires additional space
- Not suitable for all problems
