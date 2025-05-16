---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/knapsack/"}
---

```python
def knapsack(values, weights, capacity):
    """
    0/1 Knapsack Problem using Dynamic Programming
    Time Complexity: O(nW)
    Space Complexity: O(nW)
    where n is number of items and W is capacity
    """
    n = len(values)
    
    # Create DP table
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    
    # Fill DP table
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                dp[i][w] = max(
                    values[i-1] + dp[i-1][w-weights[i-1]],  # Include item
                    dp[i-1][w]  # Exclude item
                )
            else:
                dp[i][w] = dp[i-1][w]  # Cannot include item
    
    # Reconstruct solution
    selected_items = []
    w = capacity
    for i in range(n, 0, -1):
        if dp[i][w] != dp[i-1][w]:
            selected_items.append(i-1)
            w -= weights[i-1]
    
    return dp[n][capacity], selected_items

def knapsack_optimized(values, weights, capacity):
    """
    Space-optimized version of 0/1 Knapsack
    Time Complexity: O(nW)
    Space Complexity: O(W)
    """
    n = len(values)
    dp = [0] * (capacity + 1)
    
    for i in range(n):
        for w in range(capacity, weights[i]-1, -1):
            dp[w] = max(dp[w], values[i] + dp[w-weights[i]])
    
    return dp[capacity]

# Example usage
if __name__ == "__main__":
    # Test case
    values = [60, 100, 120]
    weights = [10, 20, 30]
    capacity = 50
    
    # Get maximum value and selected items
    max_value, selected = knapsack(values, weights, capacity)
    print(f"Maximum value: {max_value}")
    print(f"Selected items: {selected}")
    
    # Get maximum value using optimized version
    max_value_opt = knapsack_optimized(values, weights, capacity)
    print(f"Maximum value (optimized): {max_value_opt}")
```

## Explanation
The 0/1 Knapsack problem is to select items with maximum value while not exceeding a given weight capacity. Each item can be either taken (1) or not taken (0).

### How it works:
1. Create DP table with dimensions (n+1) x (W+1)
2. Fill table in bottom-up manner:
   - For each item
   - For each possible weight
   - Choose maximum of including or excluding current item
3. Reconstruct solution by backtracking through table

### Time Complexity:
- Best Case: O(nW)
- Average Case: O(nW)
- Worst Case: O(nW)

### Space Complexity:
- Original: O(nW)
- Optimized: O(W)

### Applications:
- Resource allocation
- Portfolio optimization
- Cutting stock problem
- Resource scheduling
- Budget allocation

### Advantages:
- Guaranteed optimal solution
- Works for integer weights
- Can handle large values
- Easy to implement
- Can be space optimized

### Disadvantages:
- Pseudo-polynomial time
- Not suitable for large W
- Requires integer weights
- May be memory intensive
- Limited to 0/1 decisions 