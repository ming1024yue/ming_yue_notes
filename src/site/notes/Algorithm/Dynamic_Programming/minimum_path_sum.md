---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/minimum-path-sum/"}
---

```python
def min_path_sum(grid):
    """
    Minimum Path Sum using Dynamic Programming
    Time Complexity: O(mn)
    Space Complexity: O(mn)
    where m and n are dimensions of grid
    """
    if not grid or not grid[0]:
        return 0
    
    m, n = len(grid), len(grid[0])
    
    # Create DP table
    dp = [[0] * n for _ in range(m)]
    dp[0][0] = grid[0][0]
    
    # Initialize first row
    for j in range(1, n):
        dp[0][j] = dp[0][j-1] + grid[0][j]
    
    # Initialize first column
    for i in range(1, m):
        dp[i][0] = dp[i-1][0] + grid[i][0]
    
    # Fill DP table
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
    
    return dp[m-1][n-1]

def min_path_sum_with_path(grid):
    """
    Minimum Path Sum with path reconstruction
    Time Complexity: O(mn)
    Space Complexity: O(mn)
    """
    if not grid or not grid[0]:
        return 0, []
    
    m, n = len(grid), len(grid[0])
    
    # Create DP table and path table
    dp = [[0] * n for _ in range(m)]
    path = [[None] * n for _ in range(m)]
    dp[0][0] = grid[0][0]
    
    # Initialize first row
    for j in range(1, n):
        dp[0][j] = dp[0][j-1] + grid[0][j]
        path[0][j] = 'R'  # Right
    
    # Initialize first column
    for i in range(1, m):
        dp[i][0] = dp[i-1][0] + grid[i][0]
        path[i][0] = 'D'  # Down
    
    # Fill DP table
    for i in range(1, m):
        for j in range(1, n):
            if dp[i-1][j] < dp[i][j-1]:
                dp[i][j] = dp[i-1][j] + grid[i][j]
                path[i][j] = 'D'
            else:
                dp[i][j] = dp[i][j-1] + grid[i][j]
                path[i][j] = 'R'
    
    # Reconstruct path
    i, j = m-1, n-1
    result_path = []
    while i > 0 or j > 0:
        result_path.append(path[i][j])
        if path[i][j] == 'D':
            i -= 1
        else:
            j -= 1
    
    return dp[m-1][n-1], result_path[::-1]

def min_path_sum_optimized(grid):
    """
    Space-optimized version of Minimum Path Sum
    Time Complexity: O(mn)
    Space Complexity: O(n)
    """
    if not grid or not grid[0]:
        return 0
    
    m, n = len(grid), len(grid[0])
    
    # Use two rows instead of full table
    prev = [0] * n
    curr = [0] * n
    
    # Initialize first row
    prev[0] = grid[0][0]
    for j in range(1, n):
        prev[j] = prev[j-1] + grid[0][j]
    
    # Fill DP table
    for i in range(1, m):
        curr[0] = prev[0] + grid[i][0]
        for j in range(1, n):
            curr[j] = min(prev[j], curr[j-1]) + grid[i][j]
        prev, curr = curr, [0] * n
    
    return prev[n-1]

# Example usage
if __name__ == "__main__":
    # Test case
    grid = [
        [1, 3, 1],
        [1, 5, 1],
        [4, 2, 1]
    ]
    
    # Get minimum path sum
    min_sum = min_path_sum(grid)
    print(f"Minimum path sum: {min_sum}")
    
    # Get minimum path sum with path
    min_sum, path = min_path_sum_with_path(grid)
    print(f"\nMinimum path sum: {min_sum}")
    print(f"Path: {path}")
    
    # Get minimum path sum using optimized version
    min_sum_opt = min_path_sum_optimized(grid)
    print(f"\nMinimum path sum (optimized): {min_sum_opt}")
```

## Explanation
The Minimum Path Sum problem is to find the path from the top-left to the bottom-right corner of a grid that minimizes the sum of numbers along the path.

### How it works:
1. Create DP table with same dimensions as grid
2. Initialize first row and column
3. Fill DP table in bottom-up manner:
   - For each cell
   - Take minimum of top and left cells
   - Add current cell value
4. Return value at bottom-right corner

### Time Complexity:
- Best Case: O(mn)
- Average Case: O(mn)
- Worst Case: O(mn)

### Space Complexity:
- Original: O(mn)
- With Path: O(mn)
- Optimized: O(n)

### Applications:
- Route planning
- Resource allocation
- Network routing
- Game development
- Image processing

### Advantages:
- Optimal solution
- Can reconstruct path
- Works for any grid
- Can be optimized
- Useful in many domains

### Disadvantages:
- Quadratic time complexity
- May be memory intensive
- Not suitable for very large grids
- Requires additional space
- May be slow for some cases 