---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/decode-ways/"}
---


# Decode Ways

```python
def num_decodings(s):
    """
    Decode Ways using Dynamic Programming
    Time Complexity: O(n)
    Space Complexity: O(n)
    """
    if not s or s[0] == '0':
        return 0
    
    n = len(s)
    
    # Create DP array
    dp = [0] * (n + 1)
    dp[0] = 1  # Empty string
    dp[1] = 1  # Single character
    
    # Fill DP array
    for i in range(2, n + 1):
        # Single digit
        if s[i-1] != '0':
            dp[i] += dp[i-1]
        
        # Two digits
        two_digits = int(s[i-2:i])
        if 10 <= two_digits <= 26:
            dp[i] += dp[i-2]
    
    return dp[n]

def num_decodings_optimized(s):
    """
    Space-optimized version of Decode Ways
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not s or s[0] == '0':
        return 0
    
    n = len(s)
    
    # Use two variables instead of array
    prev2 = 1  # dp[i-2]
    prev1 = 1  # dp[i-1]
    
    # Fill DP array
    for i in range(2, n + 1):
        current = 0
        
        # Single digit
        if s[i-1] != '0':
            current += prev1
        
        # Two digits
        two_digits = int(s[i-2:i])
        if 10 <= two_digits <= 26:
            current += prev2
        
        prev2, prev1 = prev1, current
    
    return prev1

def num_decodings_with_solution(s):
    """
    Decode Ways with solution reconstruction
    Time Complexity: O(n)
    Space Complexity: O(n)
    """
    if not s or s[0] == '0':
        return 0, []
    
    n = len(s)
    
    # Create DP array and solution array
    dp = [0] * (n + 1)
    solutions = [[] for _ in range(n + 1)]
    
    dp[0] = 1
    solutions[0] = [""]
    
    dp[1] = 1
    solutions[1] = [s[0]]
    
    # Fill DP array
    for i in range(2, n + 1):
        current_solutions = []
        
        # Single digit
        if s[i-1] != '0':
            dp[i] += dp[i-1]
            for sol in solutions[i-1]:
                current_solutions.append(sol + s[i-1])
        
        # Two digits
        two_digits = int(s[i-2:i])
        if 10 <= two_digits <= 26:
            dp[i] += dp[i-2]
            for sol in solutions[i-2]:
                current_solutions.append(sol + " " + s[i-2:i])
        
        solutions[i] = current_solutions
    
    return dp[n], solutions[n]

# Example usage
if __name__ == "__main__":
    # Test case
    s = "226"
    
    # Get number of decodings
    num_ways = num_decodings(s)
    print(f"Number of decodings: {num_ways}")
    
    # Get number of decodings using optimized version
    num_ways_opt = num_decodings_optimized(s)
    print(f"\nNumber of decodings (optimized): {num_ways_opt}")
    
    # Get all possible decodings
    num_ways, decodings = num_decodings_with_solution(s)
    print(f"\nNumber of decodings: {num_ways}")
    print("Possible decodings:")
    for decoding in decodings:
        print(decoding)
```

## Explanation
The Decode Ways problem is to determine the number of ways to decode a string of digits into letters, where 'A' is 1, 'B' is 2, ..., 'Z' is 26.

### How it works:
1. Create DP array initialized to 0
2. Set base cases (empty string and single character)
3. Fill DP array in bottom-up manner:
   - For each position
   - Check single digit (if not '0')
   - Check two digits (if between 10 and 26)
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
- Text processing
- Data compression
- Error correction
- Cryptography
- Message encoding

### Advantages:
- Optimal solution
- Can reconstruct solutions
- Works for any string
- Can be optimized
- Useful in many domains

### Disadvantages:
- Linear time complexity
- May be memory intensive
- Not suitable for very long strings
- Requires additional space
- May be slow for some cases 