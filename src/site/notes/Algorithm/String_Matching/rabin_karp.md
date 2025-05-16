---
{"dg-publish":true,"permalink":"/algorithm/string-matching/rabin-karp/"}
---

```python
class RabinKarp:
    def __init__(self, base=256, prime=101):
        """
        Rabin-Karp Algorithm
        Time Complexity: O(n+m) average, O(nm) worst case
        Space Complexity: O(1)
        where n is text length and m is pattern length
        """
        self.base = base  # Number of characters in input alphabet
        self.prime = prime  # A prime number
    
    def find_pattern(self, text, pattern):
        """Find all occurrences of pattern in text"""
        n = len(text)
        m = len(pattern)
        occurrences = []
        
        # Calculate hash value for pattern and first window of text
        pattern_hash = self._hash(pattern, m)
        text_hash = self._hash(text, m)
        
        # Slide pattern over text one by one
        for s in range(n - m + 1):
            # Check hash values
            if pattern_hash == text_hash:
                # If hash values match, check characters one by one
                if self._check_match(text, pattern, s):
                    occurrences.append(s)
            
            # Calculate hash value for next window of text
            if s < n - m:
                text_hash = self._rehash(text, text_hash, s, m)
        
        return occurrences
    
    def _hash(self, string, length):
        """Calculate hash value for string of given length"""
        hash_value = 0
        for i in range(length):
            hash_value = (self.base * hash_value + ord(string[i])) % self.prime
        return hash_value
    
    def _rehash(self, text, old_hash, start, length):
        """Calculate hash value for next window"""
        # Remove leading digit
        old_hash = (old_hash - ord(text[start]) * pow(self.base, length-1, self.prime)) % self.prime
        # Add trailing digit
        new_hash = (old_hash * self.base + ord(text[start + length])) % self.prime
        return new_hash
    
    def _check_match(self, text, pattern, start):
        """Check if pattern matches text starting at position start"""
        for i in range(len(pattern)):
            if text[start + i] != pattern[i]:
                return False
        return True

class RabinKarpWithMultiplePatterns(RabinKarp):
    def __init__(self, base=256, prime=101):
        super().__init__(base, prime)
        self.pattern_hashes = {}
    
    def find_patterns(self, text, patterns):
        """Find all occurrences of multiple patterns in text"""
        n = len(text)
        m = len(patterns[0])  # Assume all patterns have same length
        occurrences = {pattern: [] for pattern in patterns}
        
        # Calculate hash values for all patterns
        for pattern in patterns:
            self.pattern_hashes[pattern] = self._hash(pattern, m)
        
        # Calculate hash value for first window of text
        text_hash = self._hash(text, m)
        
        # Slide window over text one by one
        for s in range(n - m + 1):
            # Check hash values for all patterns
            for pattern in patterns:
                if self.pattern_hashes[pattern] == text_hash:
                    if self._check_match(text, pattern, s):
                        occurrences[pattern].append(s)
            
            # Calculate hash value for next window of text
            if s < n - m:
                text_hash = self._rehash(text, text_hash, s, m)
        
        return occurrences

# Example usage
if __name__ == "__main__":
    # Create sample text and pattern
    text = "AABAACAADAABAABA"
    pattern = "AABA"
    
    # Basic Rabin-Karp
    rk = RabinKarp()
    occurrences = rk.find_pattern(text, pattern)
    print("Pattern occurrences (basic):", occurrences)
    
    # Rabin-Karp with multiple patterns
    rk_multi = RabinKarpWithMultiplePatterns()
    patterns = ["AABA", "ACA", "BA"]
    occurrences_multi = rk_multi.find_patterns(text, patterns)
    print("\nPattern occurrences (multiple):")
    for pattern, positions in occurrences_multi.items():
        print(f"{pattern}: {positions}")
```

## Explanation
Rabin-Karp algorithm is a string matching algorithm that uses hashing to find patterns in text.

### How it works:
1. Calculate hash value for pattern and first window of text
2. Slide pattern over text one by one
3. If hash values match, check characters one by one
4. Calculate hash value for next window using rolling hash
5. Repeat until end of text

### Time Complexity:
- Best Case: O(n+m)
- Average Case: O(n+m)
- Worst Case: O(nm)

### Space Complexity: O(1)

### Applications:
- Text search
- Plagiarism detection
- DNA sequence matching
- Multiple pattern matching
- Large text processing

### Advantages:
- Efficient average case
- Multiple pattern matching
- Rolling hash optimization
- Simple implementation
- Works for any pattern

### Disadvantages:
- Poor worst-case performance
- Hash collisions possible
- Sensitive to hash function
- Requires prime number
- May need multiple checks
