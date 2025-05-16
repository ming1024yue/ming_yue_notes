---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/partition-equal-subset-sum/"}
---


```python
def can_partition(nums):
    """
    Partition Equal Subset Sum using Dynamic Programming
    Time Complexity: O(nS)
    Space Complexity: O(S)
    where n is number of elements and S is total sum
    """
    total_sum = sum(nums)
    
    # If total sum is odd, can't partition
    if total_sum % 2 != 0:
        return False
    
    target = total_sum // 2
    n = len(nums)
    
    # Create DP array
    dp = [False] * (target + 1)
    dp[0] = True
    
    # Fill DP array
    for num in nums:
        for j in range(target, num - 1, -1):
            dp[j] = dp[j] or dp[j - num]
    
    return dp[target]

def can_partition_with_solution(nums):
    """
    Partition Equal Subset Sum with solution reconstruction
    Time Complexity: O(nS)
    Space Complexity: O(nS)
    """
    total_sum = sum(nums)
    
    if total_sum % 2 != 0:
        return False, []
    
    target = total_sum // 2
    n = len(nums)
    
    # Create DP table
    dp = [[False] * (target + 1) for _ in range(n + 1)]
    dp[0][0] = True
    
    # Fill DP table
    for i in range(1, n + 1):
        for j in range(target + 1):
            if j < nums[i-1]:
                dp[i][j] = dp[i-1][j]
            else:
                dp[i][j] = dp[i-1][j] or dp[i-1][j-nums[i-1]]
    
    # Reconstruct solution
    if not dp[n][target]:
        return False, []
    
    solution = []
    j = target
    for i in range(n, 0, -1):
        if not dp[i-1][j]:
            solution.append(nums[i-1])
            j -= nums[i-1]
    
    return True, solution

def can_partition_optimized(nums):
    """
    Space-optimized version of Partition Equal Subset Sum
    Time Complexity: O(nS)
    Space Complexity: O(S)
    """
    total_sum = sum(nums)
    
    if total_sum % 2 != 0:
        return False
    
    target = total_sum // 2
    
    # Use bitset for faster computation
    dp = 1  # Initialize with 1 (binary 1)
    
    for num in nums:
        dp |= dp << num
    
    return bool(dp & (1 << target))

# Example usage
if __name__ == "__main__":
    # Test case
    nums = [1, 5, 11, 5]
    
    # Check if can partition
    can_part = can_partition(nums)
    print(f"Can partition: {can_part}")
    
    # Get partition solution
    can_part, solution = can_partition_with_solution(nums)
    print(f"\nCan partition: {can_part}")
    if can_part:
        print(f"Partition: {solution}")
    
    # Check using optimized version
    can_part_opt = can_partition_optimized(nums)
    print(f"\nCan partition (optimized): {can_part_opt}")
```

## Explanation
The Partition Equal Subset Sum problem is to determine if a given array can be partitioned into two subsets with equal sum.

### How it works:
1. Calculate total sum of array
2. If sum is odd, return False
3. Set target as half of total sum
4. Create DP array initialized to False
5. Fill DP array in bottom-up manner:
   - For each number
   - For each possible sum
   - Update DP value if sum can be achieved

### Time Complexity:
- Best Case: O(nS)
- Average Case: O(nS)
- Worst Case: O(nS)

### Space Complexity:
- Original: O(S)
- With Solution: O(nS)
- Optimized: O(S)

### Applications:
- Resource allocation
- Load balancing
- Data partitioning
- Task scheduling
- Financial planning

### Advantages:
- Optimal solution
- Can reconstruct solution
- Works for any numbers
- Can be optimized
- Useful in many domains

### Disadvantages:
- Pseudo-polynomial time
- Not suitable for large sums
- Requires integer numbers
- May be memory intensive
- Limited to two partitions 