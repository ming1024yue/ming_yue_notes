---
{"dg-publish":true,"permalink":"/algorithm/string-matching/suffix-arrays/"}
---

```python
class SuffixArray:
    def __init__(self):
        """
        Suffix Arrays
        Time Complexity: O(n log n) construction, O(m log n) search
        Space Complexity: O(n)
        where n is text length and m is pattern length
        """
        self.suffix_array = None
        self.text = None
    
    def build(self, text):
        """Build suffix array for text"""
        self.text = text
        n = len(text)
        
        # Create array of suffixes with their starting indices
        suffixes = [(text[i:], i) for i in range(n)]
        
        # Sort suffixes lexicographically
        suffixes.sort()
        
        # Extract starting indices
        self.suffix_array = [suffix[1] for suffix in suffixes]
    
    def find_pattern(self, pattern):
        """Find all occurrences of pattern in text"""
        if self.suffix_array is None:
            raise ValueError("Suffix array not built")
        
        n = len(self.text)
        m = len(pattern)
        occurrences = []
        
        # Binary search for pattern
        left = 0
        right = n - 1
        while left <= right:
            mid = (left + right) // 2
            suffix = self.text[self.suffix_array[mid]:self.suffix_array[mid] + m]
            
            if pattern == suffix:
                # Found a match, search for all occurrences
                occurrences.append(self.suffix_array[mid])
                # Search left
                i = mid - 1
                while i >= 0 and self.text[self.suffix_array[i]:self.suffix_array[i] + m] == pattern:
                    occurrences.append(self.suffix_array[i])
                    i -= 1
                # Search right
                i = mid + 1
                while i < n and self.text[self.suffix_array[i]:self.suffix_array[i] + m] == pattern:
                    occurrences.append(self.suffix_array[i])
                    i += 1
                break
            elif pattern < suffix:
                right = mid - 1
            else:
                left = mid + 1
        
        return sorted(occurrences)

class SuffixArrayWithLCP(SuffixArray):
    def __init__(self):
        super().__init__()
        self.lcp = None
    
    def build(self, text):
        """Build suffix array with LCP array"""
        super().build(text)
        self._build_lcp()
    
    def _build_lcp(self):
        """Build longest common prefix array"""
        n = len(self.text)
        self.lcp = [0] * n
        
        # Compute LCP array
        for i in range(1, n):
            # Find length of common prefix between adjacent suffixes
            j = 0
            while (self.suffix_array[i] + j < n and 
                   self.suffix_array[i-1] + j < n and 
                   self.text[self.suffix_array[i] + j] == self.text[self.suffix_array[i-1] + j]):
                j += 1
            self.lcp[i] = j
    
    def find_pattern(self, pattern):
        """Find all occurrences of pattern in text using LCP array"""
        if self.suffix_array is None or self.lcp is None:
            raise ValueError("Suffix array or LCP array not built")
        
        n = len(self.text)
        m = len(pattern)
        occurrences = []
        
        # Binary search with LCP optimization
        left = 0
        right = n - 1
        lcp_left = lcp_right = 0
        
        while left <= right:
            mid = (left + right) // 2
            # Use LCP to skip common prefix
            min_lcp = min(lcp_left, lcp_right)
            suffix = self.text[self.suffix_array[mid] + min_lcp:self.suffix_array[mid] + m]
            pattern_suffix = pattern[min_lcp:]
            
            if pattern_suffix == suffix:
                # Found a match, search for all occurrences
                occurrences.append(self.suffix_array[mid])
                # Search left
                i = mid - 1
                while i >= 0 and self.text[self.suffix_array[i]:self.suffix_array[i] + m] == pattern:
                    occurrences.append(self.suffix_array[i])
                    i -= 1
                # Search right
                i = mid + 1
                while i < n and self.text[self.suffix_array[i]:self.suffix_array[i] + m] == pattern:
                    occurrences.append(self.suffix_array[i])
                    i += 1
                break
            elif pattern_suffix < suffix:
                right = mid - 1
                lcp_right = min_lcp
            else:
                left = mid + 1
                lcp_left = min_lcp
        
        return sorted(occurrences)

# Example usage
if __name__ == "__main__":
    # Create sample text and pattern
    text = "AABAACAADAABAABA"
    pattern = "AABA"
    
    # Basic suffix array
    sa = SuffixArray()
    sa.build(text)
    occurrences = sa.find_pattern(pattern)
    print("Pattern occurrences (basic):", occurrences)
    
    # Suffix array with LCP
    sa_lcp = SuffixArrayWithLCP()
    sa_lcp.build(text)
    occurrences_lcp = sa_lcp.find_pattern(pattern)
    print("\nPattern occurrences (with LCP):", occurrences_lcp)
```

## Explanation
Suffix Arrays is a string matching algorithm that uses a sorted array of all suffixes of a text to efficiently find patterns.

### How it works:
1. Build suffix array by sorting all suffixes
2. Use binary search to find pattern
3. Record all occurrences of pattern
4. Optionally use LCP array for optimization

### Time Complexity:
- Construction: O(n log n)
- Search: O(m log n)
- Total: O(n log n + m log n)

### Space Complexity: O(n)

### Applications:
- Text search
- Pattern matching
- Genome analysis
- Data compression
- Bioinformatics

### Advantages:
- Efficient search time
- Flexible for multiple patterns
- Works for any pattern
- Good for large texts
- Supports various operations

### Disadvantages:
- High construction time
- Memory intensive
- Complex implementation
- Not suitable for dynamic texts
- Requires preprocessing
