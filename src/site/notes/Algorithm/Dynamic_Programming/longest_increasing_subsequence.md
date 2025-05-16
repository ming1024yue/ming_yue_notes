---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/longest-increasing-subsequence/"}
---

```python
def longest_increasing_subsequence(arr):
    """
    Longest Increasing Subsequence using Dynamic Programming
    Time Complexity: O(n²)
    Space Complexity: O(n)
    """
    n = len(arr)
    
    # Initialize DP array
    dp = [1] * n
    
    # Fill DP array
    for i in range(1, n):
        for j in range(i):
            if arr[i] > arr[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    # Find maximum length
    max_length = max(dp)
    
    # Reconstruct subsequence
    subsequence = []
    current_length = max_length
    for i in range(n-1, -1, -1):
        if dp[i] == current_length:
            subsequence.append(arr[i])
            current_length -= 1
            if current_length == 0:
                break
    
    return max_length, subsequence[::-1]

def lis_optimized(arr):
    """
    Optimized LIS using binary search
    Time Complexity: O(n log n)
    Space Complexity: O(n)
    """
    n = len(arr)
    tails = [0] * n
    size = 0
    
    for x in arr:
        i, j = 0, size
        while i != j:
            m = (i + j) // 2
            if tails[m] < x:
                i = m + 1
            else:
                j = m
        tails[i] = x
        size = max(i + 1, size)
    
    return size

# Example usage
if __name__ == "__main__":
    # Test case
    arr = [10, 22, 9, 33, 21, 50, 41, 60, 80]
    
    # Get length and subsequence
    length, subsequence = longest_increasing_subsequence(arr)
    print(f"Length of LIS: {length}")
    print(f"One possible LIS: {subsequence}")
    
    # Get length using optimized version
    length_opt = lis_optimized(arr)
    print(f"Length of LIS (optimized): {length_opt}")
```

## Explanation
The Longest Increasing Subsequence problem is to find the longest subsequence of a given sequence in which the subsequence's elements are in sorted order, lowest to highest.

### How it works:
1. Create DP array initialized to 1
2. Fill DP array in bottom-up manner:
   - For each element
   - Compare with all previous elements
   - Update DP value if current element is larger
3. Find maximum value in DP array
4. Reconstruct subsequence by backtracking

### Time Complexity:
- Original: O(n²)
- Optimized: O(n log n)

### Space Complexity: O(n)

### Applications:
- Pattern recognition
- Bioinformatics
- Data compression
- Sequence alignment
- Stock market analysis

### Advantages:
- Simple implementation
- Works for any sequence
- Can reconstruct solution
- Can be optimized
- Useful in many domains

### Disadvantages:
- Quadratic time in basic version
- May find only one solution
- Not suitable for very large n
- Requires additional space
- May be slow for some cases 