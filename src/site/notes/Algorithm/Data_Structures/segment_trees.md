---
{"dg-publish":true,"permalink":"/algorithm/data-structures/segment-trees/"}
---

```python
class SegmentTree:
    def __init__(self, arr):
        """
        Initialize segment tree with array
        """
        self.n = len(arr)
        self.size = 1
        while self.size < self.n:
            self.size <<= 1
        self.tree = [0] * (2 * self.size)
        
        # Build tree
        for i in range(self.n):
            self.tree[self.size + i] = arr[i]
        for i in range(self.size - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index, value):
        """
        Update value at index
        Time Complexity: O(log n)
        """
        index += self.size
        self.tree[index] = value
        index >>= 1
        
        while index >= 1:
            self.tree[index] = self.tree[2 * index] + self.tree[2 * index + 1]
            index >>= 1

    def query(self, l, r):
        """
        Query sum in range [l, r)
        Time Complexity: O(log n)
        """
        res = 0
        l += self.size
        r += self.size
        
        while l < r:
            if l % 2 == 1:
                res += self.tree[l]
                l += 1
            if r % 2 == 1:
                r -= 1
                res += self.tree[r]
            l >>= 1
            r >>= 1
            
        return res

    def get_array(self):
        """
        Get current array
        Time Complexity: O(n)
        """
        return self.tree[self.size:self.size + self.n]

# Example usage
if __name__ == "__main__":
    arr = [1, 3, 5, 7, 9, 11]
    st = SegmentTree(arr)
    
    print("Initial array:", st.get_array())
    print("Sum in range [1, 4):", st.query(1, 4))
    
    # Update value
    st.update(2, 10)
    print("After update:", st.get_array())
    print("Sum in range [1, 4):", st.query(1, 4))
    
    # Query different ranges
    print("Sum in range [0, 6):", st.query(0, 6))
    print("Sum in range [2, 5):", st.query(2, 5))
```

## Explanation
A segment tree is a data structure that allows efficient range queries and updates on an array. It supports operations like finding the sum, minimum, maximum, etc. of elements in a range.

### Operations:
1. Update: Change value at index
2. Query: Get sum in range
3. Get Array: Get current array

### Time Complexities:
- Update: O(log n)
- Query: O(log n)
- Get Array: O(n)
- Build: O(n)

### Space Complexity: O(n)

### Applications:
- Range sum queries
- Range minimum queries
- Range maximum queries
- Range GCD queries
- Range XOR queries
- Dynamic programming

### Advantages:
- Efficient range queries
- Efficient updates
- Flexible
- Memory efficient
- Useful for many problems

### Disadvantages:
- Complex implementation
- Memory overhead
- Not suitable for all problems
- Can be slow for some operations
- Limited operations 