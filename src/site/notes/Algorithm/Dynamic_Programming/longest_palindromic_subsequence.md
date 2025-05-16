---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/longest-palindromic-subsequence/"}
---

```python
def longest_palindromic_subsequence(s):
    """
    Longest Palindromic Subsequence using Dynamic Programming
    Time Complexity: O(n²)
    Space Complexity: O(n²)
    """
    n = len(s)
    
    # Create DP table
    dp = [[0] * n for _ in range(n)]
    
    # Initialize diagonal (single characters)
    for i in range(n):
        dp[i][i] = 1
    
    # Fill DP table
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                dp[i][j] = 2 + dp[i+1][j-1] if length > 2 else 2
            else:
                dp[i][j] = max(dp[i+1][j], dp[i][j-1])
    
    return dp[0][n-1]

def longest_palindromic_subsequence_with_string(s):
    """
    Longest Palindromic Subsequence with string reconstruction
    Time Complexity: O(n²)
    Space Complexity: O(n²)
    """
    n = len(s)
    
    # Create DP table and direction table
    dp = [[0] * n for _ in range(n)]
    direction = [[None] * n for _ in range(n)]
    
    # Initialize diagonal
    for i in range(n):
        dp[i][i] = 1
        direction[i][i] = 'D'
    
    # Fill DP table
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                dp[i][j] = 2 + dp[i+1][j-1] if length > 2 else 2
                direction[i][j] = 'D'
            else:
                if dp[i+1][j] > dp[i][j-1]:
                    dp[i][j] = dp[i+1][j]
                    direction[i][j] = 'L'
                else:
                    dp[i][j] = dp[i][j-1]
                    direction[i][j] = 'R'
    
    # Reconstruct subsequence
    result = []
    i, j = 0, n-1
    while i <= j:
        if direction[i][j] == 'D':
            result.append(s[i])
            i += 1
            j -= 1
        elif direction[i][j] == 'L':
            i += 1
        else:
            j -= 1
    
    # Handle even length palindrome
    if len(result) * 2 > dp[0][n-1]:
        result = result[:-1]
    
    # Create full palindrome
    palindrome = result + result[::-1]
    return dp[0][n-1], ''.join(palindrome)

def longest_palindromic_subsequence_optimized(s):
    """
    Space-optimized version of Longest Palindromic Subsequence
    Time Complexity: O(n²)
    Space Complexity: O(n)
    """
    n = len(s)
    
    # Use two rows instead of full table
    prev = [0] * n
    curr = [0] * n
    
    # Initialize first row
    for i in range(n):
        prev[i] = 1
    
    # Fill DP table
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                curr[i] = 2 + prev[i+1] if length > 2 else 2
            else:
                curr[i] = max(prev[i], curr[i+1])
        prev, curr = curr, [0] * n
    
    return prev[0]

# Example usage
if __name__ == "__main__":
    # Test case
    s = "BBABCBCAB"
    
    # Get length of LPS
    length = longest_palindromic_subsequence(s)
    print(f"Length of LPS: {length}")
    
    # Get LPS string
    length, lps = longest_palindromic_subsequence_with_string(s)
    print(f"Length of LPS: {length}")
    print(f"LPS: {lps}")
    
    # Get length using optimized version
    length_opt = longest_palindromic_subsequence_optimized(s)
    print(f"Length of LPS (optimized): {length_opt}")
```

## Explanation
The Longest Palindromic Subsequence problem is to find the longest subsequence of a given string that is also a palindrome.

### How it works:
1. Create DP table with dimensions n x n
2. Initialize diagonal (single characters)
3. Fill table in bottom-up manner:
   - If characters match, add 2 to diagonal value
   - Otherwise, take maximum of left and bottom values
4. Return value at top-right corner

### Time Complexity:
- Best Case: O(n²)
- Average Case: O(n²)
- Worst Case: O(n²)

### Space Complexity:
- Original: O(n²)
- Optimized: O(n)

### Applications:
- DNA sequence analysis
- Text compression
- Pattern matching
- Bioinformatics
- String processing

### Advantages:
- Optimal solution
- Can reconstruct subsequence
- Works for any string
- Can be space optimized
- Useful in many domains

### Disadvantages:
- Quadratic time complexity
- May be memory intensive
- Not suitable for very long strings
- Requires additional space
- May be slow for some cases 