---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/rod-cutting/"}
---


```python
def rod_cutting(prices, n):
    """
    Rod Cutting Problem using Dynamic Programming
    Time Complexity: O(n²)
    Space Complexity: O(n)
    """
    # Create a table to store solutions of subproblems
    dp = [0] * (n + 1)
    
    # Build the table in bottom-up manner
    for i in range(1, n + 1):
        max_val = float('-inf')
        for j in range(i):
            max_val = max(max_val, prices[j] + dp[i - j - 1])
        dp[i] = max_val
    
    return dp[n]

def rod_cutting_with_solution(prices, n):
    """
    Rod Cutting Problem with solution reconstruction
    Time Complexity: O(n²)
    Space Complexity: O(n)
    """
    dp = [0] * (n + 1)
    cuts = [0] * (n + 1)
    
    for i in range(1, n + 1):
        max_val = float('-inf')
        for j in range(i):
            if prices[j] + dp[i - j - 1] > max_val:
                max_val = prices[j] + dp[i - j - 1]
                cuts[i] = j + 1
        dp[i] = max_val
    
    # Reconstruct the solution
    solution = []
    length = n
    while length > 0:
        solution.append(cuts[length])
        length -= cuts[length]
    
    return dp[n], solution

# Example usage
if __name__ == "__main__":
    # Price table for rods of length 1 to 8
    prices = [1, 5, 8, 9, 10, 17, 17, 20]
    n = 8
    
    # Get maximum value
    max_value = rod_cutting(prices, n)
    print(f"Maximum value for rod of length {n}: {max_value}")
    
    # Get maximum value with solution
    max_value, solution = rod_cutting_with_solution(prices, n)
    print(f"Maximum value for rod of length {n}: {max_value}")
    print(f"Optimal cuts: {solution}")
```

## Explanation
The Rod Cutting problem is a classic optimization problem where we need to cut a rod of length n into pieces to maximize the total value. Each piece of length i has a price p[i].

### How it works:
1. Create a table to store solutions of subproblems
2. For each length i from 1 to n:
   - Try all possible cuts j from 0 to i-1
   - Calculate value for cut j plus value of remaining length
   - Store maximum value in table
3. Return value for length n

### Time Complexity:
- Best Case: O(n²)
- Average Case: O(n²)
- Worst Case: O(n²)

### Space Complexity: O(n)

### Applications:
- Resource allocation
- Inventory management
- Production planning
- Financial optimization
- Supply chain management

### Advantages:
- Optimal solution
- Efficient for small to medium n
- Easy to implement
- Can be extended to other problems
- Works for any price function

### Disadvantages:
- Quadratic time complexity
- Not suitable for very large n
- Requires price table
- Limited to one-dimensional cuts
- May not be optimal for all cases
