---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/edit-distance/"}
---

```python
def edit_distance(str1, str2):
    """
    Edit Distance (Levenshtein Distance) using Dynamic Programming
    Time Complexity: O(mn)
    Space Complexity: O(mn)
    where m and n are lengths of str1 and str2
    """
    m, n = len(str1), len(str2)
    
    # Create DP table
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Initialize first row and column
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    
    # Fill DP table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if str1[i-1] == str2[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],    # Delete
                    dp[i][j-1],    # Insert
                    dp[i-1][j-1]  # Replace
                )
    
    return dp[m][n]

def edit_distance_optimized(str1, str2):
    """
    Space-optimized version of Edit Distance
    Time Complexity: O(mn)
    Space Complexity: O(min(m, n))
    """
    m, n = len(str1), len(str2)
    if m < n:
        return edit_distance_optimized(str2, str1)
    
    # Use two rows instead of full table
    prev = [j for j in range(n + 1)]
    curr = [0] * (n + 1)
    
    for i in range(1, m + 1):
        curr[0] = i
        for j in range(1, n + 1):
            if str1[i-1] == str2[j-1]:
                curr[j] = prev[j-1]
            else:
                curr[j] = 1 + min(
                    prev[j],    # Delete
                    curr[j-1],  # Insert
                    prev[j-1]   # Replace
                )
        prev, curr = curr, prev
    
    return prev[n]

def get_operations(str1, str2):
    """
    Get sequence of operations to transform str1 to str2
    """
    m, n = len(str1), len(str2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Initialize DP table
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    
    # Fill DP table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if str1[i-1] == str2[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],
                    dp[i][j-1],
                    dp[i-1][j-1]
                )
    
    # Reconstruct operations
    operations = []
    i, j = m, n
    while i > 0 or j > 0:
        if i > 0 and j > 0 and str1[i-1] == str2[j-1]:
            i -= 1
            j -= 1
        else:
            if i > 0 and (j == 0 or dp[i][j] == dp[i-1][j] + 1):
                operations.append(f"Delete '{str1[i-1]}' at position {i-1}")
                i -= 1
            elif j > 0 and (i == 0 or dp[i][j] == dp[i][j-1] + 1):
                operations.append(f"Insert '{str2[j-1]}' at position {i}")
                j -= 1
            else:
                operations.append(f"Replace '{str1[i-1]}' with '{str2[j-1]}' at position {i-1}")
                i -= 1
                j -= 1
    
    return operations[::-1]

# Example usage
if __name__ == "__main__":
    # Test case
    str1 = "kitten"
    str2 = "sitting"
    
    # Get edit distance
    distance = edit_distance(str1, str2)
    print(f"Edit distance: {distance}")
    
    # Get edit distance using optimized version
    distance_opt = edit_distance_optimized(str1, str2)
    print(f"Edit distance (optimized): {distance_opt}")
    
    # Get sequence of operations
    operations = get_operations(str1, str2)
    print("\nOperations:")
    for op in operations:
        print(op)
```

## Explanation
The Edit Distance problem is to find the minimum number of operations (insertions, deletions, or substitutions) required to transform one string into another.

### How it works:
1. Create DP table with dimensions (m+1) x (n+1)
2. Initialize first row and column
3. Fill table in bottom-up manner:
   - If characters match, take diagonal value
   - Otherwise, take minimum of three operations + 1
4. Return value at bottom-right corner

### Time Complexity:
- Best Case: O(mn)
- Average Case: O(mn)
- Worst Case: O(mn)

### Space Complexity:
- Original: O(mn)
- Optimized: O(min(m, n))

### Applications:
- Spell checking
- DNA sequence alignment
- Plagiarism detection
- Text similarity
- Natural language processing

### Advantages:
- Optimal solution
- Can reconstruct operations
- Works for any strings
- Can be space optimized
- Useful in many domains

### Disadvantages:
- Quadratic time complexity
- May be memory intensive
- Not suitable for very long strings
- Requires additional space
- May be slow for some cases 