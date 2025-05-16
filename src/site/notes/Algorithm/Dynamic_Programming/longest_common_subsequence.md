---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/longest-common-subsequence/"}
---


```python
def longest_common_subsequence(X, Y):
    """
    Longest Common Subsequence using Dynamic Programming
    Time Complexity: O(mn)
    Space Complexity: O(mn)
    """
    m = len(X)
    n = len(Y)
    
    # Create DP table
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Fill DP table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if X[i - 1] == Y[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    
    # Reconstruct LCS
    lcs = []
    i, j = m, n
    while i > 0 and j > 0:
        if X[i - 1] == Y[j - 1]:
            lcs.append(X[i - 1])
            i -= 1
            j -= 1
        elif dp[i - 1][j] > dp[i][j - 1]:
            i -= 1
        else:
            j -= 1
    
    return dp[m][n], ''.join(reversed(lcs))

# Example usage
if __name__ == "__main__":
    # Test cases
    test_cases = [
        ("ABCDGH", "AEDFHR"),  # ADH
        ("AGGTAB", "GXTXAYB"),  # GTAB
        ("ABC", "DEF"),  # ""
        ("", ""),  # ""
        ("A", "A"),  # A
    ]
    
    for X, Y in test_cases:
        length, lcs = longest_common_subsequence(X, Y)
        print(f"X: {X}, Y: {Y}")
        print(f"Length of LCS: {length}")
        print(f"LCS: {lcs}")
        print()
```

## Explanation
The Longest Common Subsequence (LCS) problem is to find the longest subsequence that is common to two given sequences. A subsequence is a sequence that appears in the same relative order but not necessarily contiguous.

### How it works:
1. Create a DP table of size (m+1) x (n+1)
2. Fill the table in bottom-up manner:
   - If characters match, add 1 to diagonal value
   - If characters don't match, take maximum of left and top values
3. Reconstruct LCS by backtracking through the table

### Time Complexity:
- Best Case: O(mn)
- Average Case: O(mn)
- Worst Case: O(mn)

### Space Complexity: O(mn)

### Applications:
- DNA sequence alignment
- Text comparison
- Version control systems
- Plagiarism detection
- Data compression

### Advantages:
- Optimal solution
- Efficient for small to medium sequences
- Easy to implement
- Can be extended to multiple sequences
- Works for any type of sequence

### Disadvantages:
- Quadratic time complexity
- Not suitable for very long sequences
- Requires large memory for long sequences
- Limited to two sequences in basic form
- May not be optimal for all cases
