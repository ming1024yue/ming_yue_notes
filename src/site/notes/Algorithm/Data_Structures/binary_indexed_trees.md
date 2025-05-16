---
{"dg-publish":true,"permalink":"/algorithm/data-structures/binary-indexed-trees/"}
---

```python
class BinaryIndexedTree:
    def __init__(self, arr):
        """
        Initialize binary indexed tree with array
        """
        self.n = len(arr)
        self.tree = [0] * (self.n + 1)
        
        # Build tree
        for i in range(self.n):
            self.update(i, arr[i])

    def update(self, index, delta):
        """
        Update value at index
        Time Complexity: O(log n)
        """
        index += 1  # 1-based indexing
        while index <= self.n:
            self.tree[index] += delta
            index += index & -index

    def query(self, index):
        """
        Query prefix sum up to index
        Time Complexity: O(log n)
        """
        index += 1  # 1-based indexing
        res = 0
        while index > 0:
            res += self.tree[index]
            index -= index & -index
        return res

    def range_query(self, l, r):
        """
        Query sum in range [l, r]
        Time Complexity: O(log n)
        """
        return self.query(r) - self.query(l - 1)

    def get_array(self):
        """
        Get current array
        Time Complexity: O(n log n)
        """
        arr = [0] * self.n
        for i in range(self.n):
            arr[i] = self.query(i) - (self.query(i - 1) if i > 0 else 0)
        return arr

# Example usage
if __name__ == "__main__":
    arr = [1, 3, 5, 7, 9, 11]
    bit = BinaryIndexedTree(arr)
    
    print("Initial array:", bit.get_array())
    print("Prefix sum up to index 3:", bit.query(3))
    print("Sum in range [1, 4]:", bit.range_query(1, 4))
    
    # Update value
    bit.update(2, 10 - 5)  # Change value at index 2 from 5 to 10
    print("After update:", bit.get_array())
    print("Prefix sum up to index 3:", bit.query(3))
    print("Sum in range [1, 4]:", bit.range_query(1, 4))
    
    # Query different ranges
    print("Sum in range [0, 5]:", bit.range_query(0, 5))
    print("Sum in range [2, 4]:", bit.range_query(2, 4))
```

## Explanation
A Binary Indexed Tree (Fenwick Tree) is a data structure that can efficiently update elements and calculate prefix sums in a table of numbers.

### Operations:
1. Update: Change value at index
2. Query: Get prefix sum up to index
3. Range Query: Get sum in range
4. Get Array: Get current array

### Time Complexities:
- Update: O(log n)
- Query: O(log n)
- Range Query: O(log n)
- Get Array: O(n log n)
- Build: O(n log n)

### Space Complexity: O(n)

### Applications:
- Prefix sum queries
- Range sum queries
- Dynamic programming
- Counting inversions
- Range updates
- Point queries

### Advantages:
- Efficient updates
- Efficient queries
- Memory efficient
- Simple implementation
- Useful for many problems

### Disadvantages:
- Limited operations
- Not suitable for all problems
- Can be slow for some operations
- Complex to understand
- Memory overhead 