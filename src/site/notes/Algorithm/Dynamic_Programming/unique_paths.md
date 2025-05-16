---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/unique-paths/"}
---

```python
def unique_paths(m, n):
    """
    Unique Paths using Dynamic Programming
    Time Complexity: O(mn)
    Space Complexity: O(mn)
    where m and n are grid dimensions
    """
    # Create DP table
    dp = [[0] * n for _ in range(m)]
    
    # Initialize first row and column
    for i in range(m):
        dp[i][0] = 1
    for j in range(n):
        dp[0][j] = 1
    
    # Fill DP table
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
    
    return dp[m-1][n-1]

def unique_paths_with_obstacles(grid):
    """
    Unique Paths with Obstacles using Dynamic Programming
    Time Complexity: O(mn)
    Space Complexity: O(mn)
    """
    if not grid or not grid[0]:
        return 0
    
    m, n = len(grid), len(grid[0])
    
    # Create DP table
    dp = [[0] * n for _ in range(m)]
    
    # Initialize first row and column
    dp[0][0] = 1 if grid[0][0] == 0 else 0
    for i in range(1, m):
        dp[i][0] = dp[i-1][0] if grid[i][0] == 0 else 0
    for j in range(1, n):
        dp[0][j] = dp[0][j-1] if grid[0][j] == 0 else 0
    
    # Fill DP table
    for i in range(1, m):
        for j in range(1, n):
            if grid[i][j] == 0:
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
            else:
                dp[i][j] = 0
    
    return dp[m-1][n-1]

def unique_paths_optimized(m, n):
    """
    Space-optimized version of Unique Paths
    Time Complexity: O(mn)
    Space Complexity: O(n)
    """
    # Use two rows instead of full table
    prev = [1] * n
    curr = [1] * n
    
    # Fill DP table
    for i in range(1, m):
        for j in range(1, n):
            curr[j] = prev[j] + curr[j-1]
        prev, curr = curr, [1] * n
    
    return prev[n-1]

def unique_paths_math(m, n):
    """
    Unique Paths using Combinatorics
    Time Complexity: O(min(m, n))
    Space Complexity: O(1)
    """
    # Total steps = m-1 down + n-1 right
    # Number of paths = (m+n-2)! / ((m-1)! * (n-1)!)
    from math import comb
    return comb(m + n - 2, m - 1)

# Example usage
if __name__ == "__main__":
    # Test case for basic unique paths
    m, n = 3, 7
    paths = unique_paths(m, n)
    print(f"Number of unique paths: {paths}")
    
    # Test case for unique paths with obstacles
    grid = [
        [0, 0, 0],
        [0, 1, 0],
        [0, 0, 0]
    ]
    paths_obs = unique_paths_with_obstacles(grid)
    print(f"\nNumber of unique paths with obstacles: {paths_obs}")
    
    # Test case for optimized version
    paths_opt = unique_paths_optimized(m, n)
    print(f"\nNumber of unique paths (optimized): {paths_opt}")
    
    # Test case for mathematical solution
    paths_math = unique_paths_math(m, n)
    print(f"\nNumber of unique paths (mathematical): {paths_math}")
```

## Explanation
The Unique Paths problem is to find the number of unique paths from the top-left to the bottom-right corner of a grid, moving only right or down.

### How it works:
1. Create DP table with dimensions m x n
2. Initialize first row and column to 1
3. Fill DP table in bottom-up manner:
   - For each cell
   - Sum paths from top and left cells
4. Return value at bottom-right corner

### Time Complexity:
- DP Solution: O(mn)
- Mathematical: O(min(m, n))

### Space Complexity:
- Original: O(mn)
- Optimized: O(n)
- Mathematical: O(1)

### Applications:
- Robot navigation
- Game development
- Path planning
- Network routing
- Resource allocation

### Advantages:
- Optimal solution
- Works for any grid
- Can handle obstacles
- Can be optimized
- Mathematical solution available

### Disadvantages:
- Quadratic time in DP
- May be memory intensive
- Not suitable for very large grids
- Requires additional space
- May be slow for some cases 