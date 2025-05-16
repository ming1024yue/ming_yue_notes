---
{"dg-publish":true,"permalink":"/algorithm/string-matching/naive-algorithm/"}
---

```python
class NaiveStringMatcher:
    def __init__(self):
        """
        Naive String Matching Algorithm
        Time Complexity: O((n-m+1)*m)
        Space Complexity: O(1)
        where n is text length and m is pattern length
        """
        pass
    
    def find_pattern(self, text, pattern):
        """Find all occurrences of pattern in text"""
        n = len(text)
        m = len(pattern)
        occurrences = []
        
        # Slide pattern over text
        for s in range(n - m + 1):
            # Check if pattern matches at position s
            if self._check_match(text, pattern, s):
                occurrences.append(s)
        
        return occurrences
    
    def _check_match(self, text, pattern, start):
        """Check if pattern matches text starting at position start"""
        for i in range(len(pattern)):
            if text[start + i] != pattern[i]:
                return False
        return True

class NaiveStringMatcherWithEarlyExit(NaiveStringMatcher):
    def _check_match(self, text, pattern, start):
        """Check if pattern matches text starting at position start with early exit"""
        for i in range(len(pattern)):
            if text[start + i] != pattern[i]:
                return False
            # Early exit if remaining text is shorter than pattern
            if start + i + 1 >= len(text):
                return False
        return True

# Example usage
if __name__ == "__main__":
    # Create sample text and pattern
    text = "AABAACAADAABAABA"
    pattern = "AABA"
    
    # Basic naive matcher
    nsm = NaiveStringMatcher()
    occurrences = nsm.find_pattern(text, pattern)
    print("Pattern occurrences (basic):", occurrences)
    
    # Naive matcher with early exit
    nsm_early = NaiveStringMatcherWithEarlyExit()
    occurrences_early = nsm_early.find_pattern(text, pattern)
    print("Pattern occurrences (early exit):", occurrences_early)
```

## Explanation
Naive String Matching algorithm is the simplest string matching algorithm that checks for all possible positions of the pattern in the text.

### How it works:
1. Slide pattern over text one position at a time
2. At each position, check if pattern matches
3. If match found, record the position
4. Continue until end of text

### Time Complexity:
- Best Case: O(n) when pattern not found
- Average Case: O((n-m+1)*m)
- Worst Case: O((n-m+1)*m)

### Space Complexity: O(1)

### Applications:
- Simple text search
- Basic pattern matching
- Learning string matching
- Small text processing
- Educational purposes

### Advantages:
- Simple to implement
- Easy to understand
- No preprocessing needed
- Works for any pattern
- No extra memory required

### Disadvantages:
- Inefficient for large texts
- Poor worst-case performance
- Not suitable for multiple patterns
- No optimization for repeated patterns
- Sensitive to pattern length
