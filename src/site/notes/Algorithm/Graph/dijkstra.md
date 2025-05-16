---
{"dg-publish":true,"permalink":"/algorithm/graph/dijkstra/"}
---

# Dijkstra's Algorithm

```python
import heapq

def dijkstra(graph, start):
    """
    Dijkstra's Algorithm for Shortest Path
    Time Complexity: O((V + E) log V)
    Space Complexity: O(V + E)
    where V is number of vertices and E is number of edges
    """
    # Initialize
    distances = {vertex: float('infinity') for vertex in graph}
    distances[start] = 0
    parent = {start: None}
    priority_queue = [(0, start)]
    
    while priority_queue:
        current_distance, current_vertex = heapq.heappop(priority_queue)
        
        # Skip if we found a better path
        if current_distance > distances[current_vertex]:
            continue
        
        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight
            
            # Update if found shorter path
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                parent[neighbor] = current_vertex
                heapq.heappush(priority_queue, (distance, neighbor))
    
    return distances, parent

def dijkstra_path(graph, start, end):
    """
    Dijkstra's Algorithm with path reconstruction
    Time Complexity: O((V + E) log V)
    Space Complexity: O(V + E)
    """
    distances, parent = dijkstra(graph, start)
    
    # Reconstruct path
    if end not in parent:
        return None, float('infinity')
    
    path = []
    current = end
    while current is not None:
        path.append(current)
        current = parent[current]
    
    return path[::-1], distances[end]

def dijkstra_all_pairs(graph):
    """
    Dijkstra's Algorithm for all pairs shortest paths
    Time Complexity: O(V(V + E) log V)
    Space Complexity: O(V²)
    """
    all_distances = {}
    all_parents = {}
    
    for vertex in graph:
        distances, parent = dijkstra(graph, vertex)
        all_distances[vertex] = distances
        all_parents[vertex] = parent
    
    return all_distances, all_parents

def dijkstra_negative_weights(graph, start):
    """
    Dijkstra's Algorithm with negative weight handling
    Time Complexity: O((V + E) log V)
    Space Complexity: O(V + E)
    """
    # Check for negative weights
    for u in graph:
        for v, weight in graph[u].items():
            if weight < 0:
                raise ValueError("Graph contains negative weights")
    
    return dijkstra(graph, start)

# Example usage
if __name__ == "__main__":
    # Test case
    graph = {
        'A': {'B': 1, 'C': 4},
        'B': {'A': 1, 'C': 2, 'D': 5},
        'C': {'A': 4, 'B': 2, 'D': 1},
        'D': {'B': 5, 'C': 1}
    }
    
    # Basic Dijkstra
    distances, parent = dijkstra(graph, 'A')
    print("Shortest Distances from A:")
    for vertex, distance in distances.items():
        print(f"Vertex: {vertex}, Distance: {distance}")
    
    # Path finding
    path, distance = dijkstra_path(graph, 'A', 'D')
    print("\nShortest Path from A to D:")
    print(f"Path: {' -> '.join(path)}")
    print(f"Distance: {distance}")
    
    # All pairs shortest paths
    all_distances, all_parents = dijkstra_all_pairs(graph)
    print("\nAll Pairs Shortest Distances:")
    for start in graph:
        print(f"\nFrom {start}:")
        for end in graph:
            print(f"To {end}: {all_distances[start][end]}")
    
    # Negative weight handling
    try:
        graph_negative = {
            'A': {'B': 1, 'C': -4},
            'B': {'A': 1, 'C': 2},
            'C': {'A': -4, 'B': 2}
        }
        dijkstra_negative_weights(graph_negative, 'A')
    except ValueError as e:
        print(f"\nError: {e}")
```

## Explanation
Dijkstra's algorithm is a greedy algorithm that finds the shortest paths from a single source vertex to all other vertices in a weighted graph.

### How it works:
1. Initialize distances to infinity
2. Set distance to start vertex to 0
3. Use priority queue to process vertices
4. For each vertex, update distances to neighbors
5. Repeat until all vertices are processed

### Time Complexity:
- Best Case: O((V + E) log V)
- Average Case: O((V + E) log V)
- Worst Case: O((V + E) log V)

### Space Complexity: O(V + E)

### Applications:
- Routing protocols
- Network analysis
- Traffic engineering
- Path planning
- Resource allocation

### Advantages:
- Efficient for sparse graphs
- Works for directed graphs
- Can handle positive weights
- Easy to implement
- Memory efficient

### Disadvantages:
- Doesn't work with negative weights
- Slower for dense graphs
- Not suitable for all problems
- May be memory intensive
- Requires priority queue 