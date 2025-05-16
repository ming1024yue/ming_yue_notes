---
{"dg-publish":true,"permalink":"/algorithm/string-matching/knuth-morris-pratt/"}
---

```python
class KMP:
    def __init__(self):
        """
        Knuth-Morris-Pratt Algorithm
        Time Complexity: O(n+m)
        Space Complexity: O(m)
        where n is text length and m is pattern length
        """
        pass
    
    def find_pattern(self, text, pattern):
        """Find all occurrences of pattern in text"""
        n = len(text)
        m = len(pattern)
        occurrences = []
        
        # Compute prefix function
        prefix = self._compute_prefix(pattern)
        
        # Initialize variables
        q = 0  # Number of characters matched
        
        # Scan text
        for i in range(n):
            while q > 0 and pattern[q] != text[i]:
                q = prefix[q-1]
            
            if pattern[q] == text[i]:
                q += 1
            
            if q == m:
                occurrences.append(i - m + 1)
                q = prefix[q-1]
        
        return occurrences
    
    def _compute_prefix(self, pattern):
        """Compute prefix function for pattern"""
        m = len(pattern)
        prefix = [0] * m
        k = 0
        
        for q in range(1, m):
            while k > 0 and pattern[k] != pattern[q]:
                k = prefix[k-1]
            
            if pattern[k] == pattern[q]:
                k += 1
            
            prefix[q] = k
        
        return prefix

class KMPWithMultiplePatterns(KMP):
    def __init__(self):
        super().__init__()
        self.patterns = {}
    
    def add_pattern(self, pattern):
        """Add a pattern to the matcher"""
        self.patterns[pattern] = self._compute_prefix(pattern)
    
    def find_patterns(self, text):
        """Find all occurrences of all patterns in text"""
        n = len(text)
        occurrences = {pattern: [] for pattern in self.patterns}
        
        # For each pattern
        for pattern, prefix in self.patterns.items():
            m = len(pattern)
            q = 0  # Number of characters matched
            
            # Scan text
            for i in range(n):
                while q > 0 and pattern[q] != text[i]:
                    q = prefix[q-1]
                
                if pattern[q] == text[i]:
                    q += 1
                
                if q == m:
                    occurrences[pattern].append(i - m + 1)
                    q = prefix[q-1]
        
        return occurrences

# Example usage
if __name__ == "__main__":
    # Create sample text and pattern
    text = "AABAACAADAABAABA"
    pattern = "AABA"
    
    # Basic KMP
    kmp = KMP()
    occurrences = kmp.find_pattern(text, pattern)
    print("Pattern occurrences (basic):", occurrences)
    
    # KMP with multiple patterns
    kmp_multi = KMPWithMultiplePatterns()
    patterns = ["AABA", "ACA", "BA"]
    for p in patterns:
        kmp_multi.add_pattern(p)
    occurrences_multi = kmp_multi.find_patterns(text)
    print("\nPattern occurrences (multiple):")
    for pattern, positions in occurrences_multi.items():
        print(f"{pattern}: {positions}")
```

## Explanation
Knuth-Morris-Pratt algorithm is a string matching algorithm that uses a prefix function to avoid unnecessary comparisons.

### How it works:
1. Compute prefix function for pattern
2. Initialize number of characters matched
3. Scan text character by character
4. Use prefix function to skip unnecessary comparisons
5. Record positions where pattern matches

### Time Complexity:
- Best Case: O(n+m)
- Average Case: O(n+m)
- Worst Case: O(n+m)

### Space Complexity: O(m)

### Applications:
- Text search
- DNA sequence matching
- Pattern recognition
- String processing
- Bioinformatics

### Advantages:
- Linear time complexity
- No backtracking
- Efficient for large texts
- Works for any pattern
- Predictable performance

### Disadvantages:
- Extra preprocessing
- Memory overhead
- Complex implementation
- Not suitable for multiple patterns
- Sensitive to pattern length
