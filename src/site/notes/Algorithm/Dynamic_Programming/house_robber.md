---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/house-robber/"}
---


# House Robber

```python
def rob(nums):
    """
    House Robber using Dynamic Programming
    Time Complexity: O(n)
    Space Complexity: O(n)
    """
    if not nums:
        return 0
    
    n = len(nums)
    if n == 1:
        return nums[0]
    
    # Create DP array
    dp = [0] * n
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    
    # Fill DP array
    for i in range(2, n):
        dp[i] = max(dp[i-1], dp[i-2] + nums[i])
    
    return dp[n-1]

def rob_optimized(nums):
    """
    Space-optimized version of House Robber
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not nums:
        return 0
    
    n = len(nums)
    if n == 1:
        return nums[0]
    
    # Use two variables instead of array
    prev2 = nums[0]
    prev1 = max(nums[0], nums[1])
    
    for i in range(2, n):
        current = max(prev1, prev2 + nums[i])
        prev2, prev1 = prev1, current
    
    return prev1

def rob_circular(nums):
    """
    House Robber with Circular Arrangement
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not nums:
        return 0
    
    n = len(nums)
    if n == 1:
        return nums[0]
    
    # Case 1: Rob first house, skip last
    result1 = rob_optimized(nums[:-1])
    
    # Case 2: Skip first house, rob last
    result2 = rob_optimized(nums[1:])
    
    return max(result1, result2)

def rob_with_solution(nums):
    """
    House Robber with solution reconstruction
    Time Complexity: O(n)
    Space Complexity: O(n)
    """
    if not nums:
        return 0, []
    
    n = len(nums)
    if n == 1:
        return nums[0], [0]
    
    # Create DP array and solution array
    dp = [0] * n
    solution = [[] for _ in range(n)]
    
    dp[0] = nums[0]
    solution[0] = [0]
    
    if nums[0] > nums[1]:
        dp[1] = nums[0]
        solution[1] = [0]
    else:
        dp[1] = nums[1]
        solution[1] = [1]
    
    # Fill DP array
    for i in range(2, n):
        if dp[i-1] > dp[i-2] + nums[i]:
            dp[i] = dp[i-1]
            solution[i] = solution[i-1]
        else:
            dp[i] = dp[i-2] + nums[i]
            solution[i] = solution[i-2] + [i]
    
    return dp[n-1], solution[n-1]

# Example usage
if __name__ == "__main__":
    # Test case for basic house robber
    nums = [2, 7, 9, 3, 1]
    max_amount = rob(nums)
    print(f"Maximum amount: {max_amount}")
    
    # Test case for optimized version
    max_amount_opt = rob_optimized(nums)
    print(f"\nMaximum amount (optimized): {max_amount_opt}")
    
    # Test case for circular arrangement
    nums_circular = [2, 3, 2]
    max_amount_circular = rob_circular(nums_circular)
    print(f"\nMaximum amount (circular): {max_amount_circular}")
    
    # Test case for solution reconstruction
    max_amount, solution = rob_with_solution(nums)
    print(f"\nMaximum amount: {max_amount}")
    print(f"Houses to rob: {solution}")
```

## Explanation
The House Robber problem is to determine the maximum amount of money that can be robbed from houses arranged in a line, with the constraint that adjacent houses cannot be robbed.

### How it works:
1. Create DP array initialized to 0
2. Set base cases (first and second houses)
3. Fill DP array in bottom-up manner:
   - For each house
   - Choose maximum of:
     - Robbing current house + amount from two houses back
     - Not robbing current house (amount from previous house)
4. Return value at last position

### Time Complexity:
- Best Case: O(n)
- Average Case: O(n)
- Worst Case: O(n)

### Space Complexity:
- Original: O(n)
- Optimized: O(1)
- With Solution: O(n)

### Applications:
- Resource allocation
- Investment planning
- Task scheduling
- Network optimization
- Financial planning

### Advantages:
- Optimal solution
- Can reconstruct solution
- Works for any values
- Can be optimized
- Handles circular case

### Disadvantages:
- Linear time complexity
- May be memory intensive
- Not suitable for very large n
- Requires additional space
- May be slow for some cases 