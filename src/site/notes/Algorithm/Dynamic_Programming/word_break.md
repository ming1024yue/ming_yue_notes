---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/word-break/"}
---

```python
def word_break(s, word_dict):
    """
    Word Break Problem using Dynamic Programming
    Time Complexity: O(n³)
    Space Complexity: O(n)
    where n is length of string
    """
    n = len(s)
    
    # Create DP array
    dp = [False] * (n + 1)
    dp[0] = True  # Empty string
    
    # Fill DP array
    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_dict:
                dp[i] = True
                break
    
    return dp[n]

def word_break_with_solution(s, word_dict):
    """
    Word Break with solution reconstruction
    Time Complexity: O(n³)
    Space Complexity: O(n²)
    """
    n = len(s)
    
    # Create DP array and solution array
    dp = [False] * (n + 1)
    solution = [[] for _ in range(n + 1)]
    dp[0] = True
    
    # Fill DP array
    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_dict:
                dp[i] = True
                solution[i].append(j)
    
    # Reconstruct solutions
    def backtrack(pos):
        if pos == 0:
            return [""]
        result = []
        for prev_pos in solution[pos]:
            for prev_solution in backtrack(prev_pos):
                if prev_solution:
                    result.append(prev_solution + " " + s[prev_pos:pos])
                else:
                    result.append(s[prev_pos:pos])
        return result
    
    if dp[n]:
        return backtrack(n)
    return []

def word_break_optimized(s, word_dict):
    """
    Optimized Word Break using Trie
    Time Complexity: O(n²)
    Space Complexity: O(n + m)
    where m is total length of words in dictionary
    """
    # Build trie
    trie = {}
    for word in word_dict:
        node = trie
        for char in word:
            if char not in node:
                node[char] = {}
            node = node[char]
        node['

## Explanation
The Word Break problem is to determine if a given string can be segmented into a space-separated sequence of dictionary words.

### How it works:
1. Create DP array initialized to False
2. Set base case (empty string)
3. Fill DP array in bottom-up manner:
   - For each position
   - Check all possible previous positions
   - If previous position is True and substring is in dictionary, set current to True
4. Return value at last position

### Time Complexity:
- Original: O(n³)
- Optimized: O(n²)

### Space Complexity:
- Original: O(n)
- With Solution: O(n²)
- Optimized: O(n + m)

### Applications:
- Text processing
- Spell checking
- Natural language processing
- Compiler design
- String matching

### Advantages:
- Optimal solution
- Can find all solutions
- Works for any dictionary
- Can be optimized
- Useful in many domains

### Disadvantages:
- Cubic time in basic version
- May be memory intensive
- Not suitable for very long strings
- Requires dictionary
- May be slow for some cases ] = True  # Mark end of word
    
    n = len(s)
    dp = [False] * (n + 1)
    dp[0] = True
    
    # Fill DP array
    for i in range(n):
        if dp[i]:
            node = trie
            for j in range(i, n):
                if s[j] not in node:
                    break
                node = node[s[j]]
                if '

## Explanation
The Word Break problem is to determine if a given string can be segmented into a space-separated sequence of dictionary words.

### How it works:
1. Create DP array initialized to False
2. Set base case (empty string)
3. Fill DP array in bottom-up manner:
   - For each position
   - Check all possible previous positions
   - If previous position is True and substring is in dictionary, set current to True
4. Return value at last position

### Time Complexity:
- Original: O(n³)
- Optimized: O(n²)

### Space Complexity:
- Original: O(n)
- With Solution: O(n²)
- Optimized: O(n + m)

### Applications:
- Text processing
- Spell checking
- Natural language processing
- Compiler design
- String matching

### Advantages:
- Optimal solution
- Can find all solutions
- Works for any dictionary
- Can be optimized
- Useful in many domains

### Disadvantages:
- Cubic time in basic version
- May be memory intensive
- Not suitable for very long strings
- Requires dictionary
- May be slow for some cases  in node:
                    dp[j+1] = True
    
    return dp[n]

# Example usage
if __name__ == "__main__":
    # Test case
    s = "leetcode"
    word_dict = {"leet", "code"}
    
    # Check if word can be broken
    can_break = word_break(s, word_dict)
    print(f"Can break word: {can_break}")
    
    # Get all possible breaks
    solutions = word_break_with_solution(s, word_dict)
    print("\nPossible breaks:")
    for solution in solutions:
        print(solution)
    
    # Check using optimized version
    can_break_opt = word_break_optimized(s, word_dict)
    print(f"\nCan break word (optimized): {can_break_opt}")
```

## Explanation
The Word Break problem is to determine if a given string can be segmented into a space-separated sequence of dictionary words.

### How it works:
1. Create DP array initialized to False
2. Set base case (empty string)
3. Fill DP array in bottom-up manner:
   - For each position
   - Check all possible previous positions
   - If previous position is True and substring is in dictionary, set current to True
4. Return value at last position

### Time Complexity:
- Original: O(n³)
- Optimized: O(n²)

### Space Complexity:
- Original: O(n)
- With Solution: O(n²)
- Optimized: O(n + m)

### Applications:
- Text processing
- Spell checking
- Natural language processing
- Compiler design
- String matching

### Advantages:
- Optimal solution
- Can find all solutions
- Works for any dictionary
- Can be optimized
- Useful in many domains

### Disadvantages:
- Cubic time in basic version
- May be memory intensive
- Not suitable for very long strings
- Requires dictionary
- May be slow for some cases 