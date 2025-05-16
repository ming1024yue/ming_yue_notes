---
{"dg-publish":true,"permalink":"/algorithm/graph/depth-first-search/"}
---


```python
def dfs(graph, start):
    """
    Depth-First Search
    Time Complexity: O(V + E)
    Space Complexity: O(V)
    where V is number of vertices and E is number of edges
    """
    visited = set()
    parent = {start: None}
    discovery_time = {}
    finish_time = {}
    time = 0
    
    def dfs_visit(vertex):
        nonlocal time
        visited.add(vertex)
        time += 1
        discovery_time[vertex] = time
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                parent[neighbor] = vertex
                dfs_visit(neighbor)
        
        time += 1
        finish_time[vertex] = time
    
    dfs_visit(start)
    return parent, discovery_time, finish_time

def dfs_path(graph, start, end):
    """
    DFS with path reconstruction
    Time Complexity: O(V + E)
    Space Complexity: O(V)
    """
    parent, _, _ = dfs(graph, start)
    
    # Reconstruct path
    if end not in parent:
        return None
    
    path = []
    current = end
    while current is not None:
        path.append(current)
        current = parent[current]
    
    return path[::-1]

def dfs_components(graph):
    """
    DFS for finding connected components
    Time Complexity: O(V + E)
    Space Complexity: O(V)
    """
    visited = set()
    components = []
    
    def dfs_visit(vertex, component):
        visited.add(vertex)
        component.append(vertex)
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                dfs_visit(neighbor, component)
    
    for vertex in graph:
        if vertex not in visited:
            component = []
            dfs_visit(vertex, component)
            components.append(component)
    
    return components

def dfs_cycle(graph):
    """
    DFS for cycle detection
    Time Complexity: O(V + E)
    Space Complexity: O(V)
    """
    visited = set()
    recursion_stack = set()
    
    def has_cycle(vertex):
        visited.add(vertex)
        recursion_stack.add(vertex)
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                if has_cycle(neighbor):
                    return True
            elif neighbor in recursion_stack:
                return True
        
        recursion_stack.remove(vertex)
        return False
    
    for vertex in graph:
        if vertex not in visited:
            if has_cycle(vertex):
                return True
    
    return False

def dfs_topological_sort(graph):
    """
    DFS for topological sorting
    Time Complexity: O(V + E)
    Space Complexity: O(V)
    """
    visited = set()
    result = []
    
    def dfs_visit(vertex):
        visited.add(vertex)
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                dfs_visit(neighbor)
        
        result.append(vertex)
    
    for vertex in graph:
        if vertex not in visited:
            dfs_visit(vertex)
    
    return result[::-1]

# Example usage
if __name__ == "__main__":
    # Test case
    graph = {
        'A': ['B', 'C'],
        'B': ['D', 'E'],
        'C': ['F'],
        'D': [],
        'E': ['F'],
        'F': []
    }
    
    # Basic DFS
    parent, discovery, finish = dfs(graph, 'A')
    print("DFS Parent, Discovery, and Finish Times:")
    for vertex in graph:
        print(f"Vertex: {vertex}, Parent: {parent[vertex]}, Discovery: {discovery.get(vertex, 'N/A')}, Finish: {finish.get(vertex, 'N/A')}")
    
    # Path finding
    path = dfs_path(graph, 'A', 'F')
    print("\nPath from A to F:", path)
    
    # Connected components
    components = dfs_components(graph)
    print("\nConnected Components:", components)
    
    # Cycle detection
    has_cycle = dfs_cycle(graph)
    print("\nGraph has cycle:", has_cycle)
    
    # Topological sort
    top_sort = dfs_topological_sort(graph)
    print("\nTopological Sort:", top_sort)
```

## Explanation
Depth-First Search (DFS) is a graph traversal algorithm that explores as far as possible along each branch before backtracking.

### How it works:
1. Start from a source vertex
2. Visit first unvisited neighbor
3. Recursively visit its neighbors
4. Backtrack when no more unvisited neighbors
5. Repeat until all vertices are visited

### Time Complexity:
- Best Case: O(V + E)
- Average Case: O(V + E)
- Worst Case: O(V + E)

### Space Complexity: O(V)

### Applications:
- Topological sorting
- Cycle detection
- Connected components
- Path finding
- Maze solving

### Advantages:
- Less memory than BFS
- Can find cycles
- Works for directed graphs
- Can do topological sort
- Useful for many problems

### Disadvantages:
- May not find shortest path
- Can get stuck in deep paths
- Not suitable for all problems
- May be slow for some graphs
- Requires careful implementation
